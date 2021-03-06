---
layout: rom
title: LineageOS 17.1 Android TV (Android 10)
subtitle: for Raspberry Pi 4
date: 2020-08-15
tags: [rpi4, LineageOS, LOS17.1, AndroidTV]
social-share: true
comments: true
---

Here's my build of LineageOS 17.1 Android TV for Raspberry Pi 4 Model B. It is unofficial and unsupported by the LineageOS team. It's for **advanced users** only. Pi 4 model with at least 2GB of RAM is required to run this build.

<span style="color:#FF0000;">Important!</span> This image includes parts that are licensed under non-commercial license ([Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](http://creativecommons.org/licenses/by-nc-sa/4.0/)). You may use this build freely in personal/educational/etc use. Commercial use is not allowed with this build!

![screenshot]({{ site.url }}/img/rpi4/LineageOS17.1-ATV/Screenshot_20200815-143204_Settings.png)

<span style="color:#FF0000;">Do not mirror my builds!</span> Please post a link to this page instead.

**lineage-17.1-20200815-UNOFFICIAL-KonstaKANG-rpi4-atv.zip**  
[https://www.androidfilehost.com/?fid=8889791610682915596](https://www.androidfilehost.com/?fid=8889791610682915596)  
md5:1e595d88c55b532b672223208d9c73ee

**Working:**

- Audio (HDMI, 3.5mm jack, USB microphones, bluetooth speakers/headsets, etc)
- Audio DAC (using PCM512x DACs e.g. Hifiberry DAC+)
- Bluetooth
- Camera (using official Pi camera modules & UVC USB webcams)
- GPIO
- GPS (using external USB modules e.g. U-Blox 7)
- Ethernet
- Hardware accelerated graphics (V3D)
- HDMI display
- I2C
- IR remotes (using external GPIO IR modules e.g. TSOP4838)
- RTC (using external GPIO I2C modules e.g. DS3231)
- Serial console (using external GPIO serial console adapters e.g. PL2303)
- SPI
- Touchscreen/multi-touch (using official 7" display with SwiftShader software renderer)
- USB (mouse, keyboard, storage, etc)
- Wifi
- Wifi tethering

**Not working:**

- Hardware video decoding & encoding (software decoding & encoding works)

**Issues:**

- Chromecast functionality is not supported on uncertified Android TV devices
- Official remote control app is not working
- Stock camera app is not working - many third party camera apps seem to work
- Wrong color format is used in some games (You can workaround this by disabling hw overlays. Settings -> Device Preferences -> Developer options -> Disable HW overlays or add 'hwc.drm.use_overlay_planes=0' to /system/build.prop for more permanent solution. This will of course decrease performance slightly.)
- SELinux is in permissive mode
- and more...

**Sources:**

- [kernel](https://github.com/lineage-rpi/android_kernel_brcm_rpi/tree/lineage-17.1)

**Thanks:**

- peyo-hd and everyone who has contributed to android-rpi
- brobwind for bluetooth fixes
- GloDroid for graphics fixes
- Eric Anholt for V3D graphics driver
- Google for Android Things platform
- Android-x86 project
- LineageOS team & everyone who has contributed to LineageOS 17.1

----

**How to install:**

1. Follow the official Raspberry Pi instructions for writing the image to the SD card ([Linux](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md), [Mac](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md), [Windows](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md)).

**FAQ:**

Q: I can't find developer options, why?  
*A: Settings -> Device Preferences -> About -> Click 'Build number' several times. You need to 'drag' the settings menu to reach the 'About tablet' section that is last on the list.*

Q: How can I enable root access?  
*A: You can enable root access under Settings -> Device Preferences -> Developer options -> Root access. LineageOS no longer has built-in root management for applications. You should keep this option disabled at all times when you are not using an app that explicitly requires root access.*

Q: Settings -> Storage shows total system size of 7 GB. There's unallocated space on my sdcard. What should I do?  
*A: This is a 7 GB image, remaining space on your sdcard will remain unallocated. You can use e.g. GParted and extend /data partition (/dev/block/mmcblk0p4) to cover the unallocated space. Resizing the partition manually will break support for encrypting /data. Format /data in TWRP recovery (Wipe->Format data) after resizing to leave required space for crypto footer.*

*Or you can flash my [resize](https://www.androidfilehost.com/?fid=8889791610682901036) zip in TWRP.*

Q: My display is not working. I can only see the rainbow screen but no Android boot animation. What should I do?  
*A: This build only supports HDMI displays that report supported resolutions using EDID. See [this page](https://www.raspberrypi.org/documentation/configuration/config-txt/video.md) under 'Which values are valid for my monitor?' to see how to check which resolutions your display supports using Raspbian. 1920x1080 resolution is used by default with this build. If your display doesn't support 1920x1080 resolution, you can try changing value in /boot/resolution.txt to something it does.*

Q: I have official 7" LCD display and touchscreen. What should I do?  
*A: Official 7" touchscreen is only supported using SwiftShader software renderer. See below how to switch between different graphics drivers. You will also need to change display size under Settings -> Display -> Display size (or change ro.sf.lcd_density to 120 in /vendor/build.prop) to adapt to the smaller resolution.*

Q: I need to use SwiftShader software renderer to use camera, the official 7" display, or I want to boot without any display connected. What should I do?  
*A: <span style="color:#FF0000;">Warning</span>, SwiftShader is a software renderer and using it affects graphics performance. You can switch between MESA and SwiftShader graphics drivers by executing following commands in 'adb shell'/serial console/terminal (you can enable built-in terminal app from Settings -> Device Preferences ->  Developer options -> Local terminal):*

```
su
rpi4-graphics.sh swiftshader
```
```
su
rpi4-graphics.sh mesa
```

Q: Raspberry Pi doesn't have power button, how do I power off/reboot my device?  
*A: Following keyboard keys work as Android buttons: F1 = Home, F2 = Back, F3 = Multi-tasking, F4 = Menu, F5 = Power, F11 = Volume down, and F12 = Volume up. You can also use one of many third party reboot applications.*

Q: How to create a DIY hardware power button?  
*A: You can send power button events by connecting GPIO21 to ground.*

![fritzing]({{ site.url }}/img/rpi4/LineageOS17.1/powerbutton.png)

*Execute following commands in 'adb shell'/serial console/terminal (you can enable built-in terminal app from Settings -> Device Preferences ->  Developer options -> Local terminal) to enable the feature and reboot your device:*

```
su
rpi4-powerbutton.sh
```

*You can also use the DIY power button to boot the device to TWRP recovery. Press and hold the button while powering on the device until you see the TWRP screen. If you use this method to boot to recovery, you can ignore what is said about booting into/out of TWRP later in the FAQ.*

Q: How to enable audio through 3.5mm jack?  
*A: Execute following commands in 'adb shell'/serial console/terminal (you can enable built-in terminal app from Settings -> Device Preferences ->  Developer options -> Local terminal) to enable the feature and reboot your device:*

```
su
rpi4-audio-jack.sh
```

Q: How to boot to TWRP recovery?  
*A: There's currently no proper way to boot between Android and TWRP, but this can achieved by renaming the ramdisk you want to boot. Execute following commands in 'adb shell'/serial console/terminal (you can enable built-in terminal app from Settings -> Device Preferences ->  Developer options -> Local terminal) and reboot your device:*

```
su
rpi4-recovery.sh
```

*If mouse cursor doesn't appear, try replugging your mouse.*

Q: How to boot out of TWRP recovery?  
*A: Flash my [recovery2boot](https://www.androidfilehost.com/?fid=8889791610682901035) zip in TWRP.*

Q: How to update from previous LineageOS 17.1 build without losing data?  
*A:*

1. Boot to TWRP recovery with the build you want to keep the data (see FAQ)
2. Plug in an external USB storage device and select 'Backup'
3. Use 'Select Storage' to choose the USB device and 'Swipe to backup' (it's only necessary to backup the data partition so you can uncheck other partitions to speed up the process)
4. Write new LineageOS 17.1 image to the sdcard following installation instructions
5. Boot to TWRP recovery with the new build (see FAQ)
6. Select 'Restore' and find the backup you created from the USB device ('Select Storage')
7. Make sure you only have data selected as partitions to restore (uncheck other partitions if available) and select 'Swipe to Restore'
8. (Flash Google apps package/other add-ons you had previously installed)
9. Boot out of recovery (see FAQ)

Q: How to install Google apps?  
*A:*

1. Download [open_gapps-arm-10.0-tvstock-xxxxxxxx.zip](https://opengapps.org/?arch=arm&api=10.0&variant=tvstock) and save it to your device's internal storage or use an external USB drive
2. Boot to TWRP recovery (see FAQ)
3. Install open_gapps-arm-10.0-tvstock-xxxxxxxx.zip from your selected storage
4. Wipe->Factory reset!
5. Boot out of recovery (see FAQ)

----

[Merged commits](https://review.lineageos.org/#/q/status:merged++branch:lineage-17.1+-project:%255E.*device.*+-project:%255E.*kernel.*,n,z) not mentioned in the changelog.

**15.8. changelog:**

- initial Android TV build

----

**Previous builds:**

- [AndroidFileHost](https://www.androidfilehost.com/?w=files&flid=315544)

----
