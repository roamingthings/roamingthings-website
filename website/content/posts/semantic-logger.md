+++
author = "Alexander Sparkowsky"
categories = ["Java", "jvm", "Kotlin"]
date = 2018-12-02T08:18:02Z
description = ""
draft = false
slug = "semantic-logger"
tags = ["Java", "jvm", "Kotlin"]
title = "A Semantic Logger for JVM Languages"

+++

This little library is inspired by the talk _Observability in einer Microservice Welt_ held on [Berlin Expert Days 2018](http://bed-con.org/2018/home)
by Andreas Weigel and Jakob Fels.

If you prefer to have some semantic to your logging and improve code readability you can use the `SemanticLogger`
and its `SemanticLoggerFactory` which adds a thin layer around Slf4j.

The `SemanticLoggerFactory` can be used like the Slf4j `LoggerFactory`
to create a new Logger by class, name or a delegateLogger:

```
SemanticLogger logger = SemanticLogger. getLogger(SomeClass.class);

SemanticLogger logger = SemanticLogger. getLogger("SomeLoggerName");

Logger delegateLogger = org.slf4j.LoggerFactory.getLogger(SomeClass.class);
SemanticLogger logger = SemanticLogger. getLogger(delegateLogger);
```

The `SemanticLogger` itself provides the following methods for logging:

```
SemanticLogger log = SemanticLoggerFactory.getLogger(MyClass.class);
log.forTestPurpose(...);                // will log a debug event
log.asExpectedByDefault(...);           // will log an info event
log.toInvestigateTomorrow(...);         // will log a warn event
log.wakeMeUpInTheMiddleOfTheNight(...); // will log an error event
```

You can find the library and more information at my [GitHub: Semantic Logger ](https://github.com/roamingthings/semanticlogger).

