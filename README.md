# kernel-playground #

This repository contains experiments related to Linux kernel and Raspberry Pi.

## Contents ##

* [Setting up development environment](https://github.com/mpod/kernel-playground#setting-up-development-environment)
* [Driver for BMP280 barometric pressure sensor](https://github.com/mpod/kernel-playground/tree/master/bmp280)
    * [Inspecting BMP280 sensor using i2c-tools](https://github.com/mpod/kernel-playground/tree/master/bmp280#inspecting-bmp280-sensor-using-i2c-tools)
    * [IIO buffers and triggers](https://github.com/mpod/kernel-playground/tree/master/bmp280#iio-buffers-and-triggers)
* [Driver for LSM9DS0 accelerometer, magnetometer, and gyroscope](https://github.com/mpod/kernel-playground/tree/master/lsm9ds0)
* [RTC triger for IIO subsystem](https://github.com/mpod/kernel-playground/tree/master/iio-trig-timer)
* [References](https://github.com/mpod/kernel-playground#references)


## Setting up development environment ##

It is recommended to use cross compiler for building examples from this 
repository. Makefiles are configured for `arm-none-eabi-` toolchain which can be 
downloaded from 
[here](https://learn.adafruit.com/adafruit-bmp280-barometric-pressure-plus-temperature-sensor-breakout/).  
It is important to add `bin` directory of cross compiler to `PATH` environment 
variable. 

Makefiles also assume that Linux kernel source code is available at relative 
location `../../linux-raspi`. Since the kernel modules are implemented for 
Raspberry Pi, it is recommended to use Raspbian version of Linux kernel. Clone 
Linux kernel source code with:

```
$ git clone https://github.com/raspberrypi/linux linux-raspi
```

Next step is to checkout kernel files to the kernel version used on Raspberry 
Pi. To find kernel version run following command from Raspberry Pi:

```
$ uname -a
Linux raspberrypi 4.1.17-v7+ #838 SMP Tue Feb 9 13:15:09 GMT 2016 armv7l GNU/Linux
```

In my case version `4.1.17` is used. To find right commit in git history run 
`git log` command and search for `4.1.17` string.

```
$ cd linux-raspi
$ git log --oneline --decorate --graph --all
...
| | | * | | cd14299 BCM270X_DT: Adjust overlay README formatting
| | | |/ /  
| | | * | 0108373 Revert "bcm270x_dt: Use the sdhost MMC controller by default"
| | | * |   6330c27 (HEAD, tag: rpi-bootloader-1.20160202-1) Merge remote-tracking branch 'stable/linux-4.1.y' into rpi-4.1.y
| | | |\ \  
| | | | |/  
| | | | * 2d5f6b0 Linux 4.1.17
| | | | * d17367a recordmcount: Fix endianness handling bug for nop_mcount
...
```

From git log is visible that commit `6330c27` is a merge of Raspbian branch and 
Linux kernel version `4.1.17`. That means we need to checkout commit `6330c27`.

```
$ git checkout 6330c27
```

## References ##

* I2C
    * [I2C (Inter-Integrated Circuit) Bus Technical Overview and Frequently 
      Asked 
Questions](http://www.esacademy.com/en/library/technical-articles-and-documents/miscellaneous/i2c-bus.html)
    * [Documentation/i2c](https://github.com/raspberrypi/linux/tree/rpi-4.1.y/Documentation/i2c) 
      files in the kernel tree.
    * [Linux Kernel HTML Documentation, Chapter 11. I2C and SMBus 
      Subsystem](https://www.kernel.org/doc/htmldocs/device-drivers/i2c.html)
    * [Linux Journal, I2C Drivers, Part 
      I](http://www.linuxjournal.com/article/7136)
    * [Linux Journal, I2C Drivers, Part II](http://www.linuxjournal.com/article/7252)
* Industrial I/O
    * [Linux Kernel HTML Documentation, Industrial I/O driver developer's 
      guide](https://www.kernel.org/doc/htmldocs/iio/index.html)
    * [Analog Devices, Linux Industrial I/O 
      Subsystem](https://wiki.analog.com/software/linux/docs/iio/iio)
    * [IIO, a new kernel 
      subsystem](https://archive.fosdem.org/2012/schedule/event/693/127_iio-a-new-subsystem.pdf) 
    * [Industrial I/O Subsystem: The Home of Linux 
      Sensors](https://www.overleaf.com/articles/industrial-i-slash-o/dmqjqpzswtvb/viewer.pdf)
    * [drivers/stagging/iio/Documentation](https://github.com/raspberrypi/linux/tree/rpi-4.1.y/drivers/staging/iio/Documentation) 
      files in the kernel tree.
