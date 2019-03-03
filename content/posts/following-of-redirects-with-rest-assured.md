+++
author = "Alexander Sparkowsky"
categories = ["Java", "testing", "TDD", "Rest-Assured"]
date = 2017-06-04T09:46:56Z
description = ""
draft = false
slug = "following-of-redirects-with-rest-assured"
tags = ["Java", "testing", "TDD", "Rest-Assured"]
title = "Control following of redirects with Rest-Assured"

+++

By default Rest-Assured will follow a redirection if it receives a 302 response after issuing a request. In this case your checks will be applied to the response _after_ the redirect.

Some times you may not want this to happen and rather examine the response of the original request for example to extract cookies or assert the correct location of the redirect.

To achieve this you can use the `redirects().follow(false)` directive when formulating the request:

```
given()
  .contentType("application/x-www-form-urlencoded; charset=utf-8")
  .when()
    .redirects().follow(false)
  .get("/foo")
  .then()
    .statusCode(302)
    .header("Location", notNullValue());
```

