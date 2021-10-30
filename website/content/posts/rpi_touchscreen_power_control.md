+++
author = "Alexander Sparkowsky"
categories = ["raspberry_pi"]
date = 2019-04-14
description = "How to turn the backlight of the official 7 inch display on/off"
draft = false
slug = "rpi-touchscreen-backlight"
tags = ["raspberry_pi", "rpi", "lcd", "display"]
title = "Turning the backlight of the official Raspberry Pi Touch Screen on or off"
+++

I've setup a [https://magicmirror.builders](MagicMirror) to have some information handy when leaving my apartment. I'm using a [https://www.raspberrypi.org/](Rasspberry Pi) with the official [https://www.raspberrypi.org/products/raspberry-pi-touch-display/](Raspberry Pi Touch Display).

However I don't want to have the display backlight turned on all the time. Especially at night the backlight is rather annoying. In Addition I don't see why I should use up energy when nobody is looking at the screen.

## Turning the backlight on and off

After some research I fond that turning the display on and off can be achieved by writing 0 or 1 to a special file:

```
$ sudo sh -c 'echo "1" > /sys/class/backlight/rpi_backlight/bl_power' # Turn backlight off
$ sudo sh -c 'echo "0" > /sys/class/backlight/rpi_backlight/bl_power' # Turn backlight on
```
