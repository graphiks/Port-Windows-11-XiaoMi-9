<img align="right" src="https://cdn.discordapp.com/attachments/546427343045132298/1205180951454552135/cepheusnew.webp" width="350" alt="Windows 11 running on cepheus">

# Running Windows on the XiaoMi 9 (Cepheus)

## Installation

## Installing Windows

### Prerequisites

* [Windows ARM64 ESD](https;//worproject.com/esd)
* Drivers (UPDATE THIS LINK)
* UEFI image (UPDATE THIS LINK)
* [DriverUpdater](https://github.com/qaz6750/XiaoMi9-Drivers/tree/main/tools) 
* [Msc script](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/msc.sh)
* [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img) (should already be installed)

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
list disk
```
> This will show all available disks. Find the disk number of your phone and replace it with "number" in the command below
```cmd
select disk <number>
```

##### Selecting the ESP partition
```cmd
lis par
```
> This will print out all of the partitions in the selected disk. Check if they match up with your device and replace "ESP partition number" with the number of the ESP partition 
> (usually 30 or 31)
```cmd
sel par <ESP partition number>
```

##### Formatting the ESP partition
> This will format ESP to fat32
```cmd
format quick fs=fat32 label="System"
```
> Assign a letter to the ESP drive
```cmd
assign letter Y
```

##### Selecting the Windows partition
> Replace "Win partition number" in the command below with the number of the Windows partition on your phone, usually 31 or 32.
> 
> If you don't know the number, run "lis par" again.
```cmd
sel par <Win partition number>
```

##### Formatting the Windows partition
> This will format Windows to NTFS
```cmd
format quick fs=ntfs label=Windows
```
> Assign a letter to the Win drive
> 
```cmd
assign letter X
```

##### Exit diskpart
```cmd
exit
```

## Installing Windows
> Replace `path\to\install.esd` with the actual path to install.esd.

> If you are using an ISO file, the image file is located in the sources folder inside the ISO. Mount the ISO with Windows Explorer and then copy the path to it.

> Replace `index:6` with `index:1` if your image is not from the link in this guide.

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

## Installing the drivers
> [!NOTE]
> To ensure the matching between UEFI and drivers, we recommend that all users download drivers directly from [Releases](https://github.com/qaz6750/XiaoMi9-Drivers/releases)

> Extract the drivers and driverupdater, and open command prompt in the directory of the driverupdater. Replace `X86` with either `AMD64` or `ARM64` respectively depending on the machine you are running the command on (NOT YOUR CEPHEUS).

```
DriverUpdater.X86.exe -d "<path to extracted drivers>\definitions\Desktop\ARM64\Internal\cepheus.xml" -r "<path to extracted drivers>" -p X:\
```

## Create Windows bootloader files
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

## Choosing the Right UEFI
> Depending on your usage of Windows, you may want to have secureboot enabled or disabled.

With secureboot enabled, you can not update drivers while in Windows and a PC is needed to update them. With secureboot disabled, you can update drivers directly from your phone. However, with secureboot disabled, you will have a "Test Mode" watermark on the bottom right of your homescreen, and some apps or games may not run (this is however very rare)

## Configuring bootloader files
> [!IMPORTANT]
> Skip this step entirely if you want to enable secureboot

Run these 5 commands seperately
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

##### Back up your Android boot image
> Use the TWRP backup feature to backup your Android boot image. Name this backup "Android"

##### Push the UEFI to your phone
> Depending on whether you want to enable or disable secureboot, drag and drop MuCepheusSecureBoot.img or MuCepheusDisableSecureBoot.img to your phone's internal storage.
> If you want to disable secureboot, make sure you ran all commands in the "Configuring bootloader files" section

##### Flash the UEFI
> Use the TWRP install feature to flash the UEFI image to your boot partition. Select "install image", then locate the image.

##### Back up your Windows boot image
> Use the TWRP backup feature to backup your Windows boot image. Name this backup "Windows"

##### Boot into Windows
> After having flashed the UEFI image, reboot your phone.

* Your device will now set up Windows. This will take some time. It will eventually reboot, and after that the initial setup (oobe) should launch.

## Setting up Windows
> [!IMPORTANT]
> OTG may encounter issues due to USB's inability to automatically switch from charging mode to OTG mode
> We need to manually switch the USB mode later on

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
