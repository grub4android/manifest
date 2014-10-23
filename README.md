# GRUB4Android

## Getting Started

To get started with GRUB4Android, you'll need to get
familiar with [Git and Repo](http://source.android.com/source/using-repo.html).

To initialize your local repository using the GRUB4Android trees, use a command like this:

    repo init -u git://github.com/grub4android/manifest.git -b master

Then to sync up:

    repo sync


## Initializing a Build Environment

###  Installing required packages (Ubuntu 14.04) 64-bit
    $ sudo apt-get install git-core gnupg flex bison gperf build-essential \
      zip curl zlib1g-dev libc6-dev lib32ncurses5-dev \
      x11proto-core-dev libx11-dev lib32z-dev \
      libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown \
      libxml2-utils xsltproc autoconf grub-common qemu-user cmake vim-common \
      realpath

### Installing required packages (Ubuntu 12.04) 32-bit
    $ sudo apt-get install git gnupg flex bison gperf build-essential \
      zip curl libc6-dev libncurses5-dev:i386 x11proto-core-dev \
      libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-glx:i386 \
      libgl1-mesa-dev g++-multilib mingw32 tofrodos \
      python-markdown libxml2-utils xsltproc zlib1g-dev:i386 \
      autoconf grub-common qemu-user cmake vim-common realpath
    $ sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so


## Building
Build everything with make. GNU make can handle parallel tasks with a -jN argument, and it's common to use a number of tasks N that's between 1 and 2 times the number of hardware threads on the computer being used for the build. E.g. on a dual-E5520 machine (2 CPUs, 4 cores per CPU, 2 threads per core), the fastest builds are made with commands between make -j16 and make -j3

    $ make -j4 DEVICENAME

check the [build repository](https://github.com/grub4android/build/tree/master/devices) for a list of supported devices.

## Installation
### LK: Xiaomi Mi2(s)(c) 'aries' and Xiaomi Redmi 1s 'armani'
The file "out/DEVICENAME/lk/build-*/emmc_appsboot.mbn" needs to be flashed to the aboot partition

    $ fastboot flash aboot emmc_appsboot.mbn

Please note that a faulty LK softbricks the device. You shouldn't flash it if you don't know how to recover it.

### LK: all other devices
The resulting file "out/DEVICENAME/lkboot.img" needs to be flashed on the boot or recovery partition of the device.

    $ fastboot flash boot lkboot.img

### GRUB
The GRUB filesystem "out/DEVICENAME/grub/grub_rootfs"needs to be installed on the device defined in the device's makefile.
Usually this is /data/media/boot or /sdcard/boot.
Afterwards you need to change the permissions of the files for security reasons.

    $ chmod -R 0644 ../boot

## Debugging
LK support several commands. Just type "fastboot oem help" to get a list.
Also you can build a sideload image of GRUB which boots GRUB with fastboot enabled.

    $ make DEVICENAME grub_sideload_image
    $ fastboot boot out/DEVICENAME/grub/grub_sideload.img
