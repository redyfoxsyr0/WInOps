# WinOps V4

<p align="center">
  <img width="128" height="128" src="Assets/StoreLogo.png" alt="WinOps Logo"/>
</p>

<p align="center">
  Advanced Windows optimization, repair, cleanup, networking, tweak, and quick-install toolkit built with WinUI 3.
</p>

---

# Overview

**WinOps V4** is an advanced Windows utility suite designed to combine system maintenance, repair tools, optimization tweaks, installer automation, and Windows utilities into one modern desktop application.

The project is built using:

* C#
* .NET
* WinUI 3
* Windows App SDK
* PowerShell
* Native Windows APIs
* Registry operations
* DISM
* winget

WinOps centralizes common Windows commands, repair utilities, networking tweaks, optimization scripts, and software installation tools into a single graphical interface.

Instead of manually using Command Prompt, Registry Editor, PowerShell, or Windows Settings, users can perform advanced system operations directly through the WinOps interface.

---

# Main Features

## 🧹 Cleanup System

The Cleanup page contains multiple tools for removing unnecessary files and clearing cached Windows data.

### Features

* Temporary File Cleanup
* Prefetch Cleanup
* Windows Logs Cleanup
* Windows Update Cache Cleanup
* Recycle Bin Cleanup

### How It Works

WinOps dynamically creates temporary batch scripts and executes native Windows cleanup commands.

Example:

```bat
@echo off
del /s /f /q %temp%\*.*
rd /s /q %temp%
```

The application also uses native Windows shell APIs to empty the Recycle Bin directly.

These operations help:

* Free storage space
* Remove temporary cache files
* Clear leftover Windows update files
* Reduce junk accumulation

---

# 🔧 System Repair Tools

The System Fix page gives quick access to built-in Windows recovery and repair commands.

### Included Tools

* SFC Scan
* DISM RestoreHealth
* Network Reset
* Windows Defender Repair
* Windows Update Repair
* Microsoft Store Reset
* CHKDSK Repair
* Explorer Restart

### Commands Used

```cmd
sfc /scannow
```

```cmd
DISM /Online /Cleanup-Image /RestoreHealth
```

```cmd
netsh winsock reset
```

```cmd
chkdsk /f /r
```

### How It Works

WinOps launches elevated command processes using:

```cs
Process.Start(...)
```

with:

```cs
Verb = "runas"
```

This allows the application to execute repair operations with Administrator privileges.

---

# ⚡ Windows Tweaks System

One of the largest systems inside WinOps is the Tweaks page.

This section allows users to enable or disable advanced Windows settings and optimization features.

## Included Tweaks

### Performance Tweaks

* Dynamic Tick
* CPU Boost
* HAGS
* Latency Optimization
* Max CPU Boot Usage
* Fast Startup

### Privacy Tweaks

* Disable Telemetry
* Disable Background Apps
* Disable Xbox Game Bar
* Disable Search Indexing

### Windows Features

* SMBv1 Toggle
* Hyper-V Toggle
* WSL Toggle
* .NET Features Toggle
* Hibernation Toggle

### Security & System

* Firewall Toggle
* Windows Defender Toggle
* Remote Desktop Toggle
* UAC Toggle
* System Restore Toggle
* Windows Update Restart Policy

---

# 🌐 Network Optimization System

WinOps includes a networking optimization script designed to improve networking behavior, lower latency, and optimize adapter settings.

## Included Network Optimizations

* TCP Tweaks
* DNS Optimization
* MTU Optimization
* Winsock Reset
* Adapter Tweaks
* Network Throttling Removal
* Interrupt Moderation Tweaks
* NetBIOS Tweaks
* DNS Cache Tweaks

### DNS Providers

* Cloudflare
* Google DNS
* Quad9
* OpenDNS

### Commands Used

```cmd
netsh int tcp set global autotuninglevel=experimental
```

```cmd
netsh winsock reset
```

```powershell
Set-DnsClientServerAddress
```

---

# 📦 Quick Install System

The Quick Install page acts as a software installer hub.

Users can select applications and install them automatically using winget or direct download URLs.

## Included Categories

* Browsers
* Developer Tools
* Utilities
* Media Apps
* Compression Tools
* Messaging Apps
* Gaming Apps
* Security Tools
* Java Runtime
* .NET Runtime
* VC++ Redistributables

### Included Applications

* Chrome
* Firefox
* Brave
* Discord
* Steam
* Epic Games Launcher
* Visual Studio Code
* Python
* Git
* VLC
* Spotify
* 7-Zip
* WinRAR
* Malwarebytes
* ShareX
* Everything
* Notepad++

and many more.

### How It Works

WinOps dynamically generates PowerShell installation scripts.

The application can:

* Install apps directly
* Export install scripts
* Bulk install applications
* Open direct download links

---

# 🔑 Windows Key Tools

The application includes Windows licensing utilities.

### Features

* View Windows product key
* Install generic Windows keys
* Attempt Windows activation

### Technologies and Windows registry access.

---

# 🏠 Home Page

The Home page contains:

* Discord link
* GitHub link
* Documentation link
* Support/Donation link

The interface also includes:

* Hover animations
* Scaling effects
* Fluent UI styling
* Interactive cards

---

# 🎨 User Interface

WinOps V4 uses:

* WinUI 3
* Fluent Design
* Animated cards
* Responsive layouts
* Smooth transitions
* Modern Windows styling

---

# 🔒 Administrator Permissions

Many WinOps features require Administrator access because they:

* Modify registry values
* Change Windows networking settings
* Execute DISM commands
* Control Windows services
* Modify boot configuration
* Execute PowerShell scripts

The application automatically requests elevation 

---

# 🧩 Technologies Used

## Languages

* C#
* PowerShell
* Batch

## Frameworks

* .NET
* WinUI 3
* Windows App SDK

## Windows Technologies

* Registry APIs
* Process APIs
* DISM
* winget
* BCDEdit
* PowerCFG
* NetSH
* Windows Services

---

# Requirements

* Windows 10 or Windows 11
* x64 System
* Administrator privileges recommended
* Internet connection required for downloads/installations

---

# Safety Notice

WinOps modifies advanced Windows settings.

Some tweaks can affect:

* Performance
* Security
* Networking
* Windows services
* Windows startup behavior

It is recommended to:

* Create a restore point
* Understand tweaks before applying them
* Use Administrator mode carefully

---

# Disclaimer

This project is provided for educational and utility purposes only.

The developers are not responsible for:

* System instability
* Broken configurations
* Data loss
* Improper tweak usage
* Third-party software behavior

Use at your own risk.

---

# Credits

Created by SyrOnix.

Built using Microsoft WinUI 3 and the Windows App SDK.
