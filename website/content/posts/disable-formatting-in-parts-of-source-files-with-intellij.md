+++
author = "Alexander Sparkowsky"
categories = ["intellij", "ide", "settings", "formatting"]
date = 2017-06-04T09:39:24Z
description = ""
draft = false
slug = "disable-formatting-in-parts-of-source-files-with-intellij"
tags = ["intellij", "ide", "settings", "formatting"]
title = "Disable formatting in parts of source files with IntelliJ"

+++

Since I'm writing a lot of Spring code or using libraries like [Rest Assured](rest-assured.io) that allow to write somehow "human readable" code it's nice to be able to turn off code formatting int IntelliJ so that the structure is not destroyed when the source file gets reformatted.

```
given()
    .contentType("application/x-www-form-urlencoded; charset=utf-8")
    .when()
    .queryParam("foo", "bar")
    .get("http://localhost:8080/bar")
    .then()
    .statusCode(302)
```

To prevent this from happening IntelliJ offers a function to turn off formatting by a marker comment. This function is disabled by default.

To enable it got to _Settings > Editor > Code Style_ and activate _Enable formatter markers in comments_.
There you can also change the default markers which are `@formatter:off` and `@formatter:on`.

After enabling the formatter markers you can prevent IntelliJ to change the formatting by adding comments to the beginning and ending of the code fragment you want to protect.

```
// @formatter:off
given()
      .contentType("application/x-www-form-urlencoded; charset=utf-8")
    .when()
      .queryParam("foo", "bar")
      .get("http://localhost:8080/bar")
    .then()
      .statusCode(302)
// @formatter:on
```

I know that adding "configuration comments" is a controversy discussion. Personally I can accept this kind of additional information since I'm working in the IDE most of the time and theses comments will bother me less then when the formatting gets destructed.

