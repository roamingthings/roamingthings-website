+++
author = "Alexander Sparkowsky"
categories = ["Synology", "letsencrypt"]
date = 2017-07-11T02:32:09Z
description = ""
draft = false
slug = "manually-refresh-lets-encrypt-on-a-synology-nas"
tags = ["Synology", "letsencrypt"]
title = "Manually refresh let's encrypt on a Synology NAS"

+++

I'm using a https certificate from [Let's Encrypt](https://letsencrypt.org) to securely connect to my NAS and other servers (like this one). The software running on my Synology NAS automatically generates and renews this certificate for me after it has been setup once.

One important thing though is that port 80 and 443 are reachable from the internet. I recently misconfigured my router so that the request was not getting thorough and I had to / wanted to start the renewal manually. After digging around I found that you can trigger the renewal process by ssh-ing onto the NAS and issue the following command (this is tested with version 6.1 of the Synology OS):

```
# /usr/syno/sbin/syno-letsencrypt renew-all
```

