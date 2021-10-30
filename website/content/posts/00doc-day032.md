+++
author = "Alexander Sparkowsky"
categories = ["100 Days of Code", "STM32", "Embedded"]
date = 2017-05-10T02:39:00Z
description = ""
draft = false
slug = "00doc-day032"
tags = ["100 Days of Code", "STM32", "Embedded"]
title = "[100 Days of Code] Day 032: May 10, 2017"

+++

**Today's Progress**: Finally setup the second "bigger" project I want to proceed. The aim is to learn about the STM32 by creating a small device based on a STM32 MCU that senses environmental information and finally transmits it over a (long range) wireless technology like LoRa or Zigbee.  
Today I setup the basic project structure by copying an I2C project from the CubeMX framework into my project structure utilizing the [stm32-cmake build chain](https://github.com/roamingthings/stm32-cmake) I've updated in the last days. The project compiles and loads on my machine and Nucleo-L476RG board and the debugger shows that the firmware is running also it's not doing anything meaningful for me right now.

**Things I've learned**: Setting up a new project based on the project structure I've previously build.

**Things I've planned for tomorrow**: Look deeper into the I2C driver from ST and try to get a smoktest running by sending any meaningful to the BME280.

**Link(s) to work**: [STM32 Environment Probe](https://github.com/roamingthings/stm32-environment-probe/commit/97ac6d6e047ba770dab885bd979270550d2c5ed8)

