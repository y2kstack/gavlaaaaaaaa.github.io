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

Apache HBase is a distributed, NoSQL database designed specifically to sit on top of Hadoop and is modelled closely on Googles BigTable.


**Designing an HBase application requires developers to engineer the system using a data-centric approach - not a relationship-centric approach.** This is obviously a lot different than the approach you would take coming from an standard SQL background - *so what are the benfits?*


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

## HBase Tables

A HBase table has the following properties:
1. **Rows, columns and column families**
2. **Row key** per row for fast lookup
3. Sorted for speed
4. Columns hold the data
5. Each **column** has **column family** (there can be one or more column family per table)

![Hbase table conceptual view](https://www.tutorialspoint.com/hbase/images/table.jpg)

*Image taken from [Tutorials Point](https://www.tutorialspoint.com/hbase/hbase_overview.htm)*

Within a HBase table, data stored in HDFS so is therefore split into blocks across nodes within the cluster. It is essentially a distributed sorted map, with the key being the row key and the value being the whole row of data. This means adjacent keys are stored next to each other on disk making them quicker to access. The cells within a table are just arbitrary arrays of bytes - this means you can store anything that can be serialized into a byte-array. However the cell size is the practical limit on the size of the values - its recommended that this should not be consistently above 10MB for performance reasons. Data is physically stored on disk on a per column family basis.

Column families are logical groups of columns (look at it like a table within a table). Separate column families are useful for frequently accessed groups of columns and if you want to compress certain columns but not others. Compression is recommended for most column families apart from those storing already compressed data e.g. JPEG data. **Column family names need to be printable as they are used as the directory name in HDFS** and these names are stored in each row within the HFile (the data file HBase uses to write data) - so if these names are long then it could potentially start to waste space over a long period of time.

Cloudera initially recommend 3 column families per table and they should be designed such that data being accessed simultaneously is in the same olumn family.



