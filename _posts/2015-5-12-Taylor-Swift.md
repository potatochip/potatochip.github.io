---
layout: post
title: Categorical Analysis of Taylor Swift's Latest Album
tags: [Data Science, Taylor Swift, Graphs]
categories: articles
share: true
---
##Out of the Woods

Every morning, my day begins with a healthy dose of coffee and Taylor Swift. In case you haven't noticed, Taylor Swift only sings love songs. Bad Blood? Love song. Welcome to New York? Love song. Everything is a love song. Don't let anybody tell you otherwise.

This morning, I thought it would be hilarious to categorize the songs in her latest album, 1989.

##Shake it Off

Each song in 1989 covers multiple topics. Contrary to popular belief, she doesn't just sing about Starbucks. The following is a chart illustrating the number of times a topic appears in an album.

![Total references per song]({{ site.baseurl }}/images/2015-5-12-Taylor-Swift/totals.png)

Top three topics?

1. A broken heart
2. That love drives her crazy
3. That her relationships are rocky

duh.

Let's see how often a topic appears at the same time as others in a song.

![Topic Correlation]({{ site.baseurl }}/images/2015-5-12-Taylor-Swift/multi-variable_correlation.png)

That dark red square shows that she takes the intiative to break up with the guy whenever she is in a shitty relationship (and then sings about it). Go Taylor.
