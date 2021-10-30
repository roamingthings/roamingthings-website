+++
author = "Alexander Sparkowsky"
categories = ["Spring Boot", "Kotlin", "Java", "Workbench", "Gradle"]
date = 2018-08-19T07:32:52Z
description = ""
draft = false
slug = "spring-boot-auto-configuration-and-starter"
tags = ["Spring Boot", "Kotlin", "Java", "Workbench", "Gradle"]
title = "Building a Spring Boot 2 Auto-Configuration and Starter with Kotlin and Gradle"

+++

A concept that makes Spring Boot very powerful are its [Starters and Auto-Configuration](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-custom-starter). It enables the automatic inclusion of dependencies and configuration or initialization based on several conditions.

Recently I have been looking for a tutorial or "best practice" on how to use Gradle to build a custom starter. Since I was unable to find an example I ended up writing my own. I also added some Kotlin into the mix.

You can find the project at [GitHub](https://github.com/roamingthings/spring-boot-gradle-workbench-starter).

