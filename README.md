# WinOps V4

<p align="center">
  Advanced Windows optimization, repair, cleanup, networking, tweak, VR optimization, and quick-install toolkit built with WinUI 3.
</p>

---

# Overview

**WinOps V4** is an advanced Windows utility suite that combines system maintenance, repair tools, optimization tweaks, VR-specific optimizations, installer automation, and Windows utilities into one modern desktop application.

The project is built using:

* C# (.NET 8)
* WinUI 3 / Windows App SDK (unpackaged, x64)
* Native Windows APIs (P/Invoke: powrprof, dnsapi, firewall COM)
* WMI (`Win32_Service`, `SoftwareLicensingService`, Defender provider, `MSFT_NetAdapter*`)
* Registry / ServiceController / EventLog / System.Text.Json
* DISM, SFC, winget, bcdedit, netsh (invoked directly — no temp scripts)

WinOps centralizes common Windows commands, repair utilities, networking tweaks, optimization settings, VR optimizations, and software installation into a single graphical interface — instead of manually using Command Prompt, Registry Editor, PowerShell, or Windows Settings.

The codebase is split into real DLLs: a thin WinUI shell plus one class library per feature (`WinOps.Core`, `WinOps.Feature.Tweaks`, `WinOps.Feature.VrTweaks`, `WinOps.Feature.Cleanup`, `WinOps.Feature.SystemFix`, `WinOps.Feature.Activation`, `WinOps.Feature.QuickInstall`), discoverable at runtime through a small plugin contract. Wherever a .NET or native API exists, WinOps uses it — PowerShell/cmd survive only for tools with no API (DISM, SFC,Exe-based repairs, AppX which needs package identity) and for user-saved script artifacts.

---

# Main Features

## 🧹 Cleanup System

Seven cleanup tasks with per-task status plus a **Clean All** run that reports exactly which tasks failed and why.

* Clean Temporary Files
* Clean Prefetch Data
* Clean Thumbnail Cache
* Empty Recycle Bin (already-empty reports success, not `0x8000FFFF`)
* Clean System Logs
* Clean Windows Update Cache
* Clean Delivery Optimization Cache

Cleanup is done with direct .NET file IO, `ServiceController` for service stops/starts, and the native `SHEmptyRecycleBin` shell API. These operations free storage, remove stale caches, and clear leftover update payloads.

---

# 🔧 System Fix

Quick access to built-in Windows recovery and repair commands, all launched elevated.

### Core Repairs

* **System File Checker (SFC)** — scans and repairs missing or damaged Windows files
* **DISM Image Restore** — repairs component store corruption
* **Network Stack Reset** — winsock reset + native DNS flush + IP renew
* **Fix Defender Updates** — clears definitions cache, forces fresh signatures
* **Windows Update Repair** — stops services (via API), clears `SoftwareDistribution`/CatRoot, restarts services, runs DISM + SFC
* **Repair Microsoft Store** — `wsreset` cache reset
* **Drive Check (CHKDSK)** and **Restart Windows Explorer**

### Device & Shell Fixes

* Restart Audio services / Bluetooth service / Print Spooler (+ clear queue)
* Reset Firewall to defaults (via firewall COM API, netsh fallback)
* Rebuild Icon Cache, Resync System Time, Rebuild Search Index
* Analyze Component Store, Re-register built-in apps

### Defender Whitelist (False Positives)

Because tweakers get flagged, WinOps can whitelist itself and anything else:

* **Automatic on every launch** — the app adds its own folder + exe to Defender exclusions at startup (verified by re-reading the exclusion list; refuses silently never — if Tamper Protection blocks it, you get a dialog with the exact fix)
* Manual controls for folder / file / process exclusions, removal, and listing current exclusions

### Required Runtimes

* **Automatic on launch** — detects missing .NET 8+ Desktop Runtime / VC++ redist and offers to download + install the official installers
* Manual Check / Install buttons in the same section

---

# ⚡ Tweaks System

The largest system in WinOps. Every toggle reads live state and applies through registry, service, power, or WMI APIs.

## System Tweaks

* Disable Hibernation, Xbox Game Bar, Background Apps, SMBv1, Telemetry, Search Indexing, SysMain
* Disable Activity History, Advertising ID, Bing Search, Cortana, Delivery Optimization
* Show File Extensions / Hidden Files, This PC desktop icon, Explorer recent files
* Disable Sticky Keys shortcut, NumLock on startup

## Taskbar, Start & Personalization

* Widgets button, Taskbar alignment / Task View / Search box, End-Task on right-click
* Dark Mode, Transparency, Window Animations, Search Highlights
* Suggested Apps, Lock Screen Spotlight, Windows Copilot

