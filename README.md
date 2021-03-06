# Composite USB Device library

This project implements a platform-independent, highly flexible USB Device software framework,
which allows you to create a full-feature USB 2.0 device firmware 
with multiple independent interfaces.

## Features

* Effective compliance to USB 2.0 specification
* Interfaces are independent of the device and can be added or removed in runtime
* Interface classes support multiple instantiation
* All USB descriptors are created internally (no need for user definition)
* Code size optimized for resource-constrained systems
* Platform-independent stack

### Supported device classes

* Communications Device Class (**CDC**-ACM) specification version 1.10
* Human Interface Device Class (**HID**) specification version 1.11 - with helper macros for report definition
* Mass Storage Class Bulk-Only Transport (**MSC**-BOT) revision 1.0 with transparent SCSI command set
* Device Firmware Upgrade Class (**DFU**) specification version 1.1
  (or DFU STMicroelectronics Extension [(DFUSE)][DFUSE] 1.1A
  using `USBD_DFU_ST_EXTENSION` compile switch)

## Contents

The project consists of the followings:
1. The USB 2.0 device framework is located in the **Device** folder.
2. Common USB classes are implemented as part of the project, under the **Class** folder.
3. The *Templates* folder contains example files which are mandatory for the library's functionality.
4. The *Doc* folder contains a prepared *doxyfile* for Doxygen documentation generation.

## Platform support

Currently the following hardware platforms are supported:
- STMicroelectronics [STM32][STM32] using the [STM32_XPD][STM32_XPD] peripheral drivers
(a [standalone][standalone] solution is also possible)

## Basis of operation

This stack on one side is called by the user application to start or stop the device
using the public API in *usbd.h*. On the other side the stack shall be notified when any of 
these device peripheral events occur:
- USB Reset signal on bus -> `USBD_ResetCallback()`
- USB control pipe setup request received -> `USBD_SetupCallback()`
- USB endpoint data transfer completed -> `USBD_EpInCallback()` or `USBD_EpOutCallback()`

The goal of the USBD structures is to be a common management structure for both this stack 
and the peripheral driver. Any additional fields that the peripheral driver necessitates
can be defined in the driver-specific *usbd_pd_def.h* header, while the *usbd_types.h* shall
be included by the driver.

## Projects using USBDevice

### [DfuBootloader][DfuBootloader]

A generic USB device bootloader firmware for STM32 controllers.
USB device with a single DFU interface
which is mountable on both the bootloader's and the application's device stack.

### [DebugDongle][DebugDongle]

A debug serial port with selectable output power and battery charging.
Composite USB device with one CDC (serial port),
two HID interfaces (onboard sensors and power management)
and the bootloader's DFU interface.

### [CanDybug][CanDybug]

A CAN bus gateway which uses a custom protocol over a USB serial port emulation.
Composite USB device with CDC function and the bootloader's DFU interface.

## How to contribute

This project is free to use for everyone as long as the license terms are met. 
Any found defects or suggestions should be reported as a GitHub issue.
Improvements in the form of pull requests are also welcome.

## Authors

* **Benedek Kupper** - [IntergatedCircuits](https://github.com/IntergatedCircuits)

[CanDybug]: https://github.com/IntergatedCircuits/CanDybugFW
[DebugDongle]: https://github.com/IntergatedCircuits/DebugDongleFW
[DfuBootloader]: https://github.com/IntergatedCircuits/DfuBootloader
[DFUSE]: http://www.st.com/resource/en/application_note/cd00264379.pdf
[standalone]: https://github.com/IntergatedCircuits/USBDevice/wiki/Integration-for-STM32-without-XPD 
[STM32]: http://www.st.com/en/microcontrollers/stm32-32-bit-arm-cortex-mcus.html
[STM32_XPD]: https://github.com/IntergatedCircuits/STM32_XPD