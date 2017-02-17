+++
subtitle = ""
title = "Discovering PlatformIO"
bigimg = ""
date ="2015-06-02T01:20:20+02:00"

+++

The RaspberryPi / Arduino combo kit is a winner option when prototyping an IoT-style project.

The [Raspberry Pi](https://www.raspberrypi.org/), as a cheap, low-powered, full-blown Linux machine, can interact with other systems via network connectivity and harness the power of high-level languages and abstractions, whereas it can offload the duty of interfacing the circuitry to the Arduino, which is in turn best suited for this tasks. The [Arduino](http://www.arduino.cc/) board can efficiently read analog signals, has better voltage/current protection and can take advantage of the myriad of *shields* and modules available in the market to expand its functionality.

This role-delegation approach can range from [reading a continuous flow of sensor data](http://pybonacci.org/2014/01/19/leer-datos-de-arduino-desde-python/) to [controlling the I/O pins](https://github.com/lekum/pyduino) of the board loading a generic *sketch* in Arduino. The communication between the Raspberry Pi and the Arduino is based on a simple serial connection (USB A to B).

Regardless of the option chosen, it is very convenient that we could upload the sketches to the Arduino directly from the Raspberry Pi, without the burden of  unplugging one from another and loading the Arduino IDE in a laptop to compile and upload the sketch. And, at that point, **I found a big problem**.

<!-- TEASER_END -->

The Arduino IDE binaries available at the official page are only for `i686` and `x86_64` architectures. My Raspberry Pi is the old model `armv6` (the new multi-core ones are `armv7`). Debian does not support fully support this architecture ([*The Raspberry Pi 1's processor falls uncomfortably between the processor families that Debian has chosen to target*](https://wiki.debian.org/RaspberryPi)) so I installed [Arch](http://archlinuxarm.org/platforms/armv6/raspberry-pi), that does have a decent support for this architecture. However, I did not find Arduino IDE built for this processor family, even in the [AUR](https://aur.archlinux.org/).

Luckily, on the same page of the Arch wiki, I found a project called [PlatformIO](http://platformio.org)... that promises **support for ARM!**. As their website states, it is a *cross-platform code builder and library manager for different boards and platforms*.

Installing it on an Arch machine is a very straightforward process. First, we need to install Python 2 and virtualenv2 (because Arch uses Python 3 -yeah!- by default).

Then, we create a virtualenv, activate it and install `platformio` and its dependencies ([sCons](http://www.scons.org/)):

```
$ virtualenv --python=python2 pio
$ source ./pio/bin/activate
$ pip install platformio && pip install --egg scons
```

Then, we create a folder, we enter it and we run:

```
$ platformio init --board=uno
```

Then, after answering a couple of configuration questions:

```
Would you like to enable firmware auto-uploading when project is successfully built using `platformio run` command? 
Don't forget that you can upload firmware manually using `platformio run --target upload` command. [y/N]: y


The current working directory /home/ex/example will be used for project.
You can specify another project directory via
`platformio init -d %PATH_TO_THE_PROJECT_DIR%` command.

The next files/directories will be created in /home/alex/kk
platformio.ini - Project Configuration File. |-> PLEASE EDIT ME <-|
src - Put your source code here
lib - Put here project specific or 3-rd party libraries
Do you want to continue? [y/N]: y

Project has been successfully initialized!
Useful commands:
`platformio run` - process/build project from the current directory
`platformio run --target upload` or `platformio run -t upload` - upload firmware to embedded board
`platformio run --target clean` - clean project (remove compiled files)
```

The on-screen instructions are pretty clear. We have initialized a project targeted to the Arduino `uno` board and it has created several files and subfolders inside the current folder:

```
.
├── lib
├── platformio.ini
└── src
```

The file `platform.ini` stores the settings of the project, including the target architecture:

```
# Project Configuration File
[env:uno]
platform = atmelavr
framework = arduino
board = uno
targets = upload
```

The subfolders `src` and `lib` are the place to store the sketch and libraries, respectively, for your project. Once we have placed them inside, we run:

```
$ platformio run
```
An interactive prompt asks for permission to install the `atmvelavr` platform support. Once we say *yes*, it compiles our project and uploads it to the Arduino Uno (yeah!).

As a bonus, the gui-less and command-line oriented interface of this tool is a perfect match for a [Continuous Integration](http://docs.platformio.org/en/latest/ci/index.html) system.

Happy hacking!
