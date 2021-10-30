+++
author = "Alexander Sparkowsky"
date = 2017-01-21T12:28:11Z
description = ""
draft = false
slug = "how-to-configure-the-clock-generator-for-usb-in-an-atsamd21-asf"
title = "How to configure the clock generator for USB in an ATSAMD21 (ASF)"

+++

Configuring the USB port on an ATSAMD21 is well documented in the ASF documentation. 

By default the GCLK generator 0 which is the main clock will be used (`GCLK_GENERATOR_0` as defined in `usb_device_udd.c`). When this configuration is used some other services like the delay or I2C module will no longer function or have to be configured with another generator.

This example shows how to configure the USB module to use the GCLK generator 3 so that the main clock can be configured independently. I found this configuration in the [rotonde-samd21 Project on github](https://github.com/HackerLoop/rotonde-samd21).

## Clock Setup

Clock setup is done in `config/conf_clocks.h`.

### Configure system clock bus
```
/* System clock bus configuration */
#  define CONF_CLOCK_CPU_CLOCK_FAILURE_DETECT     false
#  define CONF_CLOCK_FLASH_WAIT_STATES            2
#  define CONF_CLOCK_CPU_DIVIDER                  SYSTEM_MAIN_CLOCK_DIV_1
#  define CONF_CLOCK_APBA_DIVIDER                 SYSTEM_MAIN_CLOCK_DIV_1
#  define CONF_CLOCK_APBB_DIVIDER                 SYSTEM_MAIN_CLOCK_DIV_1
#  define CONF_CLOCK_APBC_DIVIDER                 SYSTEM_MAIN_CLOCK_DIV_1
```

### Config DFLL for USB operations

```
/* SYSTEM_CLOCK_SOURCE_DFLL configuration - Digital Frequency Locked Loop */
#  define CONF_CLOCK_DFLL_ENABLE                  true
#  define CONF_CLOCK_DFLL_LOOP_MODE               SYSTEM_CLOCK_DFLL_LOOP_MODE_USB_RECOVERY
#  define CONF_CLOCK_DFLL_ON_DEMAND               true
```

### Set the main clock to run in standby

```
/* Configure GCLK generator 0 (Main Clock) */
#  define CONF_CLOCK_GCLK_0_ENABLE                true
#  define CONF_CLOCK_GCLK_0_RUN_IN_STANDBY        true
#  define CONF_CLOCK_GCLK_0_CLOCK_SOURCE          SYSTEM_CLOCK_SOURCE_OSC8M
#  define CONF_CLOCK_GCLK_0_PRESCALER             1
#  define CONF_CLOCK_GCLK_0_OUTPUT_ENABLE         false
```

### Config Clock Generator to use DFLL to clock USB

```
/* Configure GCLK generator 3 */
#  define CONF_CLOCK_GCLK_3_ENABLE                true
#  define CONF_CLOCK_GCLK_3_RUN_IN_STANDBY        true
#  define CONF_CLOCK_GCLK_3_CLOCK_SOURCE          SYSTEM_CLOCK_SOURCE_DFLL
#  define CONF_CLOCK_GCLK_3_PRESCALER             1
#  define CONF_CLOCK_GCLK_3_OUTPUT_ENABLE         false
```

### Changing the GCLK generator used by the USB module

Add the following definition to a header file (e.g. `my_usb.h`):

```
// Define USB clock generator
#  define UDD_CLOCK_GEN      GCLK_GENERATOR_3
```

And include this file _at the end_ of `config/conf_usb.h`

```
...
//! The includes of classes and other headers must be done at the end of this file to avoid compile error
#include "udi_cdc_conf.h"
#include "my_usb.h"

#endif // _CONF_USB_H_
```

