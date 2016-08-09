--- 
layout: post 
title:  Starting your Big Data Journey with Cloudera and Pentaho: Part 1
author: Lewis Gavin 
comments: true 
tags: 
- docker 
- hadoop
- pentaho 
---

## Getting Started

To get started make sure you have [docker]() installed on your machine. Once that is done simply go the [Cloudera Quickstart Docker]() page and follow the really intuitive step by step guide. I will include the instructions for the machine I used below too for reference. (Ubuntu 16-04-1 OS with Docker 1.10.3)

1. Pull the latest version of the VM

~~~bash
docker pull cloudera/quickstart:latest
~~~

2. Run the Image

**Note:** I have used the `-p` flag to specify which ports I want to be available to my host machine. Port `8888` is for Hue and `8080` will be used for Pentaho BA.

~~~bash
docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888 -p 8080 cloudera/quickstart /usr/bin/docker-quickstart
~~~

