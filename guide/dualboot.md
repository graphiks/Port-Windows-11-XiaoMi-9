<img align="right" src="https://cdn.discordapp.com/attachments/546427343045132298/1205180951454552135/cepheusnew.webp" width="350" alt="Windows 11 running on cepheus">
  
# Running Windows on the XiaoMi 9 (Cepheus)

## Dualboot guide

> [!NOTE] 
> There are two methods, use whichever one suits your situation the most. Method 1 requires root, while method 2 does not.

### Prerequisites
- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [TWRP](https://github.com/graphiks/woa-raphael/releases/download/raphael-partitioning/twrp.img) (should already be installed)
- [UEFI image](https://github.com/graphiks/woa-raphael/releases/download/raphael-uefi/xiaomi-raphael.img)
- [WOA Helper app](https://github.com/graphiks/woa-raphael/releases/download/raphael-dualboot/woa-helper-raphael.apk)
- [StA Installer](https://github.com/graphiks/woa-raphael/releases/download/raphael-dualboot/StA_Installer_raphael.exe)

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
