--- 
layout: post 
title:  Data Ingestion with Kafka and Spark, Big Data Journey Part 2
author: Lewis Gavin 
comments: true 
tags: 
- docker 
- hadoop
- pentaho 
---


Welcome to part 2 of the series. In [part 1](http://www.lewisgavin.co.uk/CDH-Docker) we looked at installing a CDH quickstart docker container along with Pentaho BA. Now we have some basic infrastructure in place, it's time to start thinking about data!

The logical place to start is **data ingestion**. You can't do anything until you have some data, so in this post we are going to explore using Apache Kafka to stream data in real time from Twitter. If you are not familiar with Kafka I recommend [this post] to outline the basics before continuing.

## Installing Kafka

Unfortunately Kafka does not come pre-installed on our CDH instance so we will need to install it. It is important that we install version **0.9.0** or above, as this will allow us to integrate it with Spark streaming a little later.

You can get all the details of how to do this from [Clouderas online documentation](https://www.cloudera.com/documentation/kafka/latest/topics/kafka_packaging.html)

Once you've made sure you're pointing at the latest parcels simply install kafka:

~~~bash
$ sudo yum install kafka
$ sudo yum install kafka-server

~~~

To ensure you have the right version go to the Kafka libs file `cd /usr/lib/kafka/libs` and there should be a file with a name something like `kafka_2.11-0.9.0-kafka-2.0.2.jar` - the 0.9.0 in the name indicates the Kafka version.

**Now for some important config changes**. Within `/usr/lib/kafka/config` edit the file called `server.properties` and make sure you add/amend the following:

Ensure the broker.id property is given a number (I recommend 0)

~~~bash
############################# Server Basics #############################                
                                                                                         
# The id of the broker. This must be set to a unique integer for each broker.            
broker.id=0

~~~

Set the port to 9093. **The default is 9092 but that is taken by Pentaho BA**

~~~bash

# The port the socket server listens on
port=9092   

~~~

Finally, make sure the hostname and advertised hostname are set to localhost

~~~bash

# Hostname the broker will bind to. If not set, the server will bind to all interfaces   
host.name=localhost    
                                                                                         
# Hostname the broker will advertise to producers and consumers. If not set, it uses the 
# value for "host.name" if configured.  Otherwise, it will use the value returned from
# java.net.InetAddress.getCanonicalHostName().          
advertised.host.name=localhost  

~~~

**Lets start Kafka!**

~~~bash

$ service kafka-server start

~~~

### Testing Kafka

There is a nice quickstart/hello world that comes with the Kafka installation that you can walk through using the [kafka quickstart guide](http://kafka.apache.org/07/quickstart.html). 

~~~bash

# Run the producer to take input from standard input
$ kafka-console-producer --broker-list localhost:9093 --topic test
Hello world
I hope this makes it to the consumer

# Run the consumer to print all messages to standard out from the beginning 
$ kafka-console-consumer --zookeeper localhost:2181 --topic test --from-beginning
Hello world
I hope this makes it to the consumer

~~~

If all goes well we're good to start and create our first Kafka producer to bring in data from Twitter.

## Writing a Kafka Twitter Producer in Java

You can do this inside or outside the container. Either way we need to build a Java application. I used a [guide](http://saurzcode.in/2015/02/kafka-producer-using-twitter-stream/) by [saurzcode](https://twitter.com/saurzcode/) to write the Java Producer to connect to twitter. The code is available on the [saurzcode github](https://github.com/saurzcode/twitter-stream/).

Just make sure you change the metadata broker list property to point at port 9093 and feel free to change the hashtag of tweets to obtain.

~~~java

properties.put("metadata.broker.list", "localhost:9093");

...

endpoint.trackTerms(Lists.newArrayList("twitterapi", "#bigdata"));

~~~

The rest of the code will work fine - so simply package it into a JAR file with maven and if you built it outside of docker - transfer it to the docker container.

~~~bash

$ docker cp kafka-twitter-producer-1.0-SNAPSHOT.jar <container id>:/home/cloudera/Documents

~~~

Before you can run this make sure you have your twitter credentials obtained (the guide above explains how to do this) as you will need to pass them in as parameters in the next step.

Within the docker container execute the jar as follows:

~~~bash

$ java -jar kafka-twitter-producer-1.0-SNAPSHOT.jar <consumer key> <consumer secret> <token> <secret> &

~~~

That should start collecting tweets from the hashtag you entered into Kafka.

We can run the 