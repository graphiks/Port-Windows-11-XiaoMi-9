<img align="right" src="https://cdn.discordapp.com/attachments/546427343045132298/1205180951454552135/cepheusnew.webp" width="350" alt="Windows 11 running on cepheus">

# Running Windows on the XiaoMi 9 (Cepheus)

## Reinstall guide
> [!NOTE]
> This guide is used whenever you want to update or change your windows and / or driver installation.

### Prerequisites
* [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img) (should already be installed)
* [DriverUpdater](https://github.com/qaz6750/XiaoMi9-Drivers/tree/main/tools)
* [Msc script](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/msc.sh)

## Reinstalling Windows
> [!IMPORTANT]
> If you have come here from the [troubleshooting page](troubleshooting-en.md) because you need to reinstall/update drivers, you can skip some steps. Make sure to read the guide properly.

##### Boot to TWRP
> If your recovery has been replaced with the stock recovery, flash it again in fastboot with:
```cmd
fastboot flash recovery path\to\twrp.img reboot recovery
```

##### Pushing the msc script
Put msc.sh in the platform-tools folder, then run:
```cmd
adb push msc.sh /
```

##### Running the msc script
```cmd
adb shell sh msc.sh
```

## Diskpart
>  [!WARNING]
> DO NOT ERASE ANY PARTITION WHILE IN DISKPART!!!! THIS WILL ERASE ALL OF YOUR UFS!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for taking the device to Xiaomi or flashing it with EDL, both of which will likely cost money)

After you hear your phone getting reconnected to your PC run:
```cmd
diskpart
```
> Run the following commands in the newly opened window

##### Finding your phone
```cmd
lis dis
```
> This will show all available disks. Find the disk number of your phone and replace it with "$" in the command below
```cmd
sel dis $
```

##### Selecting the ESP partition
```cmd
lis par
```
> This will print out all of the partitions in the selected disk. Check if they match up with your device and replace "$" with the number of the ESP partition (usually 30 or 31)
```cmd
sel par $
```

##### Formatting the ESP partition
> Skip this step if you are only reinstalling/updating drivers, or you will have to also reapply the image.
> This will format ESP to fat32
```cmd
format quick fs=fat32 label="System"
```

##### Add letter Y to the ESP partion
```cmd
assign letter y
```

##### Selecting the Windows partitiom
> Replace "$" in the command below with the number of the Windows partition, usually 31 or 32. If you don't know the number, run "lis par" again
```cmd
sel par $
```

##### Formatting the Windows partition
> Skip this step if you are only reinstalling/updating drivers, or you will have to also reapply the image.
> This will format Windows to NTFS
```cmd
format quick fs=ntfs label=Windows
```

##### Add letter X to the Windows partion
```cmd
assign letter x
```

##### Exit diskpart
```cmd
exit
```

## Installing Windows
> [NOTE]
> Skip this step if you are only reinstalling/updating your drivers

> Replace `path\to\install.esd` with the actual path to install.esd.

> If you are using an ISO file, the image file is located in the sources folder inside the ISO. Mount the ISO with Windows Explorer and then copy the path to it.

> Replace `index:6` with `index:1` if your image is not from the link in this guide.

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```



## About Choosing the Right UEFI
### Enable SecureBoot
> [!NOTE]
> - Under normal conditions, please use MuCepheusSecureBoot.img
### Disable SecureBoot
> [!NOTE]
> - If you need to enable testing mode, please use MuCepheusDisableSecureBoot.img
> - If you need to start systems like Linux, you also need to disable secure boot
> - And it requires disable driver signature checks

##### Create Windows bootloader files
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```
###### Configuring bootloader files
* Run these 5 commands seperately
```cmd
cd Y:\EFI\Microsoft\Boot
```
```cmd
Y:
```
```cmd
bcdedit /store BCD /set "{default}" testsigning on
```
```cmd
bcdedit /store BCD /set "{default}" nointegritychecks on
```
```cmd
bcdedit /store BCD /set "{default}" recoveryenabled no
```

## Unassign disk letters
> So that they don't stay there after disconnecting the device
```cmd
diskpart
```

##### Select the Windows volume of the phone
> Use `lis vol` to find it, it's the one named "Windows"
```diskpart
select volume <number>
```
##### Unassign the letter X
```diskpart
remove letter x
```

##### Select the ESP volume of the phone
> Use `list volume` to find it, it's the one named "ESP"
```diskpart
select volume <number>
```
##### Unassign the letter Y
```diskpart
remove letter y
```

##### Exit diskpart
```diskpart
exit
```

## Backing up boot images

##### Reboot your recovery
> To remove the msc script
- Reboot to recovery through TWRP, or run
```cmd
adb reboot recovery
```

##### Push the UEFI to your phone
> Drag and drop the UEFI (xiaomi-raphael.img) to your phone

##### Back up your Android boot image
Use the TWRP backup feature to backup your Android boot image. Name this backup "Android"

##### Flash the UEFI
Use the TWRP install feature to flash the UEFI image to your boot partition. Select "install image", then locate the image.

##### Back up your Windows boot image
Use the TWRP backup feature to backup your Windows boot image. Name this backup "Windows"

## Boot into Windows
After having flashed the UEFI image, reboot your phone.

Your device will now set up Windows. This will take some time. It will eventually reboot, and after that the initial setup (oobe) should launch.

## Setting up Windows
> You will have to run the limited setup because Wi-Fi does not work during boot.

To do this, open the accessibility menu and open the on-screen keyboard, then press SHIFT + F10 to open CMD where you will run
```cmd
oobe/bypassnro
```
Your device will now reboot. Finish setup after it boots back up. Make sure to press the "I don't have internet" button during setup.

After windows finishes booting, you may notice thay USB does not work. To fix this, enable USB host mode using the optional [post install guide](postinstall.md).

After doing this, press the restart button and force boot to TWRP with the button combination after the screen shuts off.


## [Next step: Setting up dualboot](/guide/dualboot.md)
