# 39C3 TROPIC01 workshop

This workshop shows you how to program the TROPIC01 chip on the CDC Badge Nametag. We focus specifically on TROPIC01, using the badge as a convenient devboard rather than programming the entire CDC Badge.

CDC Badge uses an ESP32-S3 as the host processor. It can be programmed directly with the Espressif toolchain, but in this workshop we use the [libtropic-arduino](https://github.com/tropicsquare/libtropic-arduino) library through the PlatformIO extension in VS Code. 

This simplifies the initial setup and makes the workshop accessible to people with different skill levels—with the PlatformIO extension in VS Code, you don't need to worry much about the toolchain.

This workshop is in C (C++) programming language. We somewhere have also a Rust library [in progress](https://github.com/tropicsquare/libtropic-rs), but that's a different story :)

# Getting ready

Follow these steps:

* Install Visual Studio Code
* In VS Code, install the PlatformIO extension
* If your machine doesn't have CMake, install it
* Create a new project for esp32-s3-devkitc-1 (the badge is based on this devkit)
* Compile the empty project

# Preparing the environment

Assuming you successfully compiled the empty project during the "Getting ready" phase, add the following code to your platformio.ini file:

```
build_flags =
    -D ARDUINO_USB_MODE=1
    -D ARDUINO_USB_CDC_ON_BOOT=1
lib_deps =
    https://github.com/tropicsquare/libtropic-arduino.git
upload_port = /dev/ttyACM0
monitor_port = /dev/ttyACM0
monitor_speed = 9600
```

These lines tell PlatformIO to download the TROPIC01 library, and the build flags ensure that debug output is routed through the USB serial port.

Your environment is now ready. PlatformIO will automatically fetch the TROPIC01 library in the background and store it in the `.pio/` folder.

# Adding the code

All code for this workshop is already written—you just need to copy/paste it and make minor changes.

Go to the [libtropic-arduino](https://github.com/tropicsquare/libtropic-arduino) GitHub repository and copy one example from the `examples/` folder.

Five examples are available:
* Hello World
* ECDSA P256
* EdDSA Ed25519
* R Memory
* MAC And Destroy

You can check them all step by step, but after copying the code, you need to make a few changes:

Because the badge has TROPIC01 wired on different pins than the defaults, every example needs slight modifications:

1. Update the chip select pin:
```
#define TROPIC01_CS_PIN 10
```

Why? The example code expects GPIO5 by default, but the badge uses GPIO10 as the chip select pin.

2. Change these lines:
```
Tropic01 tropic01(TROPIC01_CS_PIN
#if LT_USE_INT_PIN
                  ,
                  TROPIC01_INT_PIN
#endif
#if LT_SEPARATE_L3_BUFF
                  ,
                  l3_buffer, sizeof(l3_buffer)
#endif
);
```

to this:

```
Tropic01 tropic01(TROPIC01_CS_PIN, esp_random(), SPI);
```

Why? The badge has TROPIC01's SPI wired on non-default pins, so we need to pass a redefined SPI class to the TROPIC01 constructor. When passing a custom SPI object, you must also define a random source—that's why we pass esp_random().
The SPI pin redefinition happens in the next step.

3. Add this line:
```
SPI.begin(12,11,13,-1);
```
just before this line:

```
returnVal = tropic01.begin();
```

Why? We're setting up custom SPI pins just before calling .begin(), which uses SPI.


# Building the code

To build the code, click the PlatformIO extension icon on the left to open the `Project tasks` submenu. Click `build` to build the project. You can also use `Clean` to remove previous builds, or `full clean` to remove builds and re-download dependencies.

# Flashing and running the code

In the PlatformIO menu on the left, under `Project tasks`, click `upload and monitor`. The code will be uploaded and you should see messages from TROPIC01.

Check and possibly change the `serial_port` setting in the platformio.ini file. The badge communicates over serial port—check how your system enumerated the badge and update `serial_port` accordingly.


# Resources

* [libtropic-arduino](https://github.com/tropicsquare/libtropic-arduino) - Arduino library used in this workshop
* [TROPIC01](https://github.com/tropicsquare/tropic01) - Chip documentation and specifications
* [MAC And Destroy app note](https://github.com/tropicsquare/tropic01/blob/main/doc/application_notes/ODN_TR01_app_002_pin_verif_1v1.pdf) - Documentation on our unique pin verification engine
* [CDC Badge Nametag firmware](https://github.com/riatlabs/cdc-badge-nametag) - Badge firmware with multiple applications (work in progress)

# Support

We're here to fix the chip industry and love answering your questions!

* **Support Desk**: [support.desk.tropicsquare.com](https://support.desk.tropicsquare.com)
* **GitHub Issues**: Open an issue in any of our repositories
* **Workshop**: Ask questions during the workshop

