# UnattendedWinstall — laptop profile

A company-laptop profile based on [UnattendedWinstall](https://github.com/memstechtips/UnattendedWinstall) and [Winhance](https://github.com/memstechtips/Winhance). Upstream attribution and terms: [LICENSE.md](./LICENSE.md).

**Purpose:** remove selected consumer bloat during a clean Windows installation. Install all work applications and developer tools manually afterward. No application downloads, developer provisioning, .NET Framework 3.5 installation or Winhance installer shortcut are included.

## What stays

The answer file does not remove:

- OneDrive, Windows Camera and new Outlook.
- Edge, WebView2 and their update infrastructure.
- Microsoft Store, App Installer/WinGet and Windows Terminal.
- Paint, Photos, Notepad, Calculator, Snipping Tool and Sticky Notes.
- Shared Windows app frameworks and dependencies.

This preserves components supplied by the Windows image; it does not install missing apps. Company Teams is **not** retained: the preinstalled `MSTeams` package is removed, and the current client can be installed manually later.

## What is removed

The exact package and capability lists are embedded in [`autounattend.xml`](./autounattend.xml). They include:

- Xbox apps, Game Bar and Solitaire.
- Consumer Copilot, Cortana, News, Weather, Bing Search, Clipchamp, 3D Viewer and Mixed Reality Portal.
- Preinstalled Teams, Skype, legacy Mail/Calendar, Office Hub and OneNote.
- Quick Assist (app and legacy capability), Power Automate Desktop, Dev Home, Feedback Hub, Get Help and Tips.
- Maps, People, consumer Family, Phone Link, To Do, Alarms, Sound Recorder and legacy media apps.
- PowerShell ISE, Steps Recorder and WordPad capabilities where present.

Recall is disabled if its optional feature is present and enabled. Game DVR recording is disabled to match the Game Bar removal.

Missing apps/features are skipped. Individual removal failures are logged to `C:\ProgramData\Winhance\Unattend\Logs\Winhancements.txt`.

## What is configured

- Suppresses advertising IDs, promotional suggestions and automatic suggested-app delivery.
- Hides/disables news widgets, Copilot's taskbar button and Bing results in Start search. Local Windows Search is not changed.
- Shows file extensions and hidden files and enables long-path support. Explorer's opening location is left at the Windows default.
- Seeds per-user preferences through the default user profile before regular users are created.
- Leaves account and network setup screens available to the administrator. No offline-account bypass or network-adapter changes are applied; available setup options depend on the Windows build and edition.

Cleanup runs **once during Windows setup**. There are no recurring removal tasks, per-user SYSTEM tasks, forced customization reboots or browser-protocol redirects. Apps installed manually afterward will not be removed by this profile.

## What is deliberately left alone

Windows/image defaults and company policy remain responsible for:

- UAC and secure desktop, workstation locking, BitLocker/device encryption, TPM and Secure Boot requirements.
- Work-account registration and device management.
- Windows, Microsoft product, Store app and driver updates, including restart notifications.
- Sleep, hibernation, OEM/Balanced power plans and battery-saving behavior.
- Services, automatic maintenance, crash reporting and compatibility/diagnostic tasks.
- Work-app notifications, camera/microphone/location permissions, accessibility, image associations and OneDrive sync/backup settings.

This answer file does not configure company encryption/recovery-key backup, endpoint protection or update management. Set those up separately. No hardware-requirement bypasses are included.

No changes here intentionally disable antivirus, firewall, security services or third-party endpoint agents. WatchGuard EPDR compatibility still requires the administrator to verify the agent's supported Windows version/architecture, installation, management-console connectivity and protection status on a pilot laptop. Do not disable Defender or add antivirus exclusions through this answer file; let the supported WatchGuard installation and company policy manage coexistence.

## Installation

Use this answer file for a clean Windows 11 installation on supported hardware.

1. Back up required data and keep the BitLocker recovery key available. Download any network/storage drivers needed by the laptop.
2. Create a bootable installation USB from an official Microsoft Windows ISO matching the laptop's architecture.
3. Copy [`autounattend.xml`](./autounattend.xml) to the USB's root with that exact filename. Do not enable additional answer-file customizations in the USB creation tool that could overwrite it.
4. Boot the laptop from the USB and perform a clean installation. Select the Windows edition and target disk when prompted. **Deleting partitions erases their data.** The answer file does not automatically wipe disks.
5. Have the administrator complete account/network setup and company enrollment. The answer file does not create accounts or force offline setup.
6. Install applications, developer tools and any required drivers manually. Configure encryption, endpoint protection and update management separately.

Setup components are included for x64, ARM64 and x86; the Windows image must support the target hardware. This configuration is intended for clean installations, not in-place upgrades or existing Windows installations.

Do not run upstream Winhance optimizations afterward expecting them to match this profile; they can reintroduce the settings deliberately removed here.
