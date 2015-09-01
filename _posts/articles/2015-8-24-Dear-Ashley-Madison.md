---
layout: post
title: Dear Ashley
excerpt: Insights into cheaters in the wake of the Ashley Madison hack
categories: articles
tags: [Data Science, Graphs, lols]
comments: true
share: true
---

Dear Ashley,

It seems you're run into some trouble lately. I found a few of the items you left behind. Please take solace in my leaving them here on your doorstep.

First, let's start with the boring stuff.

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/ethnicity_gender_count.jpeg"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/ethnicity_gender_count.jpeg" alt="image"></a>
	<figcaption>Ethnic breakdown across genders</figcaption>
</figure>

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/male_weight_nyc_v_world.png.jpeg"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/male_weight_nyc_v_world.png.jpeg" alt="image"></a>
	<figcaption>AM Body weights in NYC vs the World</figcaption>
</figure>

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/bodytype_bmi_violin_men.jpeg"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/bodytype_bmi_violin_men.jpeg" alt="image"></a>
	<figcaption>BMI compared to self-selected body type across genders</figcaption>
</figure>

Here, the fatter the section, the more people claiming that body type while having the BMI on the y-axis. I know the above figure isn't very clear, but I just like the way it looks.


### Alabama is probably still special in other ways

I think at this point everybody has seen the graph showing Alabama as the most cheating-ist state according to the Ashley Madison credit card transaction data. I have the same data and just could not duplicate those results.

Using the total amount of sales for each state, I divided by the population of the state to find the sales per capita.

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/sales_per_capita.jpeg"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/sales_per_capita.jpeg" alt="image"></a>
	<figcaption>DC: where people go to spend the most money on corrupt behavior</figcaption>
</figure>

Kind of hilarious that the home of all the politicians is the place spending the most money on cheating. I'm sure there's an analogy in there somewhere for how they treat the general public.

This still doesn't take into account differences in economies. So instead I reorganized the data to show the total number of users per capita per state. I removed duplicate users beforehand to account for people that make multiple transactions. I did this by removing duplicates where the first name, last name, and last four digits of the credit card number were the same.

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/users_per_capita.jpeg"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/users_per_capita.jpeg" alt="image"></a>
	<figcaption>Chris Christie is the governor of New Jersey and wants to move to DC? Coincidence? I think not.</figcaption>
</figure>

The order changes a little, but everything is still generally in the same spot. It's also important to note that these plots just take into account the number of people who paid money to Ashley Madison. It doesn't refer to the total number of people who signed up and were just browsing around.

Since there is a discrepancy with the other author who posted that Alabama-oriented graph, I'm posting my code in case anybody wishes to critique it.

{% highlight python %}
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import glob
import seaborn as sns

#create full transaction dataframe
path =r'CreditCardTransactions/'
allFiles = glob.glob(path + "/*.csv")

frame = pd.DataFrame()
list_ = []
for file_ in allFiles:
    try:
        df = pd.read_csv(file_,index_col=None, header=None, skiprows=3, quotechar='"')
        list_.append(df)
    except Exception as e:
        '''adding missing quote mark at the end manually'''
        print e
        print file_
frame = pd.concat(list_)

column_names = ["ACCOUNT","ACCOUNT NAME","AMOUNT","AUTH CODE","AVS","BRAND","CARD ENDING","CVD","FIRST NAME","LAST NAME","MERCHANT TRANS. ID","OPTION CODE","DATE","TXN ID","CONF. NO.","ERROR CODE","AUTH TYPE","TYPE","TXT_CITY","CTXT_COUNTRY","CTXT_EMAIL","CTXT_PHONE","CTXT_STATE","CTXT_ADDR1","CTXT_ADDR2","CZIP","CCONSUMER_IP",""]
frame.columns = column_names
frame.AMOUNT = frame.AMOUNT.convert_objects(convert_numeric=True)

states_by_amount = frame[frame.CTXT_COUNTRY == 'US'].groupby('CTXT_STATE').AMOUNT.sum()

# using no_dupes dataframe so just getting individual users rather than duplicates
no_dupes = frame.drop_duplicates(subset=['FIRST NAME', 'LAST NAME', 'CARD ENDING'])
states_by_count = no_dupes[no_dupes.CTXT_COUNTRY == 'US'].CTXT_STATE.value_counts()

abbr = pd.DataFrame.from_csv('state_populations.csv', index_col=None)
both = pd.merge(states_by_amount.reset_index(), states_by_count.reset_index(), left_on='CTXT_STATE', right_on='index').drop('index', axis=1)
both.columns = ['abbreviation', 'amount', 'number_of_users']
percapita = pd.merge(abbr, both)
percapita['amount_pc'] = percapita.amount / percapita.population
percapita['users_pc'] = percapita.number_of_users / percapita.population

# amounts per state per capita
sns.barplot(x='abbreviation', y='amount_pc', data=percapita.sort('amount_pc', ascending=False))
plt.tight_layout()
plt.xlabel('State')
plt.ylabel('Sales per Capita')

# number of users per state per capita
sns.barplot(x='abbreviation', y='users_pc', data=percapita.sort('users_pc', ascending=False))
plt.tight_layout()
plt.xlabel('State')
plt.ylabel('Number of Users per Capita')

{% endhighlight %}

### Hypocrites
A few news organizations have given Ashley Madison credit for being gay inclusive. What they seem to be missing is that Ashley Madison provides the following sign up options:
* Attached Male seeking Females
* Attached Female seeking Males
* Single Male seeking Females
* Single Female seeking Males
* Male seeking Males
* Female seeking Females
Is the assumption that if you are gay and on Ashley Madison then you must be cheating? I don't see why AM can't provide equitable options across preferences.

I'm sure everyone has heard the right-wing bag of nuts screaming how allowing gay couples to marry will ruin the sanctity of marriage. To that, I proudly present the following:

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/relationship_to_seeking"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/relationship_to_seeking" alt="image"></a>
	<figcaption>Books will rot your brain.</figcaption>
</figure>

This is a plot comparing the hetero (just the attached ones) and gay user base of AM as a percentage of their respective US populations. I think it's pretty clear who isn't very good at complying with their marital contract.

### Kinkiest state in the USA
I also made a tiny yet hilariously revealing interactive map in illustrating which state's users are most interested in a particular kink. Check it out [here](http://gifsandgraphs.com/blog/Ashley-Madison-Kinks/).

[Click here to view the full interactive map](http://bl.ocks.org/potatochip/raw/756ef5fcc9febfb4fd66/)

<figure>
	<a href="http://bl.ocks.org/potatochip/raw/756ef5fcc9febfb4fd66/"><img src="{{ site.baseurl }}/images/2015-8-24-Dear-Ashley/kinks.png" alt="image"></a>
	<figcaption>In the navy</figcaption>
</figure>
