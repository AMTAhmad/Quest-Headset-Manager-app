# Quest Manager

A menu-driven Windows Batch tool for managing one or many Meta Quest headsets from a PC using ADB and scrcpy. No typing of ADB commands is required; everything is done through numbered text menus.

> ## ⚠️ Disclaimer
>
> **This project was created with the help of AI (AI-generated code).** It is **not complete**, may contain bugs, and has only been tested on a limited setup (a single Meta Quest 3S). Features such as APK installation and multi-headset workflows have **not been fully tested yet**.
>
> Use it at your own risk. Review the scripts before running them, and keep backups of your files. The author provides no warranty of any kind. This project is not affiliated with, endorsed by, or sponsored by Meta, Google, or the scrcpy project.

## Features

- **Headset Manager**: view saved headsets with `CONNECTED` / `OFFLINE` status, connect to all saved headsets, rename, and forget one or all headsets.
- **USB Setup and Wireless ADB**: detect a headset over USB, read its permanent serial number and current Wi-Fi IP, enable Wireless ADB (`tcpip 5555`), and save or update the record so the newest IP replaces the old one.
- **Cast**: open a scrcpy window per selected headset showing a cropped right-eye view.
- **Refresh App List**: collect third-party apps from all connected headsets into one combined list, showing which headsets have each app. Optionally look up readable app names with AAPT, with a persistent label cache. A clean option removes apps that are not installed on any currently connected headset.
- **Open Installed App**: pick an app, then pick one, several, or all connected headsets to launch it on. Headsets that do not have the app are skipped with a message.
- **Install APK**: install an APK on all connected headsets (not fully tested yet).

## Requirements

- Windows 10 or 11
- `adb.exe` (Android platform-tools) placed in the project root
- `scrcpy.exe` and its files (scrcpy for Windows) placed in the project root
- Optional: `aapt.exe` from Android SDK build-tools, only needed for readable app names
- A Meta Quest headset with **Developer Mode** enabled and USB debugging authorized
- The PC and the headsets on the same Wi-Fi network for wireless use

## Folder Structure

```text
<root folder>\
├─ Quest_Manager.bat              Main launcher and main menu
├─ adb.exe
├─ scrcpy.exe
├─ (other scrcpy files: DLLs, scrcpy-server, ...)
└─ QuestManagerFiles\
   ├─ modules\                    One BAT file per feature
   │  ├─ devices.bat
   │  ├─ usb_setup.bat
   │  ├─ casting.bat
   │  ├─ apps_refresh.bat
   │  ├─ apps_launch.bat
   │  └─ installer.bat
   └─ data\                       Plain-text data files (created automatically)
      ├─ Quest_Headsets.txt
      ├─ Quest_Apps.txt
      ├─ Quest_Packages.txt
      ├─ Quest_App_Devices.txt
      └─ Quest_App_Labels.txt
```

## Installation

1. Download scrcpy for Windows and extract it. Its folder becomes the project root (`adb.exe` is bundled with scrcpy).
2. Copy `Quest_Manager.bat` into the root folder.
3. Create `QuestManagerFiles\modules` and put the module BAT files inside it.
4. Run `Quest_Manager.bat`. The `data` folder and its files are created automatically on first run.

Always run `Quest_Manager.bat`. Do not run the files in `modules` directly, because they rely on variables defined by the main file.

## First-Time Setup

1. Enable Developer Mode for the headset in the Meta mobile app.
2. Connect the headset to the PC with a USB cable and put the headset on to approve the USB debugging prompt.
3. Run `Quest_Manager.bat` and choose **[2] USB SETUP AND WIRELESS ADB**.
4. Give the headset a name (for example `H1`). The tool saves the serial, the name, and the current Wi-Fi IP.
5. Unplug the cable. Next time, use **[1] HEADSET MANAGER** and choose **Connect all saved headsets**.

If the headset's IP changes (for example after a reboot), plug in the USB cable and run **USB SETUP** again. The permanent serial number identifies the headset, and the saved IP is updated.

## Usage

Main menu:

| Option | Action |
|---|---|
| 1 | Headset Manager |
| 2 | USB Setup and Wireless ADB |
| 3 | Cast selected headsets |
| 4 | Open installed app |
| 5 | Refresh app list |
| 6 | Install APK on connected headsets |
| 0 | Exit |

Menu conventions:

- Enter `0` to go back or cancel.
- When choosing headsets, enter `A` for all, or numbers separated by commas such as `1,3,4`.
- Run **Refresh App List** before using **Open Installed App**.

## Data Files

All data files are plain text with `|` as the separator and no header line.

`Quest_Headsets.txt`:

```text
<USB serial>|<name>|<wireless ip:port>
```

`Quest_Apps.txt`:

```text
<number>|<package id>|<label>|<headsets that have the app>
```

`Quest_App_Labels.txt` (cache):

```text
<package id>|<readable name>
```

## Development Notes

- Each feature lives in its own module file so it can be replaced independently.
- Batch labels are only reachable inside the same file, so a module must not `call :LABEL` for a label defined in another file. Helper routines are defined inside each module that needs them.
- Files should be saved as ANSI or UTF-8 **without BOM**, with CRLF line endings, and should contain English text only.
- End every module path with `exit /b` so control returns to the main menu.

## Known Limitations

- Tested only with a single Meta Quest 3S.
- APK installation has not been fully tested.
- The app list shows package IDs unless readable names are extracted with AAPT.
- Headset status is updated only when the Headset Manager screen is opened or refreshed manually.
- Wired and wireless connections are not yet shown separately.

## License and Credits

This project uses [scrcpy](https://github.com/Genymobile/scrcpy) and Android platform-tools (ADB), which have their own licenses. Please follow their license terms. No license has been chosen for this project's scripts yet; add one before redistributing.
