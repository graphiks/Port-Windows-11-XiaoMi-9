<img align="right" src="https://cdn.discordapp.com/attachments/546427343045132298/1205180951454552135/cepheusnew.webp" width="350" alt="Windows 11 running on cepheus">

# Running Windows on the XiaoMi 9 (Cepheus)

## Root guide

### Prerequisites
- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [TWRP](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/twrp.img)

##### Boot TWRP
> If your recovery has been replaced back to stock, flash it again in fastboot with:
```cmd
fastboot flash recovery path\to\twrp.img reboot recovery
```
##### Backing up your boot image
> Sometimes flashing Magisk can cause a bootloop. To fix this, you'll need to restore a boot.img backup.

Use the TWRP backup feature to make a backup of the boot partition.

##### Flashing Magisk
- Flash the magisk.apk (you may have to rename it to magisk.zip) and reboot your phone. 
- Once booted, locate the Magisk app and open it.
- Follow any instructions provided. Select the direct install method if you are provided with several methods.

Your phone will now reboot, and you have succesfully rooted it.

## Finished!
