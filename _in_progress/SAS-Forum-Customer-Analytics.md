--- 
layout: post 
title:  SAS Forum UK Customer Analytics
author: Lewis Gavin 
comments: true 
tags: 
- datascience 
- machinelearning
- spark 
---

![SAS Forum UK 2016 Birmingham Day 3](../)

This post will explore the second day of conferences - make sure you check out the [previous day](http://www.lewisgavin.co.uk/SAS-Forum-UK) too. The main theme was around Customer Intelligence. How do you monitor customer events? How do you better understand how your customers feel? How do you improve and tailer each customers experience?

## Keynote: SAS Customer Intelligence 360

The main goal of SAS CI is to monitor customer events. The main differentiator between this and Google Analytics for example, is that the tool allows you to dig deeper and track even the smallest of customer interactions. This includes what part of the webpage have they seen, what buttons have they clicked and what fields have they filled in.

The tool takes this further by allowing you to then perform analytics on these events. It can automate what to show a specific customer based on other customers that have the same profile or based on the content that gets the best customer response. For example dynamically change what image of an item of clothing to show a customer that will more likely make them buy the item. Obviously the outcomes change based on different segments of customers, so this allows you to show customer group X one picture and Y another, as that will mean those customers will be more likely to buy based on historical data.

This obviously is a leap forward in providing analytics to help personalise a customers experience. In the future they are looking to enhance the product to not only work using digital data but other data channels including social media or phone calls.

The next talk was a deeper dive into SAS CI 360 going into a little more detail of what was discussed above.

## Streaming Analytics and IoT

SAS have built their own streaming product called ESP (Event Stream Processing). It follows the usual model of publish and subscribe however between the publisher and subscriber, there is an engine that allows you to enrich event data, run models on the events or apply business rules. These actions then create Complex Events that can be sent to the subscriber to be visualised, analysed or reported more efficiently. 

ESP again runs in the cloud and is built to be very performant. It was built using C++ and brings the data into memory to gain the best performance. They quoted a benchmark that stated that it could run a KMeans model on 3.5 million events in one second, which is pretty impressive.

## Identity Row Level Security

This session was another session hosted by RBS.