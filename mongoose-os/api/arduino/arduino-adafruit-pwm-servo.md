# Adafruit PWM servo
| Github Repo | C Header | C source  | JS source |
| ----------- | -------- | --------  | ----------------- |
| [mongoose-os-libs/arduino-adafruit-pwm-servo](https://github.com/mongoose-os-libs/arduino-adafruit-pwm-servo) | &nbsp; | &nbsp;  | [api_pwm_servo.js](https://github.com/mongoose-os-libs/arduino-adafruit-pwm-servo/blob/master/mjs_fs/api_pwm_servo.js)         |

Arduino Adafruit - 16-Channel 12-bit PWM/Servo Driver - I2C interface - PCA9685 library for Mongoose OS
=========

Tested and works on esp8266/esp32
## Description

16-Channel 12-bit PWM/Servo Driver – I2C interface – PCA9685 Module lets you add more IOs to your microcontroller boards. The Module has chainable I2C interface meaning for even more IOs more boards can be daisy chained. Its and excellent product for Robots that require lots of Servo motors to control using single microcontroller.See [product page link](https://www.adafruit.com/product/815) for more information about the hardware.

![alt text](https://cdn-shop.adafruit.com/970x728/815-06.jpg)
### Features

* It is 5V compliant, which means you can control it from a 3.3V microcontroller and still safely drive up to 6V outputs (this is good for when you want to control white or blue LEDs with 3.4+ forward voltages)
* 6 address select pins so you can wire up to 62 of these on a single i2c bus, a total of 992 outputs – that’s a lot of servos or LEDs
* Adjustable frequency PWM up to about 1.6 KHz
* 12-bit resolution for each output – for servos, that means about 4us resolution at 60Hz update rate
* Configurable push-pull or open-drain output
* Output enable pin to quickly disable all the outputs
* Reverse polarity protection on the terminal block input
* Power-good LED
* 3 pin connectors in groups of 4 so you can plug in 16 servos at once
* A big capacitor on the V+ line
* 220 ohm series resistors on all the output lines to protect them, and to make driving LEDs trivial
* IO expander chip: PCA9685PW
* Operating Voltage: 2.3V – 5.5V
* Solder jumpers for the 6 address select pins
* I2C 7-bit address Range: between 0x40-0x7F, selectable with jumpers

### Usage

mos.yml, add:
```
config_schema:
 - ["i2c.enable", true]
libs:
 - origin: https://github.com/mongoose-os-libs/arduino-adafruit-pwm-servo
  ```
init.js, add:
```
load('api_pwm_servo.js');
```
main.c, add:
```
#include "mgos_arduino_PWMServoDriver.h"
```

### Slave address
* Board 0: Address = 0x40 Offset = binary 00000 (no jumpers required) 
* Board 1: Address = 0x41 Offset = binary 00001 (bridge A0 ) 
* Board 2: Address = 0x42 Offset = binary 00010 (bridge A1) 
* Board 3: Address = 0x43 Offset = binary 00011 (bridge A0 & A1) 
* ...


 ----- 

### JS API

 --- 
#### Adafruit_PWMServoDriver.create

```javascript
Adafruit_PWMServoDriver.create()
```
Create an instance of PWM servo driver, which has the methods described
below.

Example:
```javascript
load("api_pwm_servo.js");
let myServo = Adafruit_PWMServoDriver.create();
```
#### myServo.close

```javascript
myServo.close()
```
Close a servo instance; no other methods can be called on this instance
after calling `close()`.
Return value: none.
#### myServo.begin

```javascript
myServo.begin()
```
Reset onewire and servo state.
Return value: none.
#### myServo.setPWMFreq

```javascript
myServo.setPWMFreq(freq)
```
This function can be used to adjust the PWM frequency (from 40 to 1000
Hz), which determines how many full 'pulses' per second are generated by
the IC. Stated differently, the frequency determines how 'long' each
pulse is in duration from start to finish, taking into account both the
high and low segments of the pulse.

Frequency is important in PWM, since setting the frequency too high with
a very small duty cycle can cause problems, since the 'rise time' of the
signal (the time it takes to go from 0V to VCC) may be longer than the
time the signal is active, and the PWM output will appear smoothed out
and may not even reach VCC, potentially causing a number of problems.

Return value: none.

Example:
```javascript
load("api_pwm_servo.js");
let myServo = Adafruit_PWMServoDriver.create();
myServo.begin();
myServo.setPWMFreq(100);
```
#### myServo.setPWM

```javascript
myServo.setPWM(channel, on, off)
```
This function sets the start (`on`) and end (`off`) of the high segment of
the PWM pulse on a specific channel. You specify the 'tick' value
between 0..4095 when the signal will turn on, and when it will turn of.
`channel` (a number from `0` to `15`) indicates which of the 16 PWM
outputs should be updated with the new values.

Return value: none.

Example:
```javascript
load("api_pwm_servo.js");
let myServo = Adafruit_PWMServoDriver.create();
myServo.begin();
// Generate square wave at 100 Hz
myServo.setPWMFreq(100);
myServo.setPWM(0, 0, 2047);
```
#### myServo.setPin

```javascript
myServo.setPin(channel, val, invert)
```
This is a wrapper for `myServo.setPWM()`, where `on` will always be 0,
and `off` is the `val` given to this function. Additionally, `invert`
can be set to `true` to invert the output.

Return value: none.

Example:
```javascript
load("api_pwm_servo.js");
let myServo = Adafruit_PWMServoDriver.create();
myServo.begin();
// Generate square wave at 100 Hz
myServo.setPWMFreq(100);
myServo.setPin(0, 2047, false);
```
