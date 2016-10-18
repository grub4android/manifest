#Grub4android


##Build Requirements

*  `git make autoconf grub-mkimage quem-user unifont`

##How to get sources

* `repo init -u https://github.com/grub4android/manifest -b master && cd grub4android && repo sync`

##How to build

* `make aries -j15 | tee aries.log`

You can replace aries with your device codename if it's supported

##How to install

(from grub4android folder)
You must have CWM 6.0.4.x+ installed

* `adb reboot recovery`
* `adb push out/aries/grub/root/boot /sdcard/boot'`
* `adb shell chmod -R 0644 /sdcard/boot`
* `adb shell busybox chown -R root:root /sdcard/boot`
* `adb reboot bootloader`
* `fastboot flash aboot out/aries/emmc_appsboot.mbn'`
* `fastboot reboot`
