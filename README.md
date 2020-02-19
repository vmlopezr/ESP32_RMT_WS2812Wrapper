# ESP32_RMT_WS2812Wrapper
 Wrapper Class for WS2812 use with ESP32 based on previous work done by JSchaenzle at https://github.com/JSchaenzle/ESP32-NeoPixel-WS2812-RMT.

 The functions are written for use with FreeRTOS on an ESP32. This was written using PlatformIO, as such it uses the arduino framework for ESP32.

## How to Use

Create a Esp32CtrlLed object. Note that when using the empty constructor, the `setPin` and `updateLength` functions need to be called after.

Use updateLength to increase or decrease the size of the data based on the amount of Leds needed.
