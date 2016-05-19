---
layout: post
title:  "Apache Oozie - the workflow scheduler for hadoop"
date:   2016-05-19
categories: architect
author: "Chyler"
---

>One of Oozie’s strengths is that it was custom built from the ground up for Hadoop. This not only means that Oozie works well on Hadoop, but that the authors of Oozie had an opportunity to build a new system incorporating much of their knowledge about other legacy workflow systems. Although some users view Oozie as just a workflow system, it has evolved into something more than that. The ability to use data availability and time-based triggers to schedule workflows via the Oozie coordinator is as important to today’s users as the workflow. The higher-level concept of bundles, which enable users to package multiple coordinators into complex data pipelines, is also gaining a lot of traction as applications and pipelines moving to Hadoop are getting more complicated.

**Recurrent Problem**

Hadoop, Pig, Hive, and many other projects provide the foundation for storing and processing large amounts of data in an efficient way. Most of the time, it is not possible to perform all required processing with a single MapReduce, Pig, or Hive job.Multiple MapReduce, Pig, or Hive jobs often need to be chained together, producing and consuming intermediate data and coordinating their flow of execution. 

As developers started doing more complex processing using Hadoop, multistage Hadoop jobs became common. This led to several ad hoc solutions to manage the execution and interdependency of these multiple Hadoop jobs. Some	developers wrote simple shell scripts to start one Hadoop job after the other. Others used Hadoop’s JobControl class, which executes multiple MapReduce jobs using topological sorting. One development team resorted to Ant with a custom Ant task to specify their MapReduce and Pig jobs as dependencies of each other—also a topological sorting mechanism. Another team implemented a server-based solution that ran multiple Hadoop jobs using one thread to execute each job.

As these solutions started to be widely used, several issues emerged. *It was hard to track errors and it was difficult to recover from failures. It was not easy to monitor progress. It complicated the life of administrators, who not only had to monitor the health of the cluster but also of different systems running multistage jobs from client machines. Developers moved from one project to another and they had to learn the specifics of the custom framework used by the project they were joining.* Different organizations were using significant resources to develop and support multiple frameworks for accomplishing basically the same task.

**A Common Solution: Oozie**

It was clear that there was a need for a general-purpose system to run multistage Hadoop jobs with the following requirements:

- It should use an adequate and well-understood programming model to facilitate its adoption and to reduce developer ramp-up time.
- It should be easy to troubleshot and recover jobs when something goes wrong.
- It should be extensible to support new types of jobs.
- It should scale to support several thousand concurrent jobs.
- Jobs should run in a server to increase reliability.
- It should be a multitenant service to reduce the cost of operation.

