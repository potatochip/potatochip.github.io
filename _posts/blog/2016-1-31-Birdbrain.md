---
layout: post
title: "Tweet-Off"
excerpt: How to automate winning cupcakes
tags: [Data Science, lol]
categories: blog
share: true
comments: true
---

Every week at work we have a competition to see who can drive the most traffic to the site. Each member of the team that drives the most traffic gets a free cupcake or donut or coffee. You get the picture.

So obviously, I built a [twitter bot](https://twitter.com/hiringbird/) to win these competitions for me.

I built in a lot of randomness because I figured twitter would suspend my account for just spamming the same post over again. Now that I've taken a closer look at other twitter accounts, its clear that twitter doesn't give an F.

The bot posts at a random time every 30-90 minutes. It randomly chooses the link/topic it will post about. 30% of the time it uses my company's suggested text. The other 60% of the time it generates text using Markov Chains built from HR-related posts on twitter.

About half the Markov Chains are completely random while the other half are generated with a seed consisting of the first 2-3 words of the company suggested text.

Here are a few recent gems the bot has come up with:

+ Struggling to stay ahead #hiring #jobopenings
+ One of the unexpected" #cv #job
+ Ad seeks candidates #jobopening #tweetmyjobs

This bot is so deep on so many levels.

And of course, the most popular post so far has been:
"Can emerging tech data startup ip seo growth trending big data b #hireme #socialrecruiting." I figure its just a bunch of other bots liking it. They post their dribble. I crawl their dribble. I rephrase their dribble in one of the most popular ways. They pick up on their dribble and like it. So meta.
