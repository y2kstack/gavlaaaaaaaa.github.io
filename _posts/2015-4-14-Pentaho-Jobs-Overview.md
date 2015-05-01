---
layout: post
title: Pentaho Jobs Overview
---

A **Job** allows high level steps to be strung together.

Like transformations it uses the concept of steps and hops, however the steps are in the form of a Transformation, a Job or a job level step.

The Transformation step simply allows you to plug in a previously created transformation and join it onto another step, for example:

![Job-Trans-Steps](http://wiki.pentaho.com/download/attachments/8291384/transformation_diagram_revised.jpg?version=1&modificationDate=1214485137000)

**The first rule of any job is that it must begin with a 'Start' step**.

As you can see in the above example, the job begins with a *Start* and then strings two transformations together, but inbetween them
it does a job level step, which checks if a file exists.

You can also string whole jobs together in the same way as you do transformations, if you want to abstract up higher.


