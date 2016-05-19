---
layout: post
title:  "Apache Oozie - the workflow scheduler for hadoop"
date:   2016-05-19
categories: architect
author: "Chyler"
---

One of Oozie’s strengths is that it was custom built from the ground up for Hadoop. This not only means that Oozie works well on Hadoop, but that the authors of Oozie had an opportunity to build a new system incorporating much of their knowledge about other legacy workflow systems. Although some users view Oozie as just a workflow system, it has evolved into something more than that. The ability to use data availability and time-based triggers to schedule workflows via the Oozie coordinator is as important to today’s users as the workflow. The higher-level concept of bundles, which enable users to package multiple coordinators into complex data pipelines, is also gaining a lot of traction as applications and pipelines moving to Hadoop are getting more complicated.