## Performance & CPU/Boot

* Latency Optimization, Balanced / High / Ultimate power plans (resolved by name, so custom builds like Atlas work), Timer Resolution / Platform Tick
* HAGS, CPU Boost Mode, Visual Effects, Memory Compression
* All CPU cores on boot (`numproc`), Dynamic Tick, Fast Startup
* Optimize Network (registry + WMI DNS/NetBIOS + netsh, no script files), Flush DNS, Restart to BIOS

## Security & Privacy

* Firewall, Defender real-time protection (registry first, cmdlet fallback, Tamper note only when both fail), RDP, UAC
* Spectre/Meltdown mitigations, Core Isolation, Remote Assistance
* Power Throttling off, AutoPlay, Verbose Boot
* Disable Windows Update auto-restart, System Restore (WMI), Hyper-V, WSL, NetFx3

## Anti-Spyware

* Block telemetry / tracking / spy domains via hosts file
* Disable DiagTrack, Tailored Experiences, App Suggestions, Location Tracking, Wi-Fi Sense, Camera/Mic access, Speech Recognition, Notifications, P2P Updates, Shared Experiences, Find My Device, Error Reporting

## Batch Actions

* Apply All Recommended, Apply All Privacy, Restore Defaults
* One-click SFC scan, DISM repair, Update-cache clear, Temp clear, DNS flush

---

# 🥽 VR Tweaks

Latency, stutter, compositor, USB power, and GPU optimizations for VR gaming — with live GPU vendor detection.

### VR Compositor & Latency

* Disable Fullscreen Optimizations, Dynamic Tick, Game Mode; Force Platform Tick (1 ms)
* VR Focus-Loss Timer Fix, GPU Priority High, GPU Interrupt Steering
* SteamVR settings-file editor (motion smoothing, supersample scale, supersample filtering, reprojection mode — JSON-preserving with backup)
* OpenXR runtime switcher (SteamVR / Oculus / WMR) with live status

### GPU & Driver (NVIDIA + AMD)

* NVIDIA: persistence mode via `nvidia-smi`
* AMD: disable ULPS / Deep Sleep / Frame Rate Target, clear AMD shader cache
* HAGS off (fixes stutter on some GPUs), VR pre-rendered frames guidance

### USB, Power & Scheduler

* Disable USB Selective Suspend + HID power management (stops headset power-cycling)
* Disable PCIe link-state saving, core parking off, High-Performance plan
* MMCS Games priority, System Responsiveness, Win32 Priority Separation
* Do-Not-Disturb (toasts) for sessions, quiet Update/Search scans

### Headset & Process Tools

* Restart VR runtime, open SteamVR settings / headset dashboard
* VR process priority: High or Above-Normal, persistent via IFEO, one-click restore
* Clear VR shader caches, reset SteamVR room setup (backed up), flush DNS, restart to BIOS
* VR game folders → Defender exclusions

---

# 🌐 Network Optimization

A ~60-step optimization run: TCP stack, DNS, MTU, Winsock, adapter offloads, throttling removal, NetBIOS, DNS cache.

* DNS set to Cloudflare (`1.1.1.1` / `1.0.0.1`) + Google (`8.8.8.8` / `8.8.4.4`), IPv4 + IPv6
* TCP auto-tuning `normal`, CTCP congestion provider, task offload / RSC / chimney tuned
* Adapter advanced properties (Flow Control, Interrupt Moderation, checksum/RSS) via WMI, best-effort per adapter

---

# 📦 Quick Install System

Ninite-style software hub: pick apps across icon-coded category cards and install them all at once.

* **Direct install** — one elevated `winget install` per app (no wrapper scripts); manual-URL apps open their download pages
* **Export Script** — saves the selection as a `.ps1` you can reuse on other machines
* Search filter, Select Popular (★-marked apps), live selection counts, per-row winget/URL badges

### Categories

Web Browsers, Messaging, Media, .NET, Java, Imaging, Documents, Security, File Sharing, Online Storage, Other, Utilities, Compression, VC++ Redistributables, Developer Tools.

### Included (selection)

Chrome, Firefox, Brave, Discord, Signal, Telegram, VLC, OBS Studio, HandBrake, Spotify, PowerToys, Windows Terminal, VS Code, Git, Python, Notepad++, 7-Zip, Bitwarden, Malwarebytes, ShareX, Everything, Obsidian, LocalSend, RustDesk, qBittorrent, Steam, Epic Games Launcher, LibreOffice, .NET / Java runtimes, VC++ redists — and more.

---

# 🔑 Windows Key Tools

* Look up the installed product key + license status (WMI, no scripts)
* Install generic (GVLK) keys and activate via KMS with 3-host fallback
* Optional logon Scheduled Task persistence (no batch files)

