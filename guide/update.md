# Running Windows on the XiaoMi 9 (Cepheus)

## Update Drivers and UEFI guide

### Prerequisites
* [DriverUpdater](https://github.com/qaz6750/XiaoMi9-Drivers/tree/main/tools) 
* [Msc script](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/msc.sh)
* [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img)
* Please select the corresponding DriverUpdater download based on the computer CPU architecture

### Entering mass storage mode

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
##### Selecting the Windows partitiom
> Replace "$" in the command below with the number of the Windows partition, usually 31 or 32. 
> Under normal circumstances, it is usually the last partition. If you don't know the number, run "lis par" again.
```cmd
sel par $
```

##### Assign Windows partition drive letters
> Now add letter X to the Windows partition
```cmd
assign letter x
```

##### Exit diskpart
```cmd
exit
```

## Get Driver
> [!NOTE]
> - To ensure the matching between UEFI and drivers, we recommend that all users download drivers directly from Releases
> - If a new driver is released, you should be able to see the new Releases
> - If your device is not using the latest driver, you can download it at this time. If you are using the latest driver, you do not need to download and update the driver


* You can get the released version through [Releases](https://github.com/qaz6750/XiaoMi9-Drivers/releases) 

## Installing the drivers
* Going to Mass Storage
* Assign drive letters to Windows and EFI of your phone
* Extract the drivers, Extract driver updater, and from the command prompt in the DriverUpdater.X86.exe(Or DriverUpdater.AMD64.exe or DriverUpdater.X86.exe) directory:

```
DriverUpdater.X86.exe -d "<path to extracted drivers>\definitions\Desktop\ARM64\Internal\cepheus.xml" -r "<path to extracted drivers>" -p X:\
```
> [!NOTE]
> - You may see some errors caused by appx, it's okay, please continue to the next step.

## About Choosing the Right UEFI
* Please download UEFI updates together when downloading driver updates
### Enable SecureBoot
> [!NOTE]
> - Under normal conditions, please use MuCepheusSecureBoot.img
### Disable SecureBoot
> [!NOTE]
> - If you need to enable testing mode, please use MuCepheusDisableSecureBoot.img
> - If you need to start systems like Linux, you also need to disable secure boot
> - And it requires disable driver signature checks

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
##### Exit diskpart
```diskpart
exit
```
## Boot into Windows
##### Push the UEFI to your phone
* Normally, you should use MuCepheusSecureBoot.img
* Drag and drop the UEFI (MuCepheusSecureBoot.img Or MuCepheusDisableSecureBoot.img) to your phone.
* Please use the updated UEFI.

##### Flash the UEFI
* Use the TWRP install feature to flash the UEFI image to your boot partition. Select "install image", then locate the image.

##### End
* It should eventually boot up to Windows normally
* Thank you to everyone