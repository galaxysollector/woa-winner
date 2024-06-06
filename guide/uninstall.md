<img align="right" src="https://github.com/galaxysollector/woa-winnerx/blob/main/winnerx.png" width="350" alt="Windows 11 running on winnerx">

# Running Windows on the SAMSUNG GALAXY FOLD 5G SM-F907N

## Uninstalling Windows

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Modded TWRP](https://github.com/galaxysollector/woa-winnerx/releases/tag/Recovery)

### Boot into the modded TWRP
> Flash it with Odin if you haven't already

#### Unmount all partitions
Go to TWRP > Mount > and unmount all partitions

### Running parted
```cmd
adb shell /sbin/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions
```cmd
print
```

#### Removing Windows partitions
> Replace **$** with the actual number of the partitions
>
> Do this for every partition after **omr**
```cmd
rm $
```

#### Recreating the userdata partition
```cmd
mkpart userdata ext4 11.7GB 512GB
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

## Finished!

















































