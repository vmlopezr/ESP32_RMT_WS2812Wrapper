# ESP32_RMT_WS2812Wrapper
 Wrapper Class for WS2812 use with ESP32 based on previous work done by JSchaenzle at https://github.com/JSchaenzle/ESP32-NeoPixel-WS2812-RMT.

 The functions are written for use with FreeRTOS on an ESP32 NodeMCU development board. This was written using PlatformIO, as such it uses the arduino framework for ESP32.

## Development

The class was written using PlatformIO IDE and the associated header files for the ESP32 platform. This repository includes the [platformio.ini](./platformio.ini) for the board information.

### Command Line Interface

To build a platformIO project via command line, [PlatformIO Core](https://docs.platformio.org/en/latest/core/index.html)
is needed. Once installed the following commands may be used:

```
# Build project
$ platformio run

#upload firmware
$ platformio run --target upload

```

**NOTE:** The target microcontroller must be recognized by the computer as a
serial device in order for PlatformIO to recognize it when uploading.

- On Windows this is detected as a COM PORT in the device manager
- On both Mac and Linux, virtual COM PORT are usually recognized under /dev/.
  - Use `ls /dev/tty*` to view the recognized USB devices.

### PlatformIO IDE

The ESP32 software was written and developed using [PlatformIO IDE](https://docs.platformio.org/en/latest/integration/ide/pioide.html).

This package can be installed on VS Code, and aims to provide a cross platform
build system for embedded, IoT applications. It integrates PlatformIO Core, so
once installed, the command line interface will be available after adding the executables'
directory to PATH. See [PlatformIO's Install Shell Commands](https://docs.platformio.org/en/latest/core/installation.html#piocore-install-shell-commands) for more information.

## How to Use

Create a Esp32CtrlLed object. Note that when using the empty constructor, the `setPin` and `updateLength` functions need to be called after.

Use updateLength to increase or decrease the size of the data based on the amount of Leds needed. The size can be updated at run-time.

### Example
```cpp
#include "Esp32CtrlLed.h"

int main(){
    Esp32CtrolLed matrix;

    // Using a 10 x 10 LED matrix
    matrix.updateLength(100);
    matrix.setPin(GPIO_NUM_26);
    matrix.ESP32_RMT_Init();

    // Clear display
    matrix.resetLeds();
    for(int index = 0; index < 100; index++){
        // Set all leds to red, input color in 0x00RRGGBB format
        matrix.setPixelRGB(index, 0x00FF0000);
    }
    matrix.write_leds();
    return 0;
}
```
## Limitations

The library only supports use of up to 1200 LEDS. Due to the use of the RMT peripheral in ["Esp32CrlLed.h"](./Esp32.CtrlLed.h), an array of size _n_ _x_ 24 _x_ 4 bytes is needed, where *n* is the total number of LEDs.

Due to the Max Heap allocation limit of about 117KB, a display of 1200 LEDs is the largest possibley via LED strips, the application may support up to 1200 LEDS.

There are other libraries that support WS2812b LEDs such as [FastLED](https://github.com/FastLED/FastLED) , Adafruit's [NeoPixel Library](https://github.com/adafruit/Adafruit_NeoPixel).
The following link illustrates a way to minimize the amount of RAM needed for a pixel, by generating the
data signal via bit-banging on a GPIO pin.

https://wp.josh.com/2014/05/13/ws2812-neopixels-are-not-so-finicky-once-you-get-to-know-them/

**NOTE:** The library written in the article uses some assembly code for the arduino's
AVR architecture. To use this idea with the ESP32, the code would need to be adapted.
