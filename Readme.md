# LetMeCreate library

## Build status

Master
[![Build Status](https://travis-ci.org/francois-berder/LetMeCreate.svg?branch=master)](https://travis-ci.org/francois-berder/LetMeCreate)


Dev
[![Build Status](https://travis-ci.org/francois-berder/LetMeCreate.svg?branch=dev)](https://travis-ci.org/francois-berder/LetMeCreate)

## Introduction

This library is a collection of small wrappers for some interfaces of the Ci40. It aims at making easier to develop on this platform. Also, it provides some wrappers for a few clicks. Notice that you cannot compile the library on Ci40 because cmake cannot run on it.

Interface supported:
  - I²C
  - SPI
  - UART
  - LED's
  - Switch
  - GPIO (Mikrobus and Raspberry Pi interfaces)
  - PWM
  - ADC

MikroClick board supported:
  - 7Seg
  - 8x8R (Led Matrix)
  - Accel
  - ADC
  - Air quality
  - Alcohol
  - Bargraph
  - CO
  - Color
  - Color2
  - EVE
  - GYRO
  - IR distance
  - IR eclipse
  - Joystick
  - Motion
  - Opto
  - Proximity
  - Relay (partial support)
  - Relay2
  - Relay4 (partial support)
  - Thermo3

Examples are installed in /usr/bin/letmecreate_examples.
Tests are installed in /usr/bin/letmecreate_tests.

## Contributing to LetMeCreate

The master branch is the stable branch and the dev branch is merged to master just before doing a release. All pull requests must be done on dev branch.
Before creating a wrapper for a click board, make sure that it is not already supported in the dev branch. To avoid any overlap, contact me my email (fberder@outlook.fr).


About code style, I follow roughly the [linux kernel coding style](https://www.kernel.org/doc/Documentation/CodingStyle) (two notable exceptions: indentation is set to 4 spaces and no 80 characters limit per line). Keep the style of your wrapper consistent with the rest of the code. For instance, most functions return 0 if they succeed and a negative number if not.


Each wrapper must implement features provided by only one click board. Do not mix code related to different click boards in a wrapper. Add doxygen documentation to all of your functions and do not forget to add a doxygen header on top of your header file otherwise the documentation will not be generated by doxygen.


Keep examples very simple and avoid parsing arguments. Examples are there to show one or two features of a **single** click board. Do not use multiple click boards in an example. The idea behind writing examples is to show how easy the library makes it using click boards. Hence, the shorter the example is, the better it is.

## Integration in Openwrt

To add new packages, Openwrt relies on feeds: a collection of packages.

### Installation steps

Clone the library and openwrt somewhere on you computer:

```sh
$ mkdir ci-40
$ cd ci-40
$ git clone https://github.com/CreatorDev/openwrt.git
$ mkdir -p custom/letmecreate
$ cd custom/letmecreate
```

#### Stable release

If you are only interested in getting the latest release of LetMeCreate library, then download a copy of Makefile.stable and Config.in.stable located in miscellaneous folder. Copy these files inside the letmecreate folder you have just created and rename it to Makefile and Config.in respectively.

#### Development configuration

If you are interested in modifying the library, getting the lastest changes, then clone it:

```sh
$ git clone https://github.com/francois-berder/LetMeCreate.git
```

Copy the Makefile and the Config.in to the right location:
```sh
$ cp LetMeCreate/miscellaneous/Makefile.devel Makefile
$ cp LetMeCreate/miscellaneous/Config.in.devel Config.in
```

This project uses two branches. The dev branch contains all the latest changes and should not be considered as stable. The dev branch is sometimes merged to master once new features are considered stable.

#### Register the library in Openwrt

To register the feed in openwrt, go back in openwrt folder and open feeds.conf.default.
Add this line:
```
src-link custom /change/this/path/to/the/location/of/ci-40/custom/directory/
```

Update and install all feeds:
```sh
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
```
In make menuconfig, select Libraries, you should see an entry for letmecreate library:

![Libraries menu](/miscellaneous/libraries_menu.png)

Select the letmecreate library in make menuconfig, and compile Openwrt:

```sh
$ make -j1 V=s
```
In the image (a tarball in bin/pistachio), you should see the examples in /usr/bin/letmecreate_examples.

To compile only the library (only possible once you built Openwrt once):

```sh
$ make package/letmecreate/{clean,compile} -j1 V=s
```
