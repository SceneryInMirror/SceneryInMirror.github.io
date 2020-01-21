---
layout: post
title: "Raspberry Pi. Re"
img: raspberrypi.jpg
date: 2018-04-13 14:24:00
category: blog
author: ykx
description: How to use RPi.GPIO package; how to start OLED by SPI; how to use PIL package(especially Image, ImageDraw, ImageFont) to draw something on the OLED.
tag: [Computer, RPi]
---

python包安装：pip install [package]

python帮助文档：pydoc [package]


## RPi.GPIO

参考文献：[RPi.GPIO的wiki：https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/](https://sourceforge.net/p/raspberry-gpio-python/wiki/Home/)

图片来源：[RPi.Pinout：https://blog.csdn.net/coder9999/article/details/76904625](https://blog.csdn.net/coder9999/article/details/76904625)

### Demo Code

### Instructions List 
* ([] means it depends on your settings, / means those are options, {} means it is optional)
* GPIO.setmode([mode])
* GPIO.getmode()
* GPIO.setwarnings(False)
* GPIO.setup([channel/chan_list], GPIO.IN/GPIO.OUT, {initial=GPIO.HIGH})
* GPIO.input([channel])
* GPIO.output([channel/chan_list], [state])
* GPIO.cleanup(), GPIO.cleanup([channel/chan_list])
* [GPIO.RPI_INFO, GPIO.VERSION() (refer to wiki)](https://sourceforge.net/p/raspberry-gpio-python/wiki/BasicUsage/)


### Importing the module

```Python
# To import the RPi.GPIO module:
import RPi.GPIO as GPIO

# To import the module and check to see if it is successful:
try:
    import RPi.GPIO as GPIO
except RuntimeError:
    print("Error importing RPi.GPIO!  This is probably because you need superuser privileges.  You can achieve this by using 'sudo' to run your script")
```

### Pin numbering

```Python
# To set the numbering system:
GPIO.setmode(GPIO.BOARD)
GPIO.setmode(GPIO.BCM)

# To detect the numbering system:
mode = GPIO.getmode()
```


<img src="https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/RPi_pinout.jpg?raw=true" width = "700" height = "400" alt="RPi_pinout" />

If you are confused about the numbers of these systems, you can use **gpio readall** in terminal to show it like this:

<img src="https://github.com/SceneryInMirror/SceneryInMirror.github.io/blob/master/assets/img/gpio_readall.png?raw=true" width = "700" height = "400" alt="GPIO_readall" />

### Warnings

```Python
# To disable warnings while more than one script/circuit on the GPIO of RPi:
GPIO.setwarnings(False)
```	

### Setup channel(s)

```Python
# [channel] depends on the numbering system you choose
	
# To configure a channel as an input:
GPIO.setup([channel], GPIO.IN)

# To set up a channel as an output:
GPIO.setup([channel], GPIO.OUT)
	
# Specify an initial value of output channel:
GPIO.setup([channel], GPIO.OUT, initial=GPIO.HIGH)
	
# Set up more than one channel per call:
chan_list = [11, 12]  # You can tuples instead
GPIO.setup(chan_list, GPIO.OUT)
```

### Input & Output

```Python
# To read the value of a GPIO pin:
GPIO.input([channel])

# TO set the output state of a GPIO pin:
GPIO.output([channel], [state])

# State can be 0/GPIO.LOW/False or 1/GPIO.HIGH/True

# Output to several channels
chan_list = [11, 12]
GPIO.output(chan_list, GPIO.LOW)  # Set all to GPIO.LOW
GPIO.output(chan_list, (GPIO.HIGH, GPIO.LOW))  # Set first HIGH and second LOW
```	

### Cleanup

```Python
# It is good practice to clean up any resources you might have used. By returning all channels you have used back to inputs with no pull up/down, you can avoid accidental damage to your RPi by shorting out the pins. Note that this will only clean up GPIO channels that your script has used. Note that GPIO.cleanup() also clears the pin numbering system in use.
# To clean up channels you used at the end of script:
GPIO.cleanup()

# Clean up individual channels, a list or a tuple of channels:
GPIO.cleanup([channel])
GPIO.cleanup((channel1, channel2))
GPIO.cleanup([channel1, channel2])
```


## SSD1306

## PIL
