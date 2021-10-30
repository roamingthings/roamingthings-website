+++
author = "Alexander Sparkowsky"
categories = ["eps32", "iot"]
date = 2019-04-13
description = "Resources for using ESP-IDF and CLion to write applications on ESP32"
draft = false
slug = "esp-idf-clion-resources"
tags = ["eps32", "esp-idf", "iot", "tools", "clion", "jetbrains"]
title = "Some resources for using CLion to build ESP32 applications"
+++
In this article I share some resources I found about how to setup CLion and ESP-IDF to develop Software for the ESP32.

<!--more-->

In the past building applications for the ESP32 using ESP-IDF required the use of `make` as build tool. Unfortunately my
favoured IDE CLion only does support CMake as build system (also there is a makefile plugin).

I recently learned that ESP-IDF will also allow the usage of CMake beginning from version 3.3 onward. It will even become the default build system by version 4.0.

Here are some resources I found and that I'm currently looking at:

* [Codingfiled: ESP32, ESP-IDF, CMake & CLion](https://codingfield.com/en/2019/01/13/esp32-esp-idf-cmake-clion/)
* [ESP-IDF Documentation: Setup Toolchain for Mac OS from Scratch (CMake)](https://docs.espressif.com/projects/esp-idf/en/latest/get-started-cmake/macos-setup-scratch.html)
* [GitHub: Using ESP-IDF in Custom CMake Projects](https://github.com/espressif/esp-idf/tree/master/examples/build_system/cmake/idf_as_lib)
* [CP210x USB to UART Bridge VCP Drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)