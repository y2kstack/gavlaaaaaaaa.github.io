---
layout: post
title: Pentaho Transformations
---

A **Transformation** is a connection of steps that allow **ETL** processes to take place on top of data.

It allows rows to be input from a number of input sources, transformed and then output to a number of output sources (anything from text files to Oracle Databases).
*Steps* within a transformation are connected by **Hop**s which are one way channels demonstrating the flow of data.

Below is an example of a transformation with two steps and a hop.

![Pentaho-Hop](https://anotherreeshu.files.wordpress.com/2014/12/capture11.png)

## Steps
A **Step** is represented by an icon within the *Spoon* interface and at a transformation level does something to so data.
In the above diagram there is a Table input step that will bring in data from a database table and then an file output step that will allow the data to be written to a file in a user specified location and format.

**Important characteristics:**

1. Step names must be unique within a transformation

2. Most steps can both read and write data (there are a few exceptions)

3. Steps can pass data to one or more other steps through outgoing hops.

4. Most steps can have multiple outgoing hops

5. When a transformation is run, one or more copies of each step are initialised and run in their own thread - all these steps run simultaneously (or as parallel as possible).

6. Each step has a set of features that define its functionality.

## Hops
A **Hop** defines the data flow between two steps and is represented by an arrow.

A hop can take many forms such as: 

1. Always flow data to the next step.

2. If a step filters data then it can send data that matches the filter to one step and the rest of the data to antoher step.

3. Loops are not allowed in transformations as a transformation depends on previous steps when passing values through steps.

Below is an example of a Hops after filtering (notice the difference to the first image)
![Filter-Hop](https://www.packtpub.com/sites/default/files/Article-Images/5245OS_06_02.png)
