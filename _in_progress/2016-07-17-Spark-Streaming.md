--- 
layout: post 
title:  Real Time Analytics with Spark Streaming
author: Lewis Gavin 
comments: true 
tags: 
- spark 
- streaming
- realtime 
---

I'm quite a big fan of Spark. I recently did a post on [improving spark code performance] (http://www.lewisgavin.co.uk/Spark-Performance) and in this post, I want to delve into the Spark Streaming API. 

## How does it work? 

The Spark Streaming API works in a micro batch mode. Processing a micro batch a time but in a distributed mode. This means you can perform computations on very small batches of data, over and over, replacing or updating the result in near real time. 

The key reason the Spark Streaming API has become so popular though, is due to its simplicity. It runs on Spark so transparently your Spark Streaming RDD can be joined with a static RDD. With the syntax and functions being so similar to that of standard RDDs, you can pick it up very easily. You can even migrate a static application to a streaming one with some very minor tweaks. 

~~~java
val config = new SparkConf().setAppName("stream-test")
val sc = new SparkContext(config)

val ssc = new StreamingContext(sc, Seconds(10))

~~~

The above code snippet shows how simple it is to set up a Spark streaming context. You simply give it a Spark context and a time in seconds. This time represents the size of your **window**. Which is where we go next... 

## Windows 

## Changing from standard Spark to the SparkStreaming API 

## Use Cases

There are a hundred and one use cases for the Spark Streaming API. The one given in most examples is that of a real time sentiment analytics engine for Twitter. Streaming tweets of a particular subject through spark, running sentiment analytics on all the words within the tweets. Then outputting the results to be reported on by a visualisation library such as D3.js. 

To get this simple use case working, you first need to sign up to the twitter applications page to receive some authentication tokens. 

You then build your Spark Streaming context. Initialise the twitter stream using the library. Then you're set to go. 

Flat map the words from your set of tweets within your window. Check for positive and negative words against a reference list. Then do a reduce to get the word count and keep the top 50. Keep doing this and join back to your "current" dataset and you have the top 50 words used on twitter for a specific subject along with their sentiment. 

This can give you a real time picture of the sentiment surrounding the particular subject matter at this point in time. With some added context of the common words being used. 
