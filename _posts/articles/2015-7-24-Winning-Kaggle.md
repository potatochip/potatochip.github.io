---
layout: post
title: Winning Kaggle
excerpt: A detailed write up of how I 'won' a Kaggle competition.
categories: articles
tags: [Data Science, Kaggle]
comments: true
share: true
permalink: Winning-Kaggle
---
Alright, that title is probably a tiny bit misleading. There are two minor corrections I should make.

1. It was a DrivenData competition, not Kaggle; and
2. I didn’t technically win.

Actually, this is a story about how I lost, won, lost again, thought I finally won, lost one more time, and then redeemed myself. I imagine this is what most data science competitions are like. This was my first.

### TL;DR Version
4th place out of 535 teams.

### Introduction to the Problem
I supposed I should start from the beginning. Once I discovered the competition, I immediately sat down at my computer with Montell Jordan's ['This Is How We Do It'](https://www.youtube.com/watch?v=0hiUuL5uTKc) playing in my head. How wrong I was.

The goal of the competition was to predict the number of Boston restaurant health code violations based on Yelp review data. There were three kinds of violations that you had to predict for. The lowest, level one violations, were far more numerous than the other two types. Essentially, level two was based on whether the restaurant had already been cited for the same violation before. Level three was based on whether the CDC was going to have to call the CDC because of a zombie outbreak.

You also end up having to predict separately for each restaurant for any point in time in the future. Often the competition required predicting multiple inspection dates for a single restaurant. The prediction format ended up looking like this. It looks pretty simple at first glance, but the results needed to be much more complex and layered.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/response_labels.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/response_labels.jpg" alt="image"></a>
	<figcaption>Prediction format</figcaption>
</figure>

Some [SciKit-Learn](http://scikit-learn.org) estimators can handle an array as the output (linear regression), but most cannot. So you have to predict for each violation type separately. I took it a step further and based the model on a different set of features for each kind of violation.

The contest scoring was based on a weighted root mean squared error (RMSE). I decided to use this as well as the accuracy for each violation type for testing at home. I made two super rudimentary baselines to judge my models against.

1. Predicting every restaurant would have zero violations gave accuracy of 22/69/57% for violation levels 1/2/3, respectively. RMSE was 2.14. Not too shabby.
2. Predicting every restaurant would have the mean of each violation type. Accuracy was 10/71/22% and RMSE 1.21.

I made a few simple models in the beginning to get a feel for how the competition and its data worked. It was amazing how often my initial models scored worse than these baselines. It really drove home how inspections have very little to do with how much customers hate the restaurant.

### Losing Before You Even Begin

I had about two weeks left in the competition before I lost.

That probably needs further explanation. I race bicycles when I'm not staring at a computer screen. A lot of people imagine that means I'm doing something like this.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/triathlete.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/triathlete.jpg" alt="image"></a>
	<figcaption>Ugh, triathletes</figcaption>
</figure>

No, this is the type of racing I do.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/real_bike_racing.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/real_bike_racing.jpg" alt="image"></a>
	<figcaption>Real racing</figcaption>
</figure>

Ugh, triathletes. Alright, so with two weeks left in the competition I became tangled up in a pretty bad race crash that required surgery. I could barely move, let alone think while on the pain medication they gave me so I ended up laying in bed watching the end of the competition tick closer and closer.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/inside_hospital.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/inside_hospital.jpg" alt="image"></a>
	<figcaption>Inside hospital</figcaption>
</figure>

That's me trying to remember what R^2 means.

The drugs were so strong that I accidentally escaped from the hospital.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/outside_hospital.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/outside_hospital.jpg" alt="image"></a>
	<figcaption>Outside hospital</figcaption>
</figure>

### How to Win When You Lose

About a week after my surgery I started feeling well enough to take myself off the pain medicine so I could start coding again. The competition was over, but I decided to try and finish my model anyway. I gave myself two weeks to finish from that point. The same amount of time I had left before I dropped out of the competition.

Yelp had given us access to more than just the review text for each restaurant. They had given us a large amount of metadata related to all of the relevant restaurants, reviews, and users. Some of my most important features (according to kbest, at least) turned out to be in the metadata.

I started off by exploring the metadata. I find that looking at graphical representations of data is much more helpful than looking at the raw numbers. It’s just so much easier to visualize what’s going on and to spot outliers.

Histograms are always useful for telling if you need to transform your data because the range of values is too large or skewed.

<figure>
	<a href="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/distplot.jpg"><img src="{{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/distplot.jpg" alt="image"></a>
	<figcaption>Post-tranformation histogram</figcaption>
</figure>

However, if you care more about seeing what’s going on rather than solving the actual problem then my favorite visualizations are coefficient and correlation plots.

[![Neighborhood coefficient plot]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_neighborhood_coefficients_score_lvl_1.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_neighborhood_coefficients_score_lvl_1.jpg)

This is a coefficient plot showing how many more violations the average restaurant will get just on account of the neighborhood it is based in (with a confidence interval  of .95).

My favorite plot from this series was this one showing violations based on the type of cuisine the restaurant served.

[![Cut of cuisine coefficient plot]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_category.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_category.jpg)

One glance at this and you know that you should definitely avoid eating at health markets in Boston.

[Click here to see the full version on your ultra widescreen display]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_type_coefficients_score_lvl_3.png)

