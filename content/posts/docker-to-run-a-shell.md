+++
author = "Alexander Sparkowsky"
categories = ["Docker", "shell", "bash"]
date = 2017-06-21T05:35:07Z
description = ""
draft = false
slug = "docker-to-run-a-shell"
tags = ["Docker", "shell", "bash"]
title = "Use a Docker container to run a shell"

+++

If you need to run a shell script in a specific environment than your current host you can use a temporary Docker container and link it to the current directory.

For example if you want to run a bash script on ubuntu you can store the script file in the current directory and start a temporary ubuntu container:
```
$ docker container run --rm -v $(pwd):/root -it ubuntu /bin/bash
```

Inside the container you can then access the script in the `/root` directory:

```
root@e3fba4084102:/# cd /root
root@e3fba4084102:~# ./myscript.sh
```

