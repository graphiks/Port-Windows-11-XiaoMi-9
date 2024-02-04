# Running Windows on the XiaoMi 9 (Cepheus)

## Installation

## Installing Windows

### Prerequisites

* [Windows on ARM64 image creator](https://uupdump.net/) |
  [Alternatively, a ready-to-go ESD](https://drive.google.com/drive/folders/1JEC2QhFTyZhnm4qdzeFANTmeqoDCbS1I?usp=drive_link) 
* [DriverUpdater](https://github.com/qaz6750/XiaoMi9-Drivers/tree/main/tools) 
* [Msc script](https://github.com/graphiks/woa-raphael/releases/download/raphael-partitioning/msc.sh)
* [TWRP](https://github.com/graphiks/woa-raphael/releases/download/raphael-partitioning/twrp.img) (should already be installed)

##### Boot to TWRP
> If rebooting on the last page has replaced your recovery back to stock, flash it again in fastboot with:
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
> This will format ESP to fat32
```cmd
format quick fs=fat32 label="System"
```
> Now add letter Y to the ESP partition
```cmd
assign letter y
```

##### Selecting the Windows partitiom
> Replace "$" in the command below with the number of the Windows partition, usually 31 or 32. If you don't know the number, run "lis par" again
```cmd
sel par $
```

##### Formatting the Windows partition
> This will format Windows to NTFS
```cmd
format quick fs=ntfs label=Windows
```
> Now add letter X to the Windows partition
```cmd
assign letter x
```

##### Exit diskpart
```cmd
exit
```

## Installing Windows
> Replace `path\to\install.wim` with the actual path to install.wim.

> If you are using an ISO file, it is located in the sources folder inside the ISO. Mount the ISO with Windows Explorer and then copy the path to it.
> Alternatively, use one of the install.esd files from the Google Drive at the top of this page. Touch doesn't seem to work on 23h2, so use 22h2.

```cmd
dism /apply-image /ImageFile:path\to\install.wim /index:1 /ApplyDir:X:\
```

## Get Driver
> [!NOTE]
> - To ensure the matching between UEFI and drivers, we recommend that all users download drivers directly from Releases

* You can get the released version through [Releases](https://github.com/qaz6750/XiaoMi9-Drivers/releases) 

## Installing the drivers
* Going to Mass Storage
* Assign drive letters to Windows and EFI of your phone
* Extract the drivers, Extract driver updater, and from the command prompt in the DriverUpdater.X86.exe(Or DriverUpdater.AMD64.exe or DriverUpdater.X86.exe) directory:

```
DriverUpdater.X86.exe -d "<path to extracted drivers>\definitions\Desktop\ARM64\Internal\cepheus.xml" -r "<path to extracted drivers>" -p X:\
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
* Normally, you should use MuCepheusSecureBoot.img
* Drag and drop the UEFI (MuCepheusSecureBoot.img Or MuCepheusDisableSecureBoot.img) to your phone.

##### Back up your Android boot image
* Use the TWRP backup feature to backup your Android boot image. Name this backup "Android"

##### Flash the UEFI
* Use the TWRP install feature to flash the UEFI image to your boot partition. Select "install image", then locate the image.

##### Back up your Windows boot image
* Use the TWRP backup feature to backup your Windows boot image. Name this backup "Windows"

##### Boot into Windows
* After having flashed the UEFI image, reboot your phone.

* Your device will now set up Windows. This will take some time. It will eventually reboot, and after that the initial setup (oobe) should launch.

## Setting up Windows
> [!IMPORTANT]
> USB will not work except if you have a powered USB hub. We can fix this after we get into the desktop.

Before continuing with setup, open the accessibility menu in the bottom right corner and enable the on-screen keyboard, then tap FN+SHIFT + F10 (if it asks you to tap somewhere to type just tap the background) which will open a command prompt, in which you will need to type:
```cmd
cd oobe
```
After that, type:
```cmd
bypassnro.cmd
```
Your device will now reboot. Continue setup as normal. Make sure to press the "I don't have internet" button when you reach the **Let's connect you to a network** section.

If you want USB working read this [post install guide](postinstall.md).


## [Next step: Setting up dualboot](/guide/dualboot.md)