Plots!

[![Correlation plot]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/corr_plot.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/corr_plot.jpg)

Plots!!

[![Strip plot of restaurant stars by restaurant ambience]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_stars_by_restaurant_ambience_stripplotscore_lvl_1.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/restaurant_stars_by_restaurant_ambience_stripplotscore_lvl_1.jpg)

More plots!!

[![A whole lotta plots]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/multi_plot.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/multi_plot.jpg)

Plots are fun. Who doesn’t love a good plotting?

#### Feature Engineering

I began cleaning up my data by removing reviews that occurred after each inspection date. I then set to work turning date information into useful features. A few of the date-related features I created were:

+ A delta representing the amount of time that had passed between each review and each inspection date. After all, those reviews from five years ago probably aren't so relevant anymore.
+ A delta representing the amount of time that had passed between each inspection date and the previous inspection date for that restaurant. Often a restaurant would get reviewed again a week after a particularly egregious inspection and magically all of its violations would be corrected.
+ Decomposed inspection dates based on the theory that certain violations are seasonal. For instance, I like to believe the rats only come out in the Summer. This led to features like inspection_quarter and inspection_dayofweek.

I also ended up separating the address of each restaurant into two features consisting of the street name and the zip code. In NYC there are some streets where all the restaurants are just inherently disgusting; I hoped the same would apply to Boston.


In the end, I needed a separate prediction for each restaurant for different future inspection dates. On top of that I had different sets of multiple reviews for each restaurant. I decided to create multiple observations for each inspection consisting of each review for that inspection. With this, I ended up multiplying everything across my dataframe. I went from 30,000 observations to almost 2 million.

#### Text Processing

I can't emphasize enough just how long it takes to process two million reviews on your home computer. Preprocessing, term-frequency inverse-document-frequency (TFIDF), sentiment, and similarity vectors, this was becoming a real drain on my system. It was taking almost 4.5 hours just for the preprocessing alone. I cut this down to just 18 minutes by taking advantage of the multiple cores in my computer with Pool().

{% highlight python %}
def preprocess_pool(df, filename):
    # convert text to categories
    cats = df.review_text.astype('category').cat

    # use multiprocessing to further cut down time
    pool = Pool()
    temp = pool.map(combine_preprocess, cats.categories)
    pool.close()
    pool.join()

    # convert the numerical categorical representation back to the newly processed
    # string representation
    docs = []
    for i in cats.codes:
        docs.append(temp[i])
    df['preprocessed_review_text'] = docs

    # mmm, pickles
    df.to_pickle('pickle_jar/'+filename)
    return df

