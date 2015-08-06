---
layout: post
title: Syrian Civil War Cause of Death
excerpt: Visualizing cause of death in the Syrian Civil War.
categories: articles
tags: [Data Science, Graphs, D3]
comments: true
share: true
permalink: Syrian-War-CoD
---

I've started work on showing the change in the use of weapons and tactics in the Syrian Civil War. I whipped up a quick graph illustrating the trend for different causes of death over time.

<figure>
	<a href="{{ site.baseurl }}/images/2015-8-6-Syrian-Civil-War-CoD/cause_of_death_weekly_trends.png"><img src="{{ site.baseurl }}/images/2015-8-6-Syrian-Civil-War-CoD/cause_of_death_weekly_trends.png" alt="image"></a>
	<figcaption>Percentage cause of death - weekly basis</figcaption>
</figure>

You can see a broad overview of what is happening. The inverse correlation for 'Gun Shots' and 'Refusal to Follow Orders' for two obvious spikes is particularly interesting. But it is hard to understand the finer details. So I moved the concept to an interactive D3 visualization instead.

<iframe src="http://bl.ocks.org/potatochip/raw/ff3c62c23a15de2f5e91/" marginwidth="0" marginheight="0" scrolling="no" width="700" height="700" frameborder="0">Browswer not supported</iframe>

[Click here to see the full version in all its glory.](http://bl.ocks.org/potatochip/raw/f7fdafc7a0e6635a7a7d/)
