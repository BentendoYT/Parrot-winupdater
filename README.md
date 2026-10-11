> [!NOTE]
> **Forked from [Librewolf WinUpdater](https://librewolf.dev/librewolf/winupdater)**

---

[Getting started](#getting-started) • [Settings](#settings) • [Issues](#issues)

<img src="Parrot-WinUpdater.ico" align="right">

# Parrot WinUpdater

An attempt to make updating Parrot Browser for Windows much easier.

![Parrot WinUpdater](SCREENSHOT.png)

## Getting started

### Parrot Setup

- When installing Parrot Browser, the installer can offer an option to install Parrot WinUpdater. On first run, it will copy itself to `%AppData%\Parrot\WinUpdater\` to be able to update itself without administrator privileges.
- Alternatively, you can download and extract the latest [`Parrot-WinUpdater_x.x.x.zip`](https://github.com/BentendoYT/Parrot-Browser/releases), then run `Parrot-WinUpdater.exe` to check for an update. If one is available, it will be downloaded immediately.

### Parrot Portable

- If you want to run the portable version of Parrot Browser, download and extract the latest portable Parrot Browser package from the official releases page. It should contain WinUpdater if you have integrated it into the package.
- Parrot Browser will be updated automatically whenever you run `Parrot-Portable.exe` (checks for new versions happen once a day). If you only wish to perform update checks manually, just rename WinUpdater to e.g. `Parrot-ManualUpdater.exe` and run it whenever you like.

### Scheduled updates

- When **installing Parrot Browser**, select the option `Schedule Automatic Updates` to automatically set up what's described in the point below.
- You can run Parrot WinUpdater to enable the option `Schedule a task for automatic update checks`. This will prompt for administrator permissions and a blue (PowerShell) window will notify you of the result. The scheduled task will run while the **current user account** is logged in (at 1 minute after login, and every 4 hours).
- If your account has **administrator permissions**, the update will be fully automatic. If not, the update will be downloaded and you will be asked to start the update (administrator permissions required).
- If Parrot Browser is already **running**, you will be notified about the new version. The update will start as soon as you close the browser.

## Settings

### Settings and log file

- The updater needs to be able to write to `Parrot-WinUpdater.ini` in its own folder (so make sure it has permission to do so), otherwise WinUpdater will copy itself to `%AppData%\Parrot\WinUpdater\` and run from there.
- `Parrot-WinUpdater.ini` contains a `[Log]` section that shows the results of the last update check and the last update action.

### Self-updating

Parrot WinUpdater also updates itself automatically, so you won't have to check for new releases on this page. However, if you prefer to update it manually, you can set `UpdateSelf` to `0` in the `.ini` file under `[Settings]`:

```ini
[Settings]
UpdateSelf=0
```

### Changing the working directory

If for some reason WinUpdater is not able to use the user's default `%Temp%` folder for downloading and extracting files, you can specify an alternative work directory by setting `WorkDir` in the `.ini` file under `[Settings]`:

```ini
[Settings]
WorkDir=D:\Temp
```

To specify the directory of `Parrot-WinUpdater.exe`, use `WorkDir=.`, or use a relative subfolder like `WorkDir=.\Temp`.

## Issues

### Anti-cheat software

If you set up scheduled updates, you might get annoyed by some anti-cheat software. It may wrongfully point at WinUpdater, because it is built upon AutoHotkey, which **can** be used to cheat in games. If this happens, you can either:

1. Open the **Task Scheduler** via the Start menu, then double-click on `Parrot WinUpdater...` in the `Task Scheduler Library`, open the `Triggers` tab, then click on `One time` and the button `Delete`. Then press `OK` to make the change to **only check for updates 1 minute after login** (and not every 4 hours). This will still cause issues if you leave games opened when locking your user account, though.
2. Create a shortcut to `%AppData%\Parrot\WinUpdater\Parrot-WinUpdater.exe /RemoveTask` and one to `%AppData%\Parrot\WinUpdater\Parrot-WinUpdater.exe /CreateTask` on your desktop, so you can quickly prevent WinUpdater from running during gameplay and reactivate it afterwards. Just put them next to the shortcuts of your game launchers and you won't forget.

### Signature verification

WinUpdater will validate itself and the setup file (or `parrot.exe` when downloading the portable version) via the signing certificate. For this, PowerShell is used in the background. You may get the warning `Could not verify that (...) was correctly signed` or the error `Signature verification has failed` if you have blocked internet access for PowerShell (e.g. `%WinDir%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`). For signature verification, PowerShell needs to be able to update certificate revocation lists. If this is not possible on your system, you can permanently disable signature checks and rely on the file hash checks exclusively by setting `NoSigChecks` to `1` in the `.ini` file under `[Settings]`:

```ini
[Settings]
NoSigChecks=1
```

### Server certificate revocation

You may encounter a `Security Alert: Revocation information for the Security certificate for this site is not available` Windows dialog or the error `The server certificate revocation check for (...) has failed` warning by WinUpdater. This can happen if you have enabled the non-default option `Check for server certificate revocation` in Windows `Internet Options` (tab `Advanced`). You can tell WinUpdater to automatically continue when this dialog pops up by setting `IgnoreCrlErrors` to `1` in the `.ini` file under `[Settings]`:

```ini
[Settings]
IgnoreCrlErrors=1
```

### Asking for help

If you experience another issue, you can search the problems (and solutions) reported in the [Parrot WinUpdater issues](https://github.com/BentendoYT/Parrot-Browser/issues) and the [Parrot Browser issues](https://github.com/BentendoYT/Parrot-Browser/issues), or create an issue yourself.
