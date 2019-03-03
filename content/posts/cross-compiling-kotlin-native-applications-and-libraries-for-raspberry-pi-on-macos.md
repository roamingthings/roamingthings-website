+++
author = "Alexander Sparkowsky"
date = 2017-12-21T08:40:36Z
description = ""
draft = false
slug = "cross-compiling-kotlin-native-applications-and-libraries-for-raspberry-pi-on-macos"
title = "Cross-compiling Kotlin/Native applications and libraries for Raspberry Pi on macOS"

+++

I'm a big fan of the [Kotlin programming language](http://kotlinlang.org) since it has been announced by JetBrains some times ago.

After [Kotlin/Native](https://github.com/JetBrains/kotlin-native) became available including the ability to compile to native Raspberry Pi binaries I decided to play around with it and see what's possible. The problem I've been facing is the fact that cross-compilation for Raspberry Pi requires a Linux system. Since I prefer to use my macOS system for developing I have been looking for an alternative to use a full blown Virtual machine. So I setup a docker image enabling me to run just the build process inside a docker container which itself is running in a Linux environment.

I've published the docker image on [Docker Hub](https://hub.docker.com/r/roamingthings/kotlin-native) and [Github](https://github.com/roamingthings/docker-kotlin-native) in case anybody else wants to get into  Kotlin/Native cross-compilation.

You can run a container interactively by issuing the following command:

```
docker run --rm -it -v $(pwd):/home/gradle -w /home/gradle roamingthings/kotlin-native /bin/bash
```

This will pull the image from Docker Hub, create a temporary container and put you into a shell. The current directory will be mapped as the project folder.

If you would like to use [Gradle](https://gradle.org) inside the container you may also want to map the `.gradle` and `.konan` folders so the dependencies and plugins only get downloaded once.

```
docker run --rm -it -v $(pwd):/home/gradle -v $HOME/.konan:/home/gradle/.konan -v $HOME/.gradle:/home/gradle/.gradle -w /home/gradle roamingthings/kotlin-native /bin/bash
```

Enjoy!

