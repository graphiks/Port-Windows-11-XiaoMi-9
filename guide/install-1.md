# Running Windows on the XiaoMi 9 (Cepheus)

## Installation

## Partitioning your device

### Prerequisites
* A brain (most important of all)
* Please ensure that the MIUI system or underlying system used is the latest locally available
* [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img)
* [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
* [Parted](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/parted)

### Notes
> [!WARNING]  
> If you ever delete any partitions via diskpart, Windows will send a UFS command which would erase the entire UFS storage!
> 
> All your data will be erased! Backup now if needed.
> 
> Do not run the same command twice.
> 
> DO NOT REBOOT YOUR PHONE if you think you made a mistake, ask for help in the [Telegram chat](https://t.me/WinOnMi9).
> 
>
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

> [!IMPORTANT]
> Make sure you use the provided TWRP Recovery throughout this whole tutorial as this will make it easier for you.
> 
> If you don't use it and you face any errors, consider it your fault and consider yourself alone if you try asking for support as you have deferred from the main guide.

##### Flash TWRP recovery
> Open a CMD window inside the platform-tools folder, then (while your phone is in fastboot mode) run
```cmd
fastboot flash recovery path\to\twrp.img reboot recovery
```

> [!IMPORTANT]
> If you do anything incorrectly, you may wipe your UFS and render your device unusable.

Use TWRP now to back up your Modem and EFS partition (as well as anything else if you have important data). Move this backup to a safe place (e.g your PC) as the next steps will wipe your data.

### Partitioning guide
> Your XiaoMi 9 may have different storage sizes. This guide uses the values of the 128GB model as an example. When relevant, the guide will mention if other values can or should be used.

##### Extending the partition limit
```cmd
adb shell sgdisk --resize-table=128 /dev/block/sda
```

##### Preparing for partitioning
> Download the parted file and move it in the platform-tools folder, before then running these commands seperately
```cmd
adb push parted /cache/
```

```cmd
adb shell "chmod 755 /cache/parted"
```

```cmd
adb shell /cache/parted /dev/block/sda
```

##### Printing the current table partition:
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

##### Resizing userdata
> Replace $ with the number of the userdata partition
```cmd
resizepart $
```
> Parted will now ask you for the end value.
> You can choose the size you want, as long as it is lower than the value it provides to you. In this example we resize it to 32GB
```cmd
End? [122GB]? 32GB
```
Note: 122GB is parted telling us the maximum end value we can select.

##### Checking free space
```cmd
p free
```

##### Creating ESP partition
> 32GB is the **End** of the **userdata** partition in this example and 32.5GB is the end of the ESP partition we will be creating, which will be 500MB in size

> Replace 32GB with the actual value you used when resizing the partition, then add 0.5 to this value and use it for the second value
```cmd
mkpart esp fat32 32GB 32.5GB
```

> Replace "$" with your ESP partition number, usually 30 or 31
```cmd
set $ esp on
```

##### Creating Windows partition
> In this example 122GB is the end value of your phone's total storage. Replace this with the actual end value you see when executing "p free"

> 32.5GB in this example is the end of ESP, replace this with your actual value accordingly as well
```cmd
mkpart win ntfs 32.5GB 122GB
```

##### Exit parted
```cmd
quit
```

##### Formatting data
Format all data in TWRP, or Android will not boot.
( Go to Wipe > Format data > type yes )

#### Check if Android still starts
Just restart the phone, and see if Android still works


## [Next step: Installing Windows](/guide/install-2.md)
