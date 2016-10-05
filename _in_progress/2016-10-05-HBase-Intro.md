--- 
layout: post 
title:  Intro to HBase
author: Lewis Gavin 
comments: true 
tags: 
- bigdata 
- hadoop
- hbase 
---


**Designing an HBase application requires developers to engineer the system using a data-centric approach - not a relationship-centric approach.**


## When to use HBase

HBase as a technolgoy has a number of features that give it a wide range of use cases. Due to it sitting on Hadoop, an obvious use case is when you have large amounts of data that continuously grows.

You might ask why not use Hive to solve the same problem, well unlike Hive, HBase is designed to be faster to access to smaller, more specific data sets. Rather than using bulky map reduce jobs to churn through lots of data, it focuses on writing lots of data fast and reading small amounts very fast. 
Facebook managed to get speeds of around 1.5m operations per second.

The way it can randomly read and write data so fast is due to how it stores data. Data is stored in HBase with a key. It can then use this key to lookup items quickly, and due to there being no penalty for sparse columns, you can store as much data as you want on a single row.

It also has in-memory caching and will allow certain nodes or blocks to be more favourable for caching too. This means less disk I/O, which we all know is one of the downfalls of MapReduce on hadoop. It also means if you scale your cluster and add new nodes - you get more cache.

| Do use HBase if... | Do not use HBase if… |
|---|---|
| you have random read and writes | you are only appending to a dataset |
| you have thousands of operations per second on lots of data | your primary usage is ad-hoc analytics - non-deterministic access patterns |
| your access patterns are well known and relatively simple | your data easily fits on one large node |

It has no integrated SQL support as it is a NoSQL database - however Hive and Impala can be used with HBase if you require this flexibility. 

HBase also has no transaction support or no multiple index tables. So this shouldn't be replacing use cases where these are important.


