# Raspberry Pi example

## Overview

Example demonstrates how to flash ESP32 from Linux using esp_serial_flash component API. AT command firmware to be flashed to ESP32 can be found in `binaries`.
By default, `/dev/ttyUSB0` is used to communication with ESP32.

Following steps are performed in order to re-program target's memory:

1. Serial port is opened.
2. Host puts slave device into boot mode tries to connect by calling `esp_loader_connect()`.
3. Then `esp_loader_flash_start()` is called to enter flashing mode and erase amount of memory to be flashed.
4. `esp_loader_flash_write()` function is called repeatedly until the whole binary image is transferred.
5. At the end, `loader_port_reset_target()` is called to restart ESP32 and execute updated firmware.

Note: In addition, to steps mentioned above, `esp_loader_change_baudrate` is called after connection is established in order to increase flashing speed. Bootloader is also capable of detecting baud rate during connection phase, and can be changed before calling `esp_loader_connect`. However, it is recommended to start at lower speed and then use dedicated command to increase baud rate. This does not apply for ESP8266, as its bootloader does not support this command, therefore, baud rate can only be changed before connection phase in this case.

## Hardware Required

- A development board with ESP32 SoC (e.g. ESP-WROVER-KIT, ESP32-DevKitC, etc.).
- USB cable in case ESP32 board is powered from USB. ESP32 can be powered by USB as well.

## Installation

### Enable UART

In order to enable serial communication, add your user to the **dialout**  group with the following command in terminal:

```
sudo usermod -a -G dialout $USER
```

- Reboot or logout to load the new user group.

## Build and run

To compile the example:

Create and navigate to `build` directory:

```
mkdir build && cd build
```

Run cmake, build example and run example:

```
cmake .. && cmake --build . && ./linux_flasher
```

For more details regarding to esp_serial_flasher configuration, please refer to top level [README.md](../../README.md).
