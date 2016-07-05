Recently I attended Strata and Hadoop World Conf in London, you can see my post on the day [here](http://www.lewisgavin.co.uk/Strata-Hadoop/). During some of the afternoon technical sessions I attended a talk by Holden Krau on scaling Spark jobs. 

The talk gave me a lot of good tips to take away and implement into my own Spark code so I thought they were worth sharing. 

## 1. Caching and Checkpointing -  which one and when? 

RDD's can sometimes be expensive to make. Even when they're not, you wouldn't want to remake them over and over again. Caching RDD's, especially when expensive, allows reuse without a performance overhead. Take the example below. 
<INSERT PIC FROM SPARK NOTEBOOK>

By simple caching the RDD I can happily re-use it for some later transformations without having to rebuild it from scratch. 

RDDs can be cached in memory (by default) or persisted to disk. You would choose the persistence level based on your use case and the size of the RDD. Full details on parameters can be found [here](http://spark.apache.org/docs/latest/programming-guide.html#rdd-persistence)

If you are in a noisey cluster, checkpointing may also help to store RDD's to files within HDFS saving memory. Otherwise it is recommended to only use checkpoints when your RDD lineage gets too large. 
It is important that you persist/cache your RDD first before checkpointing as checkpointing will materialise the RDD twice, once when it builds it for use and again when it needs to write it to disk - however if you have cached it before hand, it only needs to materialise it once, the second time it can read from the cache.

**So whats the difference between checkpoints and using `persist(DISK_ONLY)` I hear you ask?**
Great question! The answer is simple - persisting will materialise and save the RDD in memory or disk or both depending on your configuration and will also **store and remember the lineage**. 
This means that if are Node failures on the node storing your cached RDD, they can be rebuilt as using the lineage. Checkpoints **do not store the lineage** and will only write RDD contents to disk.

## 2. The evil Group By Key

Holden used the term Key skewing. Keys aren't evenly distributed. Can lead to unbalanced partitions. Null records are also an issue. Group by key doesn't work well if keys are skewed. Group by key groups all records with the same key into a single record. Even if we immediately reduce or sum. This can be too large to fit in memory and cause jobs to fail. Reduce by key allows map side reduction and doesn't make a super large single record. It will reduce the data locally before distributing it across the network. Sort by key. All data for a key goes to the same node so the collection could be too large for the node. Can add some extra junk to a key to help distribute it better. 

## 3. Spark SQL and Data Frames to the rescue

Spark sql performance benefits. Structured or semi structured data. Non jvm users should use data frames as data needs to be copied from jvm to worker and back and it's expensive. Space efficient columnar representation. Can push down operations to data store and optimizer is able to look inside operations. Regular spark can't do this so doesn't know difference between (min(_,_))  and (append(_,_)). Data frames are faster than RDD. (see pic) Will explode for iterative algorithms and large plans. Default shuffle size is sometimes too small for big data(200 partitions) To avoid lineage explosions,convert to RDD, cache and then recreate data frame. 

## 4. Datasets are the future. 

Datasets are the new data frames. Iterator to Iterator transformations. Allow spark to spill to disk when reading a partition and has better pipelining. Most default transform functions are set up for this. 
