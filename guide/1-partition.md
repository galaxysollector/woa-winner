<img align="right" src="https://github.com/galaxysollector/woa-winnerx/blob/main/winnerx.png" width="350" alt="Windows 11 running on winnerx">

# Running Windows on the SAMSUNG GALAXY FOLD 5G SM-F907N

## Partitioning your device

### Prerequisites
- A brain (most important of all)

- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)
  
- [Modded TWRP](https://github.com/galaxysollector/woa-winnerx/releases/tag/Recovery)


### Notes
> [!WARNING]  
> Do not run the same command twice unless specified.
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/woa_msmnile_issues).
> 
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!

### Flash the modded TWRP
> If you have the official TWRP installed, flash it through there. Otherwise use Odin.

#### Unmount all partitions
Go to TWRP > Mount > and unmount all partitions

#### Running parted
```cmd
adb shell /sbin/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, userdata should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **34**
```cmd
rm $
```

#### Creating ESP partition
```cmd
mkpart esp fat32 11.7GB 12GB
```

#### Creating Windows partition
> Replace **312GB** with the value you want to be allocated to Windows
```cmd
mkpart win ntfs 12GB 312GB
```

#### Creating WinPE partition
> [!Note]
> This is optional

> Replace **312GB** with the end value of Windows
>
> Replace **325GB** with the end value you want WinPE to have
```cmd
mkpart pe fat32 312GB 325GB
```

#### Recreating userdata
> Replace **312GB** with the end value of the Windows or WinPE partition you created above
```cmd
mkpart userdata ext4 312GB 512GB
```

#### Make ESP bootable
> Replace **$** with the number of the **ESP** partition, which should be **34**
```cmd
set $ esp on
```

#### Exit parted
```cmd
quit
```

#### Format data
Reboot to recovery, go to the Wipe menu and press Format Data, 
then type `yes`.

### Check if Android still starts
Restart the phone, and see if Android still works

## [Next step: Installing Windows](2-install.md)

















