+++
title = "Disable Gralde tests when building projects with SAM"
author = "Alexander Sparkowsky"
categories = ["aws"]
date: 2021-10-30T15:47:13+02:00
description = ""
draft = false
slug = "disable-gradle-tests-on-sam-build"
tags = ["aws", "sam", "cloud", "java", "gradle"]
+++


The [Serverless Application Model (SAM)](https://aws.amazon.com/serverless/sam/) allows to easily build, deploy and run
serverless applications on AWS. I like the fact that SAM is able to work with Lambda functions that are written in Java
and are using [Gradle](https://gradle.org/) as build tool.

When running `sam build` to (re-)build any such function, SAM will always do a `gradle clean build` which includes
execution of tests. This makes total sense when running in a CI/CD pipeline. However, during development this will slow
down my development process since all test  are executed. Failing tests will also prevent sam from finishing the build.

To prevent SAM from running the tests, I added a script to my `$HOME/.gradle/init.gradle.kts` file:
```kotlin
allprojects {
    tasks.withType(Test::class) {
        onlyIf {
            System.getProperty("software.amazon.aws.lambdabuilders.scratch-dir") == null
                    || System.getenv("GRADLE_SAM_EXECUTE_TEST") != null
        }
    }
}
```

This works because SAM sets the system property `software.amazon.aws.lambdabuilders.scratch-dir` during build. With this script in place I don't have to do any special modifications to my project. I can even let SAM execute the tests by setting the environment variable `GRADLE_SAM_EXECUTE_TEST` to any value:

```shell
GRADLE_SAM_EXECUTE_TEST=1 sam build
```
