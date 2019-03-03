+++
author = "Alexander Sparkowsky"
date = 2016-03-01T18:00:00Z
description = ""
draft = false
slug = "a-maven-archetype-for-bootstrapping-a-javaee-7-twitter-bootstrap-project"
title = "A Maven archetype for bootstrapping a JavaEE 7 / Twitter Bootstrap project"

+++

I know that [Twitter Bootstrap](https://getbootstrap.com) is discussed very controversially in the community of "real" designers. However I like this template framework since it provides me a way to bootstrap a new project very quickly and have a variety of utilities at hand for creating a responsive frontend. I'm using _Bootstrap_ for a long time now and became comfortable with it.

Also the task to set up a JavaEE 7 web project has become much easier there are still some things to configure and set up to have a JSF frontend with Bootstrap and some other features I usually need to work.

Inspired by the [`javaee7-essential-archetype` written by Adam Bien](https://github.com/AdamBien/javaee7-essentials-archetype) I created a maven archetype that initializes a JavaEE 7 project containing the current version of Twitter Bootstrap (3.3.6 at the time of this writing) and some other features like a text resource and a very simple template.

The archetype can be cloned from the [repository at GitHub](https://github.com/roamingthings/javaee7-bootstrap-archetype). After getting the archetype project you have to install it using `mvn install`. Maybe I will eventually provide the archetype by a public plugin repository.

To use the archetype you execute 

```
mvn archetype:generate -Dfilter=de.roamingthings:javaee7-bootstrap-archetype
```

