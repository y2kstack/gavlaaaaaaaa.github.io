--- 
layout: post 
title:  Sarcasm Detection with Machine Learning in Spark
author: Lewis Gavin 
comments: true 
tags: 
- datascience 
- machinelearning
- spark 
---


## Intro to the Approach

This post is inspired by a [site](http://www.thesarcasmdetector.com/about/) I found whilst searching for a way to detect sarcasm within sentences. As humans we sometimes struggle detecting sarcasm when we have a lot more contextual information available to us. People are emotive when they speak, they use certain tones and these traits can help us understand when someone is being sarcastic. However we don't always catch it! So how the hell could a computer detect this, when all it has is text.

Well one way is to Machine Learning. I wondered if I could set up a machine learning model that could accurately predict sarcasm (or accurately enough for it to be effective). This search led me to the above link site where the author Mathieu Cliche cleverly came up with the idea of using tweets as the training set.

As with any machine learning algorithm, its level of accuracy is only as good as the training data it's provided. Create a large catalog of sarcastic sentences could be rather challenging. However searching for tweets that contain the hastag #sarcasm or #sarcastic would provide me with a vast amount of training data (providing a good percentage of those tweets are actually sarcstic).

Using that approach as the basis, I developed a Spark application using the MlLib api that would use the Naive Bayes classifier to detect sarcasm in sentences - using twitter data as my training set!

## Setting up the Naive Bayes algorithm

## Testing it on sample data

## Integrating with Twitter