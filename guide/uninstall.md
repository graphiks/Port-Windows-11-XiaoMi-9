# Running Windows on the XiaoMi 9 (Cepheus)

## Uninstallation

### Why is this needed?
If you want to uninstall windows this is used instead of deleting partitions manually to avoid human error + writing a whole dedicated guide to just uninstalling.

If you want to relock your bootloader you'll need your partition table to be stock.

### Prerequisites

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
- [gpt_both0](https://github.com/qaz6750/Port-Windows-11-XiaoMi-9/releases/download/Tools/gpt_both0.bin)

#### Uninstall instructions

##### Restore GPT
> Replace ```path\to\gpt_both0.bin``` with the path to the gpt_both0.bin file.

```cmd
fastboot flash partition:0 path\to\gpt_both0.bin
```

##### Erase userdata to avoid a bootloop and restore FS size
```cmd
fastboot -w
```
