# ESP32-PWM-Driver-Devkit
## ESP32 Development kit with 8 PWM outputs that can be driven at 5-24V at up to 2A each

### Board and Pinout:
![Board Top and Pinout](https://github.com/vintlabs/ESP32-PWM-Driver-Devkit/raw/master/VintLabsESP32_PWM_Driver_Pinout.png)

The PWM driver board is powered by an ESP32 and features 8 PWM outputs that can power LED lighting at 5-24V at up to 2A each. It can also be used as a general-purpose ESP32 development kit.

The primary purpose of the design is to control LED lighting (like these: https://www.amazon.ca/gp/product/B00QN4X5MM/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1  I am not affiliated with this seller, but the LED strips are great!), however there may be many other uses, like controlling DC motors! The board can be powered by either USB or by connecting 5-24V to the power input connector (connecting both is fine!), however do note that to use the PWM outputs an external power source is strongly recommended, as otherwise it will try to use the power from the USB bus and likely would cause power/stability issues.

### Schematic:
RevA: https://github.com/vintlabs/ESP32-PWM-Driver-Devkit/raw/master/ESP32-PWM-Driver-Devkit-RevA-schematic.pdf
RevB: https://github.com/vintlabs/ESP32-PWM-Driver-Devkit/raw/master/ESP32-PWM-Driver-Devkit-RevB-schematic.pdf

### Connecting via USB
#### Linux
If the device is not detected, you may need to add `CONFIG_USB_SERIAL_CP210X=m` to your kernel config:
```
Device Drivers --->
	USB Support --->
		USB Serial Converter Support --->
			<M>   USB CP210x family of UART Bridge Controllers  
```

If it's detected, you should see the following in /var/log/messages (may be syslog or other file depending on your distro):
```
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: new full-speed USB device number 35 using ehci-pci
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: New USB device found, idVendor=10c4, idProduct=ea60, bcdDevice= 1.00
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: Product: CP2102N USB to UART Bridge Controller
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: Manufacturer: VintLabs
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: SerialNumber: 0002
Dec 31 17:57:51 nautilus kernel: cp210x 1-2.4:1.0: cp210x converter detected
Dec 31 17:57:51 nautilus kernel: usb 1-2.4: cp210x converter now attached to ttyUSB0
```


#### Windows
See https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

### Usage in Arduino environment
To use with Arduino, ensure that you have ESP32 boards added (if you have not, see https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/ for some good instructions), and then select **VintLabs ESP32 DevKit** in *Tools->Board*. (If you do not see it in the list, please make sure that the ESP32 core is updated)

Example code: `git clone https://github.com/vintlabs/VintLabs-PWM-Arduino-Basic-Example.git`
```
// PWM Example for VintLabs ESP32 PWM Driver DevKit
// Connect an LED light from PWM0 on the board to ground (if using a discrete LED use a current limiting series resistor!)

// Select the PWM output to use - you can use PWM0-PWM7 for the VintLabs boards, or simply specify the GPIO number
const int pwmOutput = PWM0;

// Set the PWM frequency
const int frequency = 10000;

// Set the LED channel. Note that this is not related to a pin number, but an arbitrary channel number from 0-7
const int ledChannel = 0;

// Set the resolution. This can be 8, 10, 12 or 15 bits
const int resolution = 12;


void setup() {
  // Basic PWM setup
  ledcSetup(ledChannel, frequency, resolution);

  // Attach the actual output to the channel
  ledcAttachPin(pwmOutput, ledChannel);

}

void loop() {
  // Just loop from off to max, then back again
  for (int dc = 0; dc < pow(2, resolution); dc++)
  {
    // set the duty cycle
    ledcWrite(ledChannel, dc);
    delay(10);
  }
  for (int dc = pow(2, resolution) - 1; dc >= 0; dc--)
  {
    // set the duty cycle
    ledcWrite(ledChannel, dc);
    delay(10);
  }
}
```
