<p align="center">
  <img src="https://github.com/wizdumb23/ha-3dprint-addon/blob/main/logo.png?raw=true" alt="ha-3dprint-addon Logo" width="300">
</p>

# 🖨️ ha-3dprint-addon — 3D Printing Add-ons for Home Assistant

<p align="center">
  A Home Assistant add-on repository bringing together the tools you need to run a self-hosted 3D-printing setup:
  <a href="https://github.com/maziggy/bambuddy">BamBuddy</a> for printer management and
  <a href="https://github.com/Donkie/Spoolman">Spoolman</a> for filament inventory tracking — both packaged as native HA add-ons.
</p>
<p align="center">
  <strong>Your printers. Your filament. No cloud. Your rules.</strong><br>
  Self-hosted command center for Bambu Lab &mdash; from one A1 to a 40-printer farm.
</p>
<p align="center">
  <img src="https://img.shields.io/badge/dynamic/yaml?url=https://raw.githubusercontent.com/wizdumb23/ha-3dprint-addon/main/bambuddy/config.yaml&query=$.version&label=bambuddy%20stable&color=blue">
  <img src="https://img.shields.io/badge/dynamic/yaml?url=https://raw.githubusercontent.com/wizdumb23/ha-3dprint-addon/main/bambuddy-beta/config.yaml&query=$.version&label=bambuddy%20beta&color=orange">
  <img src="https://img.shields.io/badge/dynamic/yaml?url=https://raw.githubusercontent.com/wizdumb23/ha-3dprint-addon/main/bambuddy-daily/config.yaml&query=$.version&label=bambuddy%20daily&color=purple">
  <img src="https://img.shields.io/badge/dynamic/yaml?url=https://raw.githubusercontent.com/wizdumb23/ha-3dprint-addon/main/spoolman/config.yaml&query=$.version&label=spoolman&color=teal">
</p>
<p align="center">
  <img src="https://img.shields.io/badge/aarch64-yes-green.svg">
  <img src="https://img.shields.io/badge/amd64-yes-green.svg">
  <img src="https://img.shields.io/badge/armv7-yes-green.svg">
</p>

---

## 📦 What's in this repository

| Add-on | What it does |
|---|---|
| **BamBuddy** | ✅ Stable release of the [BamBuddy](https://github.com/maziggy/bambuddy) printer command center — recommended for most users |
| **BamBuddy (Beta)** | 🧪 Beta release — newer features, may contain bugs |
| **BamBuddy (Daily)** | 🔬 Daily build — cutting-edge, least stable, kept in sync automatically (see below) |
| **Spoolman** | 🧵 [Spoolman](https://github.com/Donkie/Spoolman) — track your filament inventory: spools, materials, vendors, and usage |

---

## 📋 Requirements

- Home Assistant OS or Supervised installation (Supervisor required)
- Supported architecture: amd64, aarch64, or armv7 (Spoolman only — see its own listing for BamBuddy's supported architectures)

---

## 🚀 Installation

### Via button (recommended)

Click the button below to automatically add the repository to Home Assistant:

[![Add Repository to Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https://github.com/wizdumb23/ha-3dprint-addon)

Or add it manually:

1. In Home Assistant, go to **Settings → Add-ons → Add-on Store**
2. Click the three-dot menu → **Repositories**
3. Add `https://github.com/wizdumb23/ha-3dprint-addon`

Once the repository is added, install whichever add-ons you need from the list above and follow each one's configuration steps.

---

## 🔄 Automatic Updates

All four add-ons report their version to Home Assistant, so updates show up in the **Add-on Store** like any other add-on — one click to install.

**BamBuddy (Daily) gets extra care**: a scheduled GitHub Actions workflow checks [maziggy/bambuddy](https://github.com/maziggy/bambuddy) for new daily tags every day and automatically bumps `bambuddy-daily/config.yaml` when a newer build is published — no manual version-tracking required to stay current with upstream's daily releases. The other add-ons are updated by hand as new stable/beta releases land.

---

## 💾 Data Persistence

All add-on data (print archives, filament inventory, settings, logs) is stored persistently under `addon_configs/<slug>/`, accessible via the **File Editor** in Home Assistant. Your data survives updates and restarts. When uninstalling, Home Assistant will ask whether to remove the add-on data as well — keep it, and your data will still be there after a reinstall.

---

## ⚠️ Known Limitations (BamBuddy)

### HA Ingress — Not Supported

HA Ingress is currently **not supported** and is not planned for BamBuddy. Its SPA architecture relies on a stable origin for API calls, routing, PWA scope, and service workers — all incompatible with HA Ingress's rotating per-session subpaths. This would require extensive rewrites to BamBuddy core. (Spoolman, by contrast, runs fully through Ingress.)

### Virtual Printer — Potential Port Conflicts

When using BamBuddy's Virtual Printer feature, several ports are bound directly on the Home Assistant host. This may conflict with other installed add-ons or services (most notably the **Mosquitto MQTT Broker** on port 8883).

For a full list of affected ports and details, see the **Documentation** tab of the BamBuddy add-on.

---

## ℹ️ Disclaimer

This repository is **not** an official release by the BamBuddy or Spoolman developers. It simply packages [BamBuddy](https://github.com/maziggy/bambuddy) and [Spoolman](https://github.com/Donkie/Spoolman) as native Home Assistant add-ons for easy installation and updates.

I am not affiliated with either upstream project, so I'm unable to provide support for BamBuddy- or Spoolman-specific issues, bugs, or feature requests. For anything related to the underlying apps, please refer to the original projects:

👉 **[github.com/maziggy/bambuddy](https://github.com/maziggy/bambuddy)**
👉 **[github.com/Donkie/Spoolman](https://github.com/Donkie/Spoolman)**

Support provided here is limited to the **add-on packaging and installation** — please [open an issue](https://github.com/wizdumb23/ha-3dprint-addon/issues) on this repo for packaging-related problems.
