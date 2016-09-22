--- 
layout: post 
title:  Training a Machine Learning Model to Detect Sarcasm using Twitter
author: Lewis Gavin 
comments: true 
tags: 
- machinelearning 
- datascience
- spark 
---


In the last post a model was trained to detect sarcasm using just a small amount of sample data. In order for the model to be useful generically, in a larger amount of scenarios, it requires more training data that comes from a wider amount of sources. This is so the model understands many different types of sarcasm. Creating this data by hand would take a long time and would be suspect to personal bias. To solve this problem, twitter can be used to train the model by using any tweets that contain #sarcasm or #sarcastic as my sarcastic training data.

## How does it work?

The basic concept is to bring data in from twitter and get a consistent amount of tweets that are both sarcastic and non-sarcastic, so that the training data is fair. 

The tweets need to be labelled correctly too. If a tweet contains #sarcasm or #sarcastic it is labelled with a 1, otherwise it is labelled with a 2. The stream then continues to bring data in until there is a consistent amount of each.

~~~scala

    val stream = TwitterUtils.createStream(ssc, None)

    //label the tweets based on sarcasm hashtag
    val labelledTweets = stream.map(tweet => standardiseString(tweet.getText))
      .map{tweet =>
        if(tweet.contains("#sarcasm")) {
          (1, createNgram(tweet, 2))
        }
        else {
          (0, createNgram(tweet, 2))
        }
      }

~~~

Now we are pretty much at the same state as we were in part 1. We have a labelled data set. This data set needs to be vectorised so that it can be used to train the Naive Bayes model.


