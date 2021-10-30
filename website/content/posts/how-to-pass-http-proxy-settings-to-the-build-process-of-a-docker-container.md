+++
author = "Alexander Sparkowsky"
categories = ["Docker"]
date = 2017-05-24T09:32:43Z
description = ""
draft = false
slug = "how-to-pass-http-proxy-settings-to-the-build-process-of-a-docker-container"
tags = ["Docker"]
title = "How to pass HTTP proxy settings to the build process of a docker container"

+++

When building a docker image you may need http proxy settings to download artefacts from the internet.

You can pass the http proxy settings (`http_proxy`, `https_proxy`) from the host system into the docker container during build:

```
$ docker build -t <image_tag> . --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy
```

