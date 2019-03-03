+++
author = "Alexander Sparkowsky"
categories = ["Java", "BLE", "Bluetooth", "Raspberry Pi", "JNI", "IoT"]
date = 2016-06-02T01:32:29Z
description = ""
draft = false
slug = "raspberry-pi-ble-and-java"
tags = ["Java", "BLE", "Bluetooth", "Raspberry Pi", "JNI", "IoT"]
title = "Raspberry Pi, Bluetooth LE and Java"

+++

When I learned about the Raspberry Pi 3 with its integrated Bluetooth 4 capabilities I got instantly excited. I find Bluetooth LE (BLE) very fascinating. It allows to spread sensor data and similar information in a very easy and standardized way. I got a board and started playing around. It turned out that the current system integration still needs some [work]. Since I'm a Java developer I wanted to do some project using Java to communicate with the _physical world_. My idea is to write a gateway collecting data from sensor nodes which send their information by using GATT profiles. A web application hosted by the Raspberry Pi connects to the server application by using web sockets to retrieve and display sensor values and updates. This project is more like a proof-of-concept to play with the various technologies and figure out how and if they are working together.

![](/content/images/2016/03/java_ble_poc-1.png)

Unfortunately also there is an implementation for [Python] there is no such thing for Java except some older projects that even don't cover BLE (e.g. [JBlueZ]). Also the underlying Bluetooth stack [bluez] seems to be hard to utilize when developing Bluetooth software.

With [Pi4J](http://pi4j.com) a great Java library for accessing the GPIO hardware already exists. I'm thinking that we need something similar to access Bluetooth LE stack on the Raspberry Pi. I have done C and some JNI coding in the past and I'm looking into implementing some kind of Java library/framework for this purpose.

To begin with I set up a remote development environment using Netbeans so I’m able to develop the JNI part in C/C\+\+ on my Mac and I’ve been able to setup a _Hello World_ project.

There is a good [tutorial](https://netbeans.org/kb/docs/cnd/remotedev-tutorial.html) on the netbeans site. I wish I could use an IDE from my favorite vendor which is [JetBrains](http://jet brains.com). Maybe Lion will eventually also be able to provide remote development capabilities like Netbeans does.

I think with the Raspberry Pi 3 BLE will gain even more importance in the creator world and it's worth the effort.

Any ideas and help is appreciated.

