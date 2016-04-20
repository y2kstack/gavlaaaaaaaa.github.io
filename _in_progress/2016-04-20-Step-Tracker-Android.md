---
layout: post
title: How to track steps and calculate running distance - Android
author: Lewis Gavin
comments: true
tags:
- android
- programming
- java
---

I am currently in the process of uplifting an android application I built in my spare time. Between finishing university and starting work I had a fair amount of free time, which I used to go to the gym. I found myself trying numerous apps and most of them just contained too much "stuff" and were really bloated. All I wanted was a simple app to track my exercises and maybe show me a graph of my progress. This inspired me to use the rest of my free time to start my own android app. 

The app I made was called Gymify. It is not available on the app store but at the time it was a simple list based application (see my previous post on my List implementation here: [ListView Adapters](../))where a user could add routines and then work through exercises in a routine and tick them off. Once I started work I found that I became too busy and progress on the app slowed. Recently I have tried to get myself back into the habit of doing a little bit of coding in my spare time, even if its just 10 minutes a night. This has allowed me to escape work based problems and also focus on different problems using different technologies than those used at work.

One of the enhancements I made to the app recently was to introduce the capability to store Cardio based activities. For this I wanted to include tracking the amount of time, tracking steps and also tracking distance. This led me to think about how distance could be tracked if, for example, the user was using a treadmill instead of running outside. GPS tracking wouldn't work well in this situation, as the subject is not really moving geographically at all. This post will explore how with a simple library, step tracking can be added to an application, and then talk about how to use this information to approximate distance run.