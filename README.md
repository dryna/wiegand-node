# Wiegand 26 and Wiegand 34 module for Node.js and using on Raspberry Pi
The Wiegand interface is a de facto standard commonly used to connect a card reader or keypad to an electronic entry system. 
Wiegand interface has the ability to transmit signal over long distance with a simple 3 wires connection. 
This module uses interrupt pins from Raspberry Pi to read the pulses from Wiegand interface and return the code and type of the Wiegand.

## Installation 

	npm install wiegand-node
    
### Example
<pre><code>
"use strict";

var Wiegand = require('wiegand-node');

var pinD0 = 4,  //DATA0 of Wiegand connects to RPi GPIO04 (Pin 7)
    pinD1 = 17; //DATA1 of Wiegand connects to RPi GPIO17 (Pin 11)

var wg = new Wiegand(pinD0, pinD1);

function getCode()
{
    if (wg.available())
        console.log(wg.getCode()); //Display code
}

setInterval(getCode); //Infinite loop

</code></pre>
### Enabling GPIO pins

To enable use of the Tinker Board GPIO pins, you need to perform the following steps as a one-time configuration. Once your Tinker Board has been configured, you do not need to do so again.

Note that these configuration steps must be performed on the Tinker Board itself. The easiest is to login to the Tinker Board via SSH:

```
ssh linaro@192.168.1.xxx
```

#### Create a group "gpio"

Create a Linux group named "gpio" by running the following command:

```
sudo groupadd -f --system gpio
```

If you already have a "gpio" group, you can skip to the next step.

#### Add the "linaro" user to the new "gpio" group

Add the user "linaro" to be a member of the Linux group named "gpio" by running the following command:

```
sudo usermod -a -G gpio linaro
```

If you already have added the "gpio" group, you can skip to the next step.

#### Add a "udev" rules file

Create a new "udev" rules file for the GPIO on the Tinker Board by running the following command:

```
sudo nano /etc/udev/rules.d/91-gpio.rules
```

And add the following contents to the file:

```
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport ; chmod 220 /sys/class/gpio/export /sys/class/gpio/unexport'"
SUBSYSTEM=="gpio", KERNEL=="gpio*", ACTION=="add", PROGRAM="/bin/sh -c 'chown root:gpio /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value ; chmod 660 /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value'"
```

### Enabling I2C

To enable use of the Tinker Board I2C, you need to perform the following steps as a one-time configuration. Once your Tinker Board has been configured, you do not need to do so again.

Note that these configuration steps must be performed on the Tinker Board itself. The easiest is to login to the Tinker Board via SSH:

```
ssh linaro@192.168.1.xxx
```

#### Create a group "i2c"

Create a Linux group named "i2c" by running the following command:

```
sudo groupadd -f --system i2c
```

If you already have a "i2c" group, you can skip to the next step.

#### Add the "linaro" user to the new "i2c" group

Add the user "linaro" to be a member of the Linux group named "i2c" by running the following command:

```
sudo usermod -a -G i2c linaro
```

If you already have added the "i2c" group, you can skip to the next step.

#### Add a "udev" rules file

Create a new "udev" rules file for the I2C on the Tinker Board by running the following command:

```
sudo nano /etc/udev/rules.d/92-i2c.rules
```

And add the following contents to the file:

```
KERNEL=="i2c-0"     , GROUP="i2c", MODE="0660"
KERNEL=="i2c-[1-9]*", GROUP="i2c", MODE="0666"
```

#### Final step 
Finally to apply changes to udev you need to reboot the device
```
sudo reboot
```

## Credits

Based on the [Wiegand-Protocol-Library-for-Arduino](https://github.com/monkeyboard/Wiegand-Protocol-Library-for-Arduino).
