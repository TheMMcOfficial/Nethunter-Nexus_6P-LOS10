# Nethunter on a Nexus 6P using LineageOs (Android 10)
Instructions to get Nethunter on a Nexus 6P Using Android 10 (LineageOS). You should also be able to use the internal wifi card in monitor mode and maybe packages injection using Nexmon. 

## Prerequisites:
A Nexus 6P, all the files to install/flash, a computer with the ADB tool installed on it. 

For the Nexmon Tools and the Lineage OS version you can find them here: https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-devices/-/issues/236

Magisk APK: https://magisk.me/apk/

Magisk ZIP: https://magisk.me/zip/

Nethunter: https://www.kali.org/kali-linux-nethunter/



## How to install Nethunter and LineageOS

1- boot/reboot The Nexus 6P in fastboot 
```adb reboot bootloader```

2- flash the recovery partition
```fastboot flash recovery twrp-3.3.1.0-FBE-10-angler.img```

3- boot the phone in TWRP

4- flush system, data and cache partition in TWRP

5- reboot in the bootloader
```adb reboot bootloader```

6- flash radio (radio-angler-angler-03.88.img), vendor (vendor-angler-opm7.181205.001.img), bootloader (bootloader-angler-angler-03.84.img)
```
fastboot flash radio radio-angler-angler-03.88.img
fastboot flash vendor vendor-angler-opm7.181205.001.img 
fastboot flash bootloader bootloader-angler-angler-03.84.img
```

7- boot in TWRP again and push the OS
```adb push lineage-17.1-20200819-UNOFFICIAL-angler.zip /sdcard/```

8- install it in TWRP

9- reboot in bootloader and push the vendor.squashfs

```
adb reboot bootloader
fastboot flash vendor vendor.squashfs
```

10- In TWRP do those commands

```
adb push fstab.angler /sdcard/
adb shell "twrp mount /system_root && twrp remountrw /system_root && cp /sdcard/fstab.angler /system_root/"
```

11- format data in TWRP

12- reboot the system and setup android

13- Install Magisk (take note this version of Magisk may be out of date you should take the latest release)

```adb push Magisk-v21-2.zip /sdcard/```

14- boot to Android and install the MagiskManager app

```adb push MagiskManager-v8.0.1.apk /sdcard/```

15- reboot to TWRP and install nethunter (the version of Nethunter may be out of date get the last one)

```adb push nethunter-2020.4-angler-los-ten-kalifs-full.zip /sdcard/```

16- copy the new libs over the existing

```
adb push libnexmon* /sdcard/
adb shell "su -c 'mount -o rw,remount / && cp /sdcard/libnexmonkali.so /system/lib64/kalilibnexmon.so && cp /sdcard/libnexmon.so /system/lib64/'"
```

for the last command we need to give the root permission for adb in setting->developer tools and in Magisk

```
adb push nexutil /sdcard/
adb shell "su -c 'mount -o rw,remount / && cp /sdcard/nexutil /system/xbin/'"
adb shell "su -c 'chmod +x /system/xbin/nexutil && chmod +x /system/lib64/kalilibnexmon.so && chmod +x /system/lib64/libnexmon.so'"
```

17- For the support of multiple wifi card go to magisk and install nethunter-magisk-wifi-firmware by rithvikvibhu. You can see the list of card supported here: https://github.com/rithvikvibhu/nh-magisk-wifi-firmware


## Sources:
https://forum.xda-developers.com/nexus-6p/development/rom-kali-nethunter-huawei-nexus-6p-oreo-t4079087/

https://forum.xda-developers.com/nexus-6p/orig-development/rom-lineageos-17-0-nexus-6p-angler-t4012099

https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-devices/-/issues/236

https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-devices/-/issues/242
