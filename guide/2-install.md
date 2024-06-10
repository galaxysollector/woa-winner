<img align="right" src="https://github.com/galaxysollector/woa-winnerx/blob/main/winnerx.png" width="350" alt="Windows 11 running on winnerx">

# Running Windows on the SAMSUNG GALAXY FOLD 5G SM-F907N

## Installation

### Prerequisites
- [Windows on ARM image](https://worproject.com/esd)

- [Gdisk](https://github.com/galaxysollector/woa-winnerx/releases/tag/Gdisk)

- [Modded TWRP](https://github.com/galaxysollector/woa-winnerx/releases/tag/Recovery) (should already be installed)

### Reboot to TWRP
> Disable MTP in TWRP

#### Entering mass storage mode
```cmd
adb shell msc.sh
```

### Windows Disk Management
- In your start menu, type "disk manager" and press enter.
- Look for your phone (which should be a 512GB disk) in disk manager.
- If it says **Offline**, right click the disk and set it to **Online**.

#### Gdisk
> Open **gdisk** as an admin, then run the following command in gdisk, replacing **$** with the actual number of the disk
```cmd
\\.\physicaldrive$
```

#### Restoring the GPT table
> In **gdisk**, run these commands individually in this exact order.
>
> If `y` does not work. type `yes y` instead.
```cmd
r
```
```cmd
c
```
```cmd
y
```
```cmd
w
```
```cmd
y
```

#### Reconnect your phone
> Simply replug the cable

### Diskpart
> [!WARNING]
> DO NOT ERASE OR CLEAN ANY PARTITION WHILE IN DISKPART!!!! THIS WILL ERASE ALL OF YOUR UFS!!!! THIS MEANS THAT YOUR DEVICE WILL BE PERMANENTLY BRICKED WITH NO SOLUTION! (except for taking the device to Samsung or flashing it with EDL, both of which will likely cost money)
```cmd
diskpart
```

#### Finding your phone
> This will list all connected disks
```cmd
lis dis
```

#### Selecting your phone
> Replace $ with the actual number of your phone (it should be the last one)
```cmd
sel dis $
```

#### Listing your phone's partitions
> This will list your device's partitions
```cmd
lis par
```

### Selecting the ESP partition
> Replace $ with the partition number of ESP (should be 34)
```cmd
sel par $
```

#### Formatting ESP drive
```cmd
format quick fs=fat32 label="ESPWINNERX"
```

#### Add letter to ESP
```cmd
assign letter y
```

### Selecting the Windows partition
> Replace $ with the partition number of Windows (should be 35)
```cmd
sel par $
```

#### Formatting Windows drive
```cmd
format quick fs=ntfs label="WINWINNERX"
```

#### Add letter to Windows
```cmd
assign letter x
```

### Selecting the WinPE partition
> Skip this step if you are not using WinPE
> 
> Replace $ with the partition number of WinPE (should be 36)
```cmd
sel par $
```

#### Formatting WinPE drive
```cmd
format quick fs=ntfs label="WINPE"
```

#### Add letter to WinPE
```cmd
assign letter r
```

#### Exit diskpart
```cmd
exit
```

### Installing Windows
> Replace `<path\to\install.esd>` with the actual path of install.esd (it may also be named install.wim)

```cmd
dism /apply-image /ImageFile:<path\to\install.esd> /index:6 /ApplyDir:X:\
```

> If you get `Error 87`, check the index of your image with `dism /get-imageinfo /ImageFile:<path\to\install.esd>`, then replace `index:6` with the actual index number of Windows 11 Pro in your image

#### Load Registry Hives
```cmd
reg load HKLM\OFFLINE X:\Windows\System32\Config\System
```

#### Open Registry Editor
```cmd
regedit
```

#### Make USB usable
> In HKEY_LOCAL_MACHINE/OFFLINE/ControlSet001/Control/USB/OsDefaultRoleSwitchMode change value to 1
> 
> After, in the command line of your PC, enter
```cmd
reg unload HKLM\OFFLINE
```

#### Create Windows bootloader files
```cmd
bcdboot X:\Windows /s Y: /f UEFI
```

#### Enabling test signing
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" testsigning on
```

#### Disabling recovery
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" recoveryenabled no
```

#### Disabling integrity checks
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" nointegritychecks on
```

#### Disabling boot status policy
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" bootstatuspolicy IgnoreAllFailures
```

#### Enabling boot menu
> Only run this command if you use WinPE
```cmd
bcdedit /store Y:\EFI\Microsoft\BOOT\BCD /set "{default}" displaybootmenu yes
```

### Removing Windows recovery and disk checking

#### Remove Windows recovery (Step 1)

In File Explorer delete this file: <br />
```
# Replace W with your device Win partition letter
W:\Windows\System32\Recovery\WinRE.wim
```

#### Remove autochk executable (Step 2)

Now we need to remove autochk.exe executable so that Windows wouldn't be able to perform disk checking/fixing procedure on startup. <br />

#### Change executable permissions for deletion (Step 2.1)
By default, Windows won't let you delete protected files. <br />
To change that, we need to add our PC username to the file permissions group. <br />
<br />
(Replace W with your device Win partition letter): <br />
In ```W:\Windows\System32``` directory find ```autochk.exe``` and right click on it. <br />
Click on Properties > Security > Advanced > Owner change > (Enter your PC username). <br />
Click Add > Select a principal > (Enter your username) > Check "Full control" under basic permissions. <br />

#### Delete autochk.exe (Step 2.2)

After changing the file permissions, now you can delete:
```
# Replace W with your device Win partition letter
W:\Windows\System32\autochk.exe
```
<br />

After doing these steps, you won't have to worry about Windows messing with your UFS partition table on Samsung devices.

### Reboot to Android
> To set up dualboot

## [Last step: Setting up dualboot](/guide/dualboot.md)










