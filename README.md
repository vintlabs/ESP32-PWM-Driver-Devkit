# ESP32-PWM-Driver-Devkit
## ESP32 Development kit with 8 PWM outputs that can be driven at 5-24V at up to 2A each

### Board and Pinout:
![Board Top and Pinout](https://github.com/vintlabs/ESP32-PWM-Driver-Devkit/raw/master/VintLabsESP32_PWM_Driver_Pinout.png)

The PWM driver board is powered by an ESP32 and features 8 PWM outputs that can power LED lighting at 5-24V at up to 2A each. It can also be used as a general-purpose ESP32 development kit.

The primary purpose of the design is to control LED lighting (like these: https://www.amazon.ca/gp/product/B00QN4X5MM/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1  I am not affiliated with this seller, but the LED strips are great!), however there may be many other uses, like controlling DC motors! The board can be powered by either USB or by connecting 5-24V to the power input connector (connecting both is fine!), however do note that to use the PWM outputs an external power source is strongly recommended, as otherwise it will try to use the power from the USB bus and likely would cause power/stability issues.

### Usage in Arduino environment
To use with Arduino, ensure that you have ESP32 boards added (if you have not, see https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/ for some good instructions), and then select **VintLabs ESP32 DevKit** in *Tools->Board*. (If you do not see it in the list, please make sure that the ESP32 core is updated)
