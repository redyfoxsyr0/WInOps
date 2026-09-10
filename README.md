

# WinOps V4

<p align="center">
  <img width="128" height="128" src="Assets/StoreLogo.png" alt="WinOps Logo"/>
</p>

<p align="center">
  Advanced Windows optimization, repair, cleanup, networking, tweak, VR optimization, and quick-install toolkit built with WinUI 3.
</p>

---

# Overview

**WinOps V4** is an advanced Windows utility suite designed to combine system maintenance, repair tools, optimization tweaks, VR-specific optimizations, installer automation, and Windows utilities into one modern desktop application.

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

WinOps centralizes common Windows commands, repair utilities, networking tweaks, optimization scripts, VR optimizations, and software installation tools into a single graphical interface.

Instead of manually using Command Prompt, Registry Editor, PowerShell, or Windows Settings, users can perform advanced system operations directly through the WinOps interface.

---

# Main Features

## 🧹 Cleanup System

The Cleanup page contains multiple tools for removing unnecessary files and clearing cached Windows data.

### Features

* Clean Temporary Files
* Clean Prefetch Data
* Clean System Logs
* Clean Windows Update Cache
* Empty Recycle Bin

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

# 🔧 System Fix

The System Fix page gives quick access to built-in Windows recovery and repair commands.

### Core Repairs

* **System File Checker (SFC)** — Scans and repairs missing or damaged Windows files.
* **DISM Image Restore** — Repairs component store corruption and servicing stack.
* **Network Stack Reset** — Flushes DNS, resets IP configuration, and restarts services.
* **Fix Defender Updates** — Clears Defender definitions cache and forces a fresh signature update.
* **Windows Update Repair** — Clears SoftwareDistribution cache and resets update services.
* **Repair Microsoft Store** — Resets Store cache and re-registers app packages.

### Advanced Tools

Additional repair utilities including CHKDSK and Explorer Restart are accessible from this section.

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

# ⚡ Tweaks System

One of the largest systems inside WinOps is the Tweaks page.

This section allows users to enable or disable advanced Windows settings and optimization features.

## System Tweaks

* Disable Hibernation
* Disable Xbox Game Bar
* Disable Background Apps
* Disable SMBv1 Protocol
* Disable Telemetry & Data Collection
* Disable Search Indexing
* Disable SysMain / Superfetch

## CPU & Boot Tweaks

* Enable All CPU Cores on Boot
* Dynamic Tick
* Fast Startup
* Disable Windows Update Auto-Restart

## Security & Privacy

* Enable Windows Firewall
* Enable Windows Defender Real-Time Protection
* Enable User Account Control (UAC)
* Disable Activity History
* Disable Advertising ID
* Disable Bing Search in Start Menu
* Disable Cortana
* Disable Delivery Optimization
* Enable Spectre/Meltdown Mitigations
* Enable Core Isolation / Memory Integrity

## System Features

* Enable System Restore
* Enable Hyper-V
* Enable Windows Subsystem for Linux (WSL)
* Enable Optional Features (.NET, Telnet, etc.)
* Show File Extensions
* Show Hidden Files
* Disable Sticky Keys Shortcut
* Enable NumLock on Startup

## Anti-Spyware Protection

* Block Microsoft Telemetry Domains (Hosts File)
* Block Tracking & Advertising Domains
* Block Windows Spy/Telemetry Services
* Disable Diagnostic Tracking Service
* Disable Tailored Experiences
* Disable App Suggestions & Tips
* Disable Location Tracking
* Disable Wi-Fi Sense & Hotspot Sharing
* Disable Camera & Microphone Access
* Disable Speech Recognition
* Disable Unnecessary Notifications
* Disable P2P Update Delivery
* Disable Shared Experiences
* Disable Find My Device
* Disable Windows Error Reporting

---

# 🎮 Performance Tweaks

The Performance section provides quick-action buttons for latency and power optimization.

### Quick Actions

* Apply Latency Optimization
* Power Plan: Balanced
* Power Plan: High Performance
* Power Plan: Ultimate Performance
* Optimize Network
* Flush DNS
* Restart to BIOS

