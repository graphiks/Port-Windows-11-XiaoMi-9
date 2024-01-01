<img align="right" src="https://raw.githubusercontent.com/graphiks/woa-raphael/65c0ee06045c13d1ef0f5f88aa687c50274ef7f5/raphael.png" width="350" alt="Windows 11 Running On A Redmi 9T Pro">


# Running Windows on the Redmi K20 Pro / Mi 9T Pro

## Dualboot guide

> [!NOTE] 
> There are two methods, use whichever one suits your situation the most. Method 1 requires root, while method 2 does not.

### Prerequisites
- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [TWRP](https://dl.twrp.me/raphael/twrp-3.7.0_9-0-raphael.img.html)

- [UEFI image](https://github.com/graphiks/woa-raphael/releases/tag/raphael2)

- [WOA Helper app](https://github.com/graphiks/woa-raphael/raw/main/woa-helper-raphael.apk)

- [StA Installer](https://github.com/graphiks/woa-raphael/raw/main/StA_Installer_raphael.exe)


## Method 1 - root required
> This method requires root. Use method 2 if you aren't rooted.

This guide assumes you are rooted, if you aren't, please follow [this guide](root.md) first.

### Setup - Android
- Create a folder named "UEFI" on your internal storage and place the UEFI image here
- Download both the WOA Helper app as well as the StA Installer
- Install and open WOA Helper and allow any root access it wants
- Press the "BACKUP ANDROID BOOT" button, which will back up your boot image to /sdcard/boot.img (internal storage)
- Also press the "Mount/Unmount Windows" button to mount Windows to /sdcard/Windows
- Move/copy the StA Installer and boot.img to /sdcard/Windows
- Return to the WOA Helper app and press "Quickboot to Windows"

### Setup - Windows
- Navigate to C:\StA_installer_raphael.exe and run it. If it doesn't work, make sure that any antivirus software is off, as it will probably not let the app run.

##### Booting to android
  - Run the new shortcut on your desktop as **ADMINISTRATOR**

##### Booting to windows
  - Run the app
  - Press "Quickboot to windows"
  
## Finished!



## Method 2 - no root required
> This method does not require root, but is not really recommended as it is quite slow and unreliable.

> [!NOTE]
> If you followed the main guide, you should already have done this, making the following steps obsolete.

- Boot to TWRP and back up your Android boot image using the Backup button.
> Name this backup "Android"

- Flash the Windows UEFI (xiaomi-raphael.img) to the boot partition, then make another backup of your boot image.
> Name this backup "Windows"

##### Switching between Android and Windows
- Shutdown / reboot your device and boot to TWRP. Restore the boot backup of whichever operating system you would like to load, then reboot.

## Finished!