### Supported Editions

Windows 10 & 11 Home / Home N / Home Single Language / Home Country Specific / Pro / Pro N / Education / Education N / Enterprise / Enterprise N; Windows Server 2016 / 2019 / 2022 / 2025 (Standard, Datacenter, Essentials where applicable, incl. Azure Edition).

---

# 🏠 Home Page

Discord, GitHub, Documentation, and Support cards with hover animations and Fluent styling.

---

# 🧩 Architecture

```
WinOps_V4_Unpackaged/
├── WinOps_V4.csproj          # WinUI 3 shell — navigation + thin pages only
├── App.xaml(.cs)             # launch
├── MainWindow.xaml(.cs)      # nav shell, startup whitelist + runtime checks
├── Pages/                    # one thin UI page per feature (no business logic)
└── Libs/
    ├── WinOps.Core.dll               # ProcessRunner, RegistryHelper, ElevationHelper,
    │                                 # ServiceHelper, RuntimePrereq, Wmi, Plugin loader,
    │                                 # Native: Shell32, Power (powrprof), DnsApi, FirewallPolicy
    ├── WinOps.Feature.Tweaks.dll     # TweakService (system/performance/privacy tweaks)
    ├── WinOps.Feature.VrTweaks.dll   # VrTweakService (VR compositor/GPU/USB/OpenXR/AMD)
    ├── WinOps.Feature.Cleanup.dll    # CleanupService (7 tasks)
    ├── WinOps.Feature.SystemFix.dll  # SystemFixService (repairs, whitelist, runtimes)
    ├── WinOps.Feature.Activation.dll # ActivationService (WMI licensing)
    └── WinOps.Feature.QuickInstall.dll # CatalogBuilder, WingetService, app models
```

Each `WinOps.Feature.*` library exposes an `IFeaturePlugin` and is resolved through `PluginLoader`, so features stay decoupled from the UI. Service methods return `OperationResult` and never throw across the UI boundary.

### How elevation and execution work

The app manifest forces Administrator at launch (`requireAdministrator`), so child tools inherit elevation — no per-action UAC prompts. Long or interactive repairs (`sfc`, `DISM`, `chkdsk`) open their own console via `Process.Start(..., Verb = "runas")`; everything else runs in-process through APIs.

---

# Requirements

* Windows 10 (1809+) or Windows 11, **x64 only**
* Administrator launch is **forced** by the app manifest (most features need it)
* .NET 8 Desktop Runtime x64 **or newer** (the build rolls forward to .NET 9/10; if nothing suitable is installed, WinOps offers to install it on launch)
* Visual C++ 2015–2022 Redistributable x64 (also auto-offered if missing)
* Internet connection for downloads, winget installs, and runtime fetching
* The Windows App SDK framework is self-contained — never needs a separate install

### Build from source

Prerequisites: .NET 8 SDK (9 works too), Visual Studio 2022 17.8+ with WinUI / Windows App SDK workload.

```powershell
dotnet build WinOps_V4.csproj -c Release -a x64
```

Output lands in `bin/Release/net8.0-windows10.0.19041.0/win-x64/` — ship the whole folder (exe + `WinOps.*.dll` + `Assets`).

---

# 🔒 Administrator Permissions

The manifest requires elevation because WinOps:

* Writes HKLM registry values
* Controls Windows services and power schemes
* Edits the BCD store, hosts file, and network stack
* Runs DISM/SFC/CHKDSK and manages Defender exclusions

---

# Safety Notice

> **⚠️ NOT A REPLACEMENT FOR ANTIVIRUS**

WinOps is a system utility toolkit, not a security product. It does **not** provide real-time protection, malware scanning, threat detection, or ransomware defense. Always run a dedicated, up-to-date antivirus alongside WinOps. (Its Defender *toggles* reduce protection — the *whitelist* feature exists precisely so you don't have to disable anything.)

**WinOps was made to unfuck the fuck Windows is limiting your system with** — removing artificial performance caps, unnecessary background services, telemetry overhead, and restrictive defaults that hold your hardware back.

Some tweaks affect performance, security, networking, services, and boot behavior. Before heavy use:

* Create a System Restore point (or use the in-app System Restore toggle + batch Restore Defaults)
* Understand a tweak before applying it
* Keep Tamper Protection in mind: it will block Defender-related changes by design

---

# Disclaimer

Educational and utility purposes only. The developers are not responsible for system instability, broken configurations, data loss, improper tweak usage, or third-party software behavior. Use at your own risk.

---

# Credits

Created by SyrOnix. Built with WinUI 3 and the Windows App SDK.