### Hardware & System Toggles

* Disable Hardware-Accelerated GPU Scheduling (HAGS)
* CPU Boost Mode (Aggressive)
* Timer Resolution / Platform Tick

---

# 🥽 VR Tweaks

Optimizations for VR gaming: latency, stutter, compositor, and USB power.

### VR Compositor & Latency

* Disable Fullscreen Optimizations (VR titles)
* Disable Dynamic Tick (VR latency)
* Force Platform Tick (1ms timer for VR)
* Disable HAGS (fixes VR stutter on some GPUs)
* Disable Windows Game Mode (VR compositor conflicts)
* VR Focus-Loss Timer Fix (keeps 1ms timer when sim loses focus)

### GPU & Driver

* Set GPU Priority to High in Registry
* Steer GPU Interrupts Off CPU 0 (micro-stutter fix)
* Set NVIDIA Power Mode: Prefer Maximum Performance
* Set VR Pre-Rendered Frames to 1 (lowest latency)

### USB & Headset Power

* Disable USB Selective Suspend (stops headset/wheel power-cycling)
* Disable 'Allow computer to turn off this device' for HID/USB

### Process & Scheduler

* Enable MMCCS 'Games' Task Priority
* Set System Responsiveness to 10 for VR
* Set Win32 Priority Separation to 0x26 (VR-friendly)
* Power Plan: High Performance (VR compositor friendly)

### Background Interference

* Disable Xbox Game Bar / DVR (VR overlay conflicts)
* Disable Game DVR Background Recording
* Quiet Windows Update / Search Scans During VR Sessions
* Add Common VR Game Folders to Defender Exclusions

### Headset-Specific Tweaks

* Restart VR Runtime (SteamVR / Oculus / WMR)
* Open SteamVR Settings
* Open Headset Dashboard (SteamVR)

### VR Process Priority (Persistent High)

* Set All VR Processes to High Priority
* Restore VR Process Priorities to Normal
* Persist High Priority for VR Executables (Registry IFEO)
* Use Above Normal (safer than High for VR compositors)

### Quick VR Actions

* Clear VR Shader Caches
* Flush DNS (multiplayer VR)
* Restart to BIOS (VR BIOS tuning)

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

* Web Browsers
* Messaging
* Media
* .NET
* Developer Tools
* Utilities
* Compression Tools
* Gaming Apps
* Security Tools
* Java Runtime
* VC++ Redistributables

### Included Applications

* Chrome
* Opera GX
* Firefox
* Brave
* Zoom
* Discord
* Teams
* Pidgin
* iTunes
* VLC
* AIM
* foobar2000
* .NET 4.8.1
* .NET Desktop Runtime x64 8
* .NET Desktop Runtime x64 9
* .NET Desktop Runtime arm64 9
* Visual Studio Code
* Python
* Git
* Spotify
* 7-Zip
* WinRAR
* Malwarebytes
* ShareX
* Everything
* Notepad++
* Steam
* Epic Games Launcher

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

### Supported Editions

* Windows 10 & 11 Home
* Windows 10 & 11 Home N
* Windows 10 & 11 Home Single Language
* Windows 10 & 11 Home Country Specific
* Windows 10 & 11 Pro
* Windows 10 & 11 Pro N
* Windows 10 & 11 Education
* Windows 10 & 11 Education N
* Windows 10 & 11 Enterprise
* Windows 10 & 11 Enterprise N
* Windows Server 2025 Standard
* Windows Server 2025 Datacenter
* Windows Server 2025 Datacenter: Azure Edition

### Technologies

Windows registry access and native Windows licensing APIs.

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

The application automatically requests elevation.

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

> **⚠️ NOT A REPLACEMENT FOR ANTIVIRUS**

WinOps is a system utility toolkit, not a security product. It does **not** provide real-time protection, malware scanning, threat detection, or ransomware defense. Always use a dedicated, up-to-date antivirus and anti-malware solution alongside WinOps.

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

---
