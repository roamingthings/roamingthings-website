+++
author = "Alexander Sparkowsky"
date = 2017-01-20T13:04:42Z
description = ""
draft = false
slug = "use-j-link-to-change-the-boot-loader-protection-of-a-sam-d21"
title = "Use J-Link to change the boot loader protection of a SAM D21"

+++

I'm a big fan of the [Adafruit Feather M0](https://www.adafruit.com/products/2772) boards equipped with an ATSAMD21 Cortex-M0+ controller.
When I recently tried to write my program to the flash using a [J-Link](https://www.segger.com/jlink-debug-probes.html) debugger the write process failed.

It took me a while to find the reason in the `BOOTPROT` fuse.

## Boot loader configuration

`BOOTPROT` defines the size of the boot loader in bytes. The defined boot loader section is write protected. So to (over)write the bootloader or write a programme without using a bootloader, this value has to be set to `0x7` which leads to a bootloader size of 0 bytes.

On the _Feather M0 basic proto_ boards I used the `BOOTPROT` value has been set to `0x2` which results in the first 8k of the flash memory to be writeprotected.

The possible values are described in the SAMD21 datasheet in chapter _22.6.5. NVM User Configuration_

### Check current fuse value

Using the _J-Link Commander_ you can check the current values of the fuses:

```
J-Link> mem8 804000,10
00804000 = FA C7 E0 D8 5D FC FF FF FF FF FF FF FF FF FF FF
```
This example shows the `BOOTPROT` fuse set to `0x2` which results in a (write protected) boot loader size of 8kbytes.

```
J-Link> mem8 804000,10
00804000 = FF C7 E0 D8 5D FC FF FF FF FF FF FF FF FF FF FF
```
This example shows the `BOOTPROT` fuse set to `0x7` which turns of boot loader protection (boot loader size is 0).

## Changing the fuse value

Unfortunately I have not been able to change the value of the fuse by either using _Atmel Studio 7_ or the current version of the _J-Link Commander_ (which is version 9).

Atmel Studio would fail writing the fuse value and J-Link Commander would seem to write the byte at 0x00804000 but the write would not be persistent. Specially since the values only become valid after the next reset of the controller.

Eventually I figured out that it works, when writing more than just that one byte using J-Link Commander and reading the values from a `.mot` file.

The file basically contains the address and data to be written with a checksum at the end. You can either use the J-Link Commander or J-Link Flash (lite) utility to write the content of this file to a controller.

```
J-Link>loadfile C:\Users\demo\ATSAMD21G_ResetOptionBytes_0xFA.mot
Downloading file [C:\Users\demo\ATSAMD21G_ResetOptionBytes_0xFA.mot]...
J-Link: Flash download: Flash programming performed for 1 range (16 bytes)
J-Link: Flash download: Total time needed: 0.104s (Prepare: 0.067s, Compare: 0.006s, Erase: 0.006s, Program: 0.009s, Verify: 0.006s, Restore: 0.006s)
O.K.
```

I've prepared two files. One will set the fuse byte to `0xFF` which results in the boot loader protection to be turned off. The other file writes the byte `0xFA` which protects the first 8k of the flash memory. All other bytes are unchanged in regard to the values my feather board had when it got shipped.

_Tip_: The last byte of every line in the mot-file specifies a checksum. If you don't want to calculate it by yourself you can just load it into J-Link Flash (lite). The application will complain about a wrong checksum and give you the value it calculated by itself.
You can read more about the [SREC file format](https://en.m.wikipedia.org/wiki/SREC_(file_format)) and how to build the checksum at Wikipedia.

---
File `set_0xFA.mot`
```
S214804000FAC7E0D85DFCFFFFFFFFFFFFFFFFFFFF63
S9030000FC
```
---
File `set_0xFF.mot`
```
S214804000FFC7E0D85DFCFFFFFFFFFFFFFFFFFFFF5E
S9030000FC
```

