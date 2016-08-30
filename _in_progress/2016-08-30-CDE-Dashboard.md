--- 
layout: post 
title:  Real Time Pentaho CDE Dashboard, Big Data Journey Part 4
author: Lewis Gavin 
comments: true 
tags: 
- docker 
- hadoop
- pentaho 
---


This is the final post in the Big Data Journey blog series. So far we have looked at [installing CDH and Pentaho BA using Docker](http://www.lewisgavin.co.uk/CDH-Docker), [ingesting data with Kafka and Spark](http://www.lewisgavin.co.uk/Data-Ingestion-Kafka-Spark) and then in the last post, we looked at [transforming and storing that data in Apache Kudu](http://www.lewisgavin.co.uk/Kudu-Spark). In this final post, we will look at reporting on top of our beautifully ingested and transformed, real time data feed. This will involve using Pentaho CTools to build a simple chart that will update based on the latest picture within our data feed.

## Pentaho CTools - What is it?

CTools is a set of visualisation and dasboarding tools that sit on top of Pentaho BA. Within the community edition of Pentaho we installed in [part 1](http://www.lewisgavin.co.uk/CDH-Docker), you get access to CDE - Community Dashboard Editor. This allows you to create HTML and Javascript dashboards that use Pentaho BA to serve the data. Data sources can be created to obtain data from a number of different sources out of the box, including Impala! This will allow us to query our impala table and then visualise the results.

Going over all of the basics in this post would be a mammoth task and considering Pentaho have already gone to the trouble, you can use their [overview of CDE](https://help.pentaho.com/Documentation/6.1/0R0/CTools/CDE_Dashboard_Overview) and [CDE quickstart guide](https://help.pentaho.com/Documentation/6.1/0R0/CTools/CDE_Quick_Start_Guide) to get you up to scratch. For the remainder of this post, I'll presume you know some basic html and are at least a little bit familiar with CDE.


## Creating a Data Source

Connect to docker - make sure ports are open for ease of use - go to 8080, login as admin - create a datasource - point at impala - test that it works by printing out results.