+++
author = "Alexander Sparkowsky"
categories = ["Spring Boot", "metrics", "Prometheus"]
date = 2018-04-22T11:41:54Z
description = ""
draft = true
slug = "spring-boot-2-and-metrics"
tags = ["Spring Boot", "metrics", "Prometheus"]
title = "Spring Boot 2 and Metrics"

+++

When running an application you want to know about what's going on inside. One way to get these insights is the collection of metrics.

Example of interesting metrics for a service might be
* Memory usage
* CPU usage
* How often is a resource being called
* What time does it take to fulfill a request
* How often do certain errors occure

Beside collecting and looking at these information they could also be used to raise alarms if for example the amount of free memory becomes low.

