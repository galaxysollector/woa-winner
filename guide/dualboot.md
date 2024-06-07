<img align="right" src="https://github.com/galaxysollector/woa-winnerx/blob/main/winnerx.png" width="350" alt="Windows 11 running on winnerx">

# Running Windows on the SAMSUNG GALAXY FOLD 5G SM-F907N

## Dualboot guide

### Prerequisites
- [Magisk](https://github.com/topjohnwu/Magisk/releases/latest)

- [UEFI image](https://github.com/galaxysollector/woa-winnerx/releases/tag/UEFI) 

- [WOA Helper app](https://github.com/Marius586/WoA-Helper-update/releases/tag/WOA)

- [Switch To Android package](https://github.com/galaxysollector/woa-winnerx/releases/download/Files/winnerx-sta.zip)

## Dualboot guide
This guide assumes you are rooted, if you aren't, please do that first.

### Setup - Android
- Download and install the **WOA Helper** app, then open it and grant it root access.
- Open the settings inside the app and uncheck `Back up boot.img if none is detected (Windows)` (this feature does not work on the Galaxy Fold 5G due to no mount support).
- Download the **UEFI** image and place it inside the folder named `UEFI` in your internal storage.
- Press the `BACK UP BOOT IMAGE` button, then select `Android`.
- Create a folder called `sta` in your internal storage and unpack the two files from `winnerx-sta.zip` file here.
- Reboot to TWRP and mount **Windows** if it isn't already mounted.
- Move/copy the **sta** folder and the **boot.img** file from your internal storage to `/win` in the root of your device's storage.
- Flash the **UEFI** in `/sdcard/UEFI` to the boot partition to boot to Windows.
  
### Setup - Windows
- Navigate to C:\sta and create a shortcut of `sta.exe` to your desktop.

#### Booting to Android
- Run the new shortcut on your desktop (you can also pin it to your start menu / taskbar for ease of access)

#### Booting to Windows
- Press `Quickboot to Windows` inside the app, or use the newly created toggle in your quick settings panel
  
## Finished!

















