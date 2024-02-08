<img align="right" src="https://cdn.discordapp.com/attachments/546427343045132298/1205180951454552135/cepheusnew.webp" width="350" alt="Windows 11 running on cepheus">

# Running Windows on the XiaoMi 9 (Cepheus)

## Reinstall guide
> [!NOTE]
> This guide is used whenever you want to update or change your windows and / or driver installation.

### Prerequisites
* [Windows ARM64 ESD](https;//worproject.com/esd)
* Drivers (UPDATE THIS LINK)
* UEFI image (UPDATE THIS LINK)
* [DriverUpdater](https://github.com/qaz6750/XiaoMi9-Drivers/tree/main/tools) 
* [Msc script](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/msc.sh)
* [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img) (should already be installed)

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
> [!NOTE]
> Skip this step if you are only reinstalling/updating drivers, or you will have to also recreate bootloader files
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
> [!NOTE]
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
> [!NOTE]
> Skip this step if you are only reinstalling/updating your drivers

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
> [!NOTE]
> Skip this step if you are only reinstalling/updating your drivers
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

## Configuring bootloader files
> [!IMPORTANT]
> Skip this step entirely if you want to enable secureboot, or if you are only reinstalling/updating your drivers

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

##### Flash the UEFI
> [!NOTE]
> You can also restore your "Windows" boot backuo you made during the first installation, unless you changed from test mode to secureboot or vice versa

> Use the TWRP install feature to flash the UEFI image to your boot partition. Select "install image", then locate the image.

##### Booting into Windows
Just reboot.

## [Next step: Setting up dualboot](/guide/dualboot.md)