def combine_preprocess(text):
    b = TextBlob(unicode(text, 'utf8').strip())
    tags = b.tags
    tokens = map(preprocess, tags)
    tokens = filter(None, tokens)
    return ' '.join(tokens)

def preprocess(tagged):
    word = Word(tagged[0])
    if word.isalpha() and word not in stopwords:
        # convert the part of speech tags to the correct format
        tag = penn_to_wn(tagged[1])
        l = word.lemmatize(tag)
    else:
        l = ''
    return l.lower()
{% endhighlight %}

There is nothing more glorious than watching all of the cores on your computer spin up.

[![Cores, cores, cores]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/cores.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/cores.jpg)

I performed the following when preprocessing each review:

+ Converted each word into its individual tokens and made each lowercase
+ Removed stop words and anything that was numeric
+ Lemmatized each word

Lemmatizing normally assumes that you are giving it the noun representation of each word. I went the extra step of getting the part of speech for each word and passing that along as well so that I would have more words lemmatized.


You may have noticed that I also converted my text from strings into a categorical datatype. This way, when there were duplicate reviews, they would be represented by a number and only need to be reviewed a single time. I used this handy trick for most text related feature conversion. When I was making similarity vectors, this cut the processing time from nine hours to one.


I should probably explain what similarity vectors are. I created a vector representation of how many times a word in a review was similar to a specified keyword. Each numerical representation in the vector was a measure of how similar each word was to the keyword. This measure was created with the magical aid of [Gensim](https://radimrehurek.com/gensim/) and the word2vec algorithm.

Boston bases its health code violations on the 1999 Federal Food Code. I read through the entire code and created a list of keywords that I felt represented concepts that a reviewer would be more likely to write about than the original legalese. I ended up with such lovely keywords as:

+ raw
+ rotten
+ sneeze
+ gross

But also some more surprising ones:

+ lights
+ yellow
+ nails
+ jewelry

The Federal Food Code is really concerned with making sure a restaurant is bright enough to see yellow nails and jewelry.

The problem with this whole review-->violation concept and probably one that also exists with my similarity vectors is that some of what is in the food code is not readily observable by yelp reviewers. There is no way a customer will know whether the shellfish box in the back of the walk-in freezer is labeled correctly. We hope that if a restaurant violates observable codes then they will also violate non-observable ones. But beyond that, I also combined these reviews with other metadata-based features to try and cover those other violations.

The metadata was split between boolean and categorical data. The categorical data had to first be converted to a numerical representation in order for it to be useful. I went the next step of turning each numerical representation into a vector of one's and zeros so that the model wouldn't start attaching order to the numerical representation. I even ended up converting some numerical features to categorical under the theory that they should also lose that ordering.

For instance, when looking at the review rating for a restaurant, there is a value of either 1, 2, 3, 4, or 5. I wanted to remove the ordering information because I was working under the assumption that some five star reviews would be shills and some one star reviews would be vindictive. This way, each review category is taken at its face rather than as an increasing variable. It worked out like this:

+ 1-star rating becomes [1, 0, 0, 0, 0]
+ 2-star rating becomes [0, 1, 0, 0, 0]
+ 3-star rating becomes [0, 0, 1, 0, 0]
+ 4-star rating becomes [0, 0, 0, 1, 0]
+ 5-star rating becomes [0, 0, 0, 0, 1]

And so on, and so on.

Using my newly created features, I started seeing some pretty good accuracy scores of around 90%.

### Losing All Over Again

Everybody knows you can’t test your model on the same data that you fit it to. I had been using Scikit-Learn’s train_test_split function to split my data into a train set and a test set. I declared what random integer it should seed with so that I could compare results across different models. When I finally started seeing some good scores, I thought that I should cross-validate what I was seeing across different train-test-split folds of the data. (The reason I hadn’t been doing this from the beginning is because it takes so much time.) In essence, I wanted to make sure that my scores would be good for different cuts of the data. Not just at cut number 42.

I have to say when the first score of 22% accuracy popped up it was pretty disheartening. Soon it was followed by 20%, and then 19%.

At this point, I had been working on this already-over competition for nearly a month. My deadline was fast approaching and I needed some product out of all this use of my time. So I threw my hands in the air and began emergency work on a D3 visualization using a choropleth map of Boston with the following features.

+ Each neighborhood in Boston would be shaded according to what the average number of violations was for that neighborhood.
+ You click the violation level to have the average and shading change accordingly.
+ There is a slider at the bottom allowing a user to select the year and see how the neighborhoods’ scores and shading changed over time.
+ Mouse-over a neighborhood and the name pops up.

[![D3 Choropleth Map]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/d3_visualization.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/d3_visualization.jpg)

It was going to be pretty dope.

Don't bother clicking. That's just a mockup of what it was going to look like when it was finished. The mockup was made in D3 though, so that has to count for something, right? If you’d like to play around with the functioning, yet not-functional, slider then you can browse the code on [bl.ocks](http://bl.ocks.org/potatochip/bc867fc76588234fbcde).

### Just Kidding, I Won

Fortunately(?), about mid-way through making this visualization, my mind started drifting back to my original problem. You see, I have a rough, late-nights-with-hands-thrown-in-the-air, history with any implementation of cross-validation by Scikit-Learn. So I was already suspicious/angry.

I originally discovered a bug where if you enable multiprocessing support with any of SciKit-Learns cross-validation functions then it would cause the kernel to hang and never finish. As I started looking over my code, I noticed that cross_val_score doesn't shuffle your data by default while train_test_split does.

```python
skf = StratifiedKFold(y[i], shuffle=True, n_folds=10)
scores = cross_val_score(clf, X, y[i], cv=skf, verbose=5)
```

Using the above code I was able to get shuffling working, and the accuracy of my model shot up to around 95%. My RMSE was enough to put me at the top of the leaderboard and win the competition (if I hadn't already lost).

### Just Kidding, I Didn’t Win

I had a few days left before my self-imposed deadline, so I spent it trying to further increase my accuracy. It was when I reached 99.9% accuracy that I knew something was wrong.

My multiplied model included each restaurant’s ID in it. Normally, this isn’t a problem and I believe a valuable feature in this particular model. But when you multiply everything over and go from 30,000 observations to 2 million, then the chances of not having identical restaurant IDs included in both your train and test set is pretty slim. Combined with inspection date information, my random forest was noticing the overlap, overfitting, and ending up with nearly 100% accuracy. Oh, to dream.

### Back to the Drawing Board

I needed a new method of dealing with the hierarchical aspect of the problem. In the end I decided on somewhat of a makeshift solution. Rather than have each review be a separate observation, I was going to make each review a feature. Then each review-feature would be ordered according to how close in time it was made to the inspection date. Rather than do everything over I used the pivot feature in Pandas.

With that I was able to go from:

[![Pre-Pivot]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/pre_pivot.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/pre_pivot.jpg)

To:

[![Post-Pivot]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/post_pivot.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/post_pivot.jpg)

I then took each review-feature-matrix and decomposed it into two components using Factor Analysis. For my TFIDF matrix I did something a little different. I decomposed it using Latent Semantic Analysis into 100 components. Technically, I would have probably gotten a better score if I had left my TFIDF matrix as a raw sparse matrix, but combined with all my other features it was just too slow an operation and I was out of time.

The following is the code I wrote to test different review-based features and different decomposition.

{% highlight python %}
def pivot_feature(df, feature, limit=None, decomp='lsi', decomp_features=2, fill='median'):
    # make the large dataframe faster to handle on pivot
    temp = df[['inspection_id', 'enumerated_review_delta'] + [feature]]

    # pivot so that each inspection id only has one observation with each review a feature
    # for that observation
    pivoted_feature = temp.pivot('inspection_id', 'enumerated_review_delta')[feature]


    # pivoting creates a number of empty variables when they have less than the max
    # number of reviews
    if fill == 'median':
        fill_empties = lambda x: x.fillna(x.median())
    elif fill == 'mean':
        fill_empties = lambda x: x.fillna(x.mean())
    elif fill == 0:
        fill_empties = lambda x: x.fillna(0)
    elif fill == 'inter':
        fill_empties = lambda x: x.interpolate()
    elif fill == None:
        fill_empties = lambda x: x
    else:
        raise Exception

    pivoted_feature = pivoted_feature.apply(fill_empties, axis=1)

    if decomp == 'lsi':
        decomposition = TruncatedSVD(decomp_features)
    elif decomp == 'pca':
        decomposition = PCA(decomp_features, whiten=True)
    elif decomp == 'kpca':
        decomposition = KernelPCA(decomp_features)
    elif decomp == 'dict':
        decomposition = DictionaryLearning(decomp_features)
    elif decomp == 'factor':
        decomposition = FactorAnalysis(decomp_features)
    elif decomp == 'ica':
        decomposition = FastICA(decomp_features)
    elif decomp == None:
        pass
    else:
        raise Exception

    if not limit:
        try:
            return decomposition.fit_transform(pivoted_feature)
        except:
            return pivoted_feature
    else:
        try:
            return decomposition.fit_transform(pivoted_feature[[i for i in range(limit)]])
        except:
            return pivoted_feature[[i for i in range(limit)]]
{% endhighlight %}

I also limited the reviews to those that had been created less than a year before the inspection date and created a few new features.

+ A Trustworthiness Index for the writer of each review. It was based in part on how objective the writing was, as well as how long they had been a yelp member and how many reviews they had written.
+ An Anger Index based on how often a user scored a restaurant negatively compared to how frequently they made a review.
+ Similarity vectors v2. I recycled my original similarity vectors into a single number representing how many words in each review achieved a similarity score greater than a specified amount.

I spent a lot of time trying to hand pick which features to use. In the end, I was out of time and just dumped everything into SciKit-Learn's Recursive Feature Elimination CV function and let it prune the features for me.

I also realized that I had been wasting my time focusing on accuracy and reality when the competition solely cared about RMSE. Besides, even if this had been a real client, the City of Boston doesn't need to know exactly how many violations a restaurant will receive. They just need to know that this restaurant will receive a lot, this restaurant will receive a little, and this restaurant will receive none so they can focus their limited resources.

With a single model based on a RandomForestRegressor, I achieved an RMSE of 1.017. Enough to put me in 24th place in the competition.

### But Wait, There's More!

[![But Wait!]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/but_wait.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/but_wait.jpg)

Let's talk about ensembling. Ensembling is seemingly out of place in this world of neural networks and deep learning. It is an amazingly simple concept that can dramatically improve your model. At its foundation, ensembling is just taking the average of several predictions. The easiest way of doing this is making several iterations of a model (that has some inherent randomness) and taking the average of each of those models. If all the models agree on a single prediction then it is a good prediction. If the models disagree on a single prediction then some of the models are getting it wrong. Averaging doesn't do anything when the models agree. When they disagree, the prediction is moved closer to the models that have come to an identical conclusion over those that were confused. I found Pearson's Correlation a great test for figuring out whether two iterations had enough variability.

I was able to get an even better ensemble by using weighted averaging. I ranked each model iteration by performance and weighed it accordingly in the averaging. An ExtraTreesClassifier was a great performer in this regard. It scores an RMSE of 1.145 as an individual model. But in a weighted iterative ensemble the RMSE jumps to 0.965 (lower is better). Moving me up to 16th place in the competition. Averaging magic!

The following is the output of my iterative ensembling function.

```
score_lvl_1
iteration 0 MSE of 16.1733791749
iteration 1 MSE of 17.4862475442
iteration 2 MSE of 16.231827112
iteration 3 MSE of 16.2151277014
iteration 4 MSE of 16.7282252783
iteration 5 MSE of 15.9885396202
iteration 6 MSE of 16.6046168959
iteration 7 MSE of 16.7378847413
iteration 8 MSE of 18.0361820563
iteration 9 MSE of 16.8425016372

ensembled MSE of 12.5898952194
weighted ensembled MSE of 12.5797553676


score_lvl_2
iteration 0 MSE of 0.418631303209
iteration 1 MSE of 0.38785199738
iteration 2 MSE of 0.37098886706
iteration 3 MSE of 0.391617550753
iteration 4 MSE of 0.3701702685
iteration 5 MSE of 0.40635232482
iteration 6 MSE of 0.362311722331
iteration 7 MSE of 0.378683693517
iteration 8 MSE of 0.397838899804
iteration 9 MSE of 0.362966601179

ensembled MSE of 0.309328749181
weighted ensembled MSE of 0.30347545828


score_lvl_3
iteration 0 MSE of 2.37180746562
iteration 1 MSE of 2.1491486575
iteration 2 MSE of 2.4377865095
iteration 3 MSE of 2.13555992141
iteration 4 MSE of 2.04436804191
iteration 5 MSE of 2.06368696791
iteration 6 MSE of 2.46201702685
iteration 7 MSE of 2.46037982973
iteration 8 MSE of 2.16093647675
iteration 9 MSE of 2.20383104126

ensembled MSE of 1.8542632613
weighted ensembled MSE of 1.80534841178

ensembled contest metric of 0.964938365219
```

### So Many Trees; I Die

Taking this concept one step further, I applied it to multiple estimators rather than iterations of a single estimator. I included several variations of the ExtraTreesClassifier since it was such a bully in the iterative ensemble.

{% highlight python %}
meta_pipeline = [
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', RandomForestRegressor(n_jobs=-1, )),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', RandomForestClassifier(n_jobs=-1, criterion='gini')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', RandomForestClassifier(n_jobs=-1, criterion='entropy')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', ExtraTreesClassifier(n_jobs=-1, criterion='gini')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', ExtraTreesClassifier(n_jobs=-1, criterion='entropy')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', ExtraTreesClassifier(n_estimators=500, n_jobs=-1, criterion='gini')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', ExtraTreesClassifier(n_estimators=500, n_jobs=-1, criterion='entropy')),
        ]),
    Pipeline([
            ('scaler', StandardScaler()),
            ('clf', ExtraTreesRegressor(n_jobs=-1)),
        ]),
    ]
{% endhighlight %}

Yes, those are a lot of classifiers for what should be a numerical model. I made the decision early on to treat this as a multi-class classification problem. Yes, the number of violations were ordered, so numerical/regression was the first thing that came to my mind. But as I explored the data, I realized that the the number of violations was finite. They couldn’t be less than zero and couldn’t be more than a maximum. Even if you break every single rule, the number of violations you could get was capped. Level two violations, being based on repeat offenses, had very few classes.

This played out in the results. Individual regressors beat out the classifiers, but when it came to ensembling, classifiers always came out on top. Regressors are just there to bring up some of the variability.

With this multi-estimator ensemble, my RMSE moved from 0.965 to 0.827.

Now in 7th place, I created a new dataset consisting of the predictions from the multi-estimator model as features. I fit this to a held-out response using a GradientBoostingRegressor estimator (LinearRegression also works well). Now my RMSE is as low as 0.725 and I'm in 4th place.

[![Leaderboard]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/leaderboard_edit.jpg)]({{ site.baseurl }}/images/2015-7-24-Winning-Kaggle/leaderboard_edit.jpg)

Linear models based on eight numerical predictors and averaging. Ha! So simple.

### Fin

With more time I would hope to improve this by running a gridsearchCV to optimize hyper-parameters, as well as using pymc3 to build a truly hierarchical model.

If you'd like to view the enormous amount of code that didn't end up working, my github repository is [here](https://github.com/potatochip/kojak).

4th place out of 535 teams. Just a step shy of the podium, but I'll take it. Not bad for my first time out of the gate.
