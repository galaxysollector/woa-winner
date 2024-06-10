<img align="right" src="https://github.com/galaxysollector/woa-winnerx/blob/main/winnerx.png" width="350" alt="Windows 11 running on winnerx">

# Running Windows on the SAMSUNG GALAXY FOLD 5G SM-F907N

### Prerequisites
- [Windows on ARM image](https://worproject.com/esd)
  
- [Modded TWRP](https://github.com/galaxysollector/woa-winnerx/releases/tag/Recovery) (should already be installed)

### Reboot to TWRP
> Disable MTP in TWRP

#### Entering mass storage mode
```cmd
adb shell msc.sh
```

### Diskpart
```cmd
diskpart
```

#### Select the Windows volume of the phone
> Use `list volume` to find it, replace "$" with the actual number of **WINWINNERX**
```diskpart
select volume $
```

#### Add letter to Windows
```cmd
assign letter x
```

#### Exit diskpart
```cmd
exit
```

#### Formatting Windows
> Go to Windows Explorer > This PC and select **WINWINNERX**. Right click and format as NTFS.

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

### Boot into Windows
Reboot your phone. If you end up in Android instead of Windows, flash the UEFI again using WOA Helper.

#### Setting up Windows
> Your device will now set up Windows. This will take some time. It will eventually reboot, and after that the initial setup (oobe) should launch.

> [!Note]
> To skip the Microsoft Account login, use "g" for the email and password. Windows will then let you make a local account

## Finished!

















