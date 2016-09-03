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


## Creating a Connection

Connect to docker and make sure the port for Pentaho BA is open for ease of use. Make sure Pentaho is running by going to the biserver-ce folder (if you followed my instructions it will be in `/opt/biserver-ce` and run `./start-pentaho.sh`). Now within a terminal window (outside of the docker container) run docker ps and see which local port has been mapped to `8080`.

Now open up a browser and go to the port you found above. The Pentaho BA login page should appear. To login as admin the details are: username = admin, password = password.

Once in the main window we need to create a JDBC connection to point at impala. Click *Manage Data Sources* and then the little settings sign and click *create new connection*. Give your connection a name and select **Impala** for the database type. The hostname is **localhost** and the database name should be **default**. The port can stay as 21050. Click test to ensure it can connect to our Impala instance.

## Creating our Dashboard

Now click Create New > CDE Dashboard from the home window. In the layout panel click the **Add FreeForm** button and within the FreeForm add a HTML element. Give it a name and then select the HTML edit button on the right. You can now write some custom HTML in here to visualise your data. We're going to do a simple test to just prove we can get some data back.

Fill it in as follows.

~~~html
<html>    
    <script>
        function write_data(impala_data){
            document.getElementById('ngram').innerHTML = JSON.stringify(impala_data);   
        }
    </script>
    <p id="ngram"></p>    
</html> 
~~~

Within the **Datasources panel** select SQL Queries and sql over sqlJndi. Within the properties column on the right fill in the properties as follows:
Name = impala
Jndi = impala (or whatever you called your Connection earlier)
query = select ngram, count from twitter_ngram;

Within the **Components panel** select Others > Query Component from the left. In the properties column select Advanced Properties and fill them in as follows:
Name = impala_component
result var = impala_data
datasource = impala
Post execution =
~~~javascript
function (){
    //call the function within our html object to write the data
    write_data(impala_data);
} 
~~~

Save your progress and click the preview button (top right), you should see something like the following depending on what words are within your table.

![Ctools test](../images/ctools-test.png)