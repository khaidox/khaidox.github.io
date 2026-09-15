---
title: How to Install LineageOS on Xiaomi Mi 9 Lite (pyxis)
date: 2026-09-15 21:50:00 +0700
categories: [Android]
tags: [xiaomi, mi9lite, pyxis, lineageos, custom-rom, android15]
author: khaido
comments: true
---

> **Device:** Xiaomi Mi 9 Lite (`pyxis` - `M1904F3BG`) | **Target OS:** LineageOS 22.2 (Android 15)

---

## Prerequisites & Key Combos

- **Stock ROM Requirement:** Must be on official Android 11 stock ROM before flashing LineageOS 22.2.
- **Fastboot Mode:** Power off -> Hold `Vol Down` + `Power`.
- **Recovery Mode:** Power off -> Hold `Vol Up` + `Power`.
- **Backup:** Unlocking bootloader and formatting data will erase **all internal storage**.

---

## Step 1: Unlock Bootloader

1. Enable **Developer Options** (`Settings` > `About Phone` > tap `MIUI Version` 7 times).
2. Link Mi Account: `Developer options` > `Mi Unlock status` > **Add account and device** (via SIM mobile data).
3. Boot into **Fastboot Mode** (`Vol Down` + `Power`).
4. Download and run [MiForge / MiUnlockTool](https://github.com/MiForge/MiUnlockTool) on PC to unlock the bootloader.

---

## Step 2: Flash Lineage Recovery

1. Download `recovery.img` from [LineageOS Downloads](https://download.lineageos.org/devices/pyxis).
2. Reboot device to Fastboot mode:
   ```bash
   adb -d reboot bootloader
   ```
3. Flash recovery:
   ```bash
   fastboot flash recovery recovery.img
   ```
   *(Optional: Alternatively boot recovery temporarily without flashing: `fastboot boot recovery.img`)*
4. **Boot immediately to Recovery** (`Vol Up` + `Power`).

---

## Step 3: Flash LineageOS & GApps

1. In Lineage Recovery, select **Factory Reset** -> **Format data / factory reset**.
2. Go back to main menu, select **Apply update** -> **Apply from ADB**.
3. Sideload LineageOS:
   ```bash
   adb -d sideload lineage-22.2-20260915-nightly-pyxis-signed.zip
   ```
   *(Note: ADB stopping at `47%` with `Success` or `No error` is normal).*
4. **(Optional GApps):** Download a GApps package (`ARM64`, `Android 15`):
   - **Recommended Lightweight:** [NikGapps Core](https://nikgapps.com) (`NikGapps-core-arm64-15-*.zip` ~126.2 MB - includes only Play Store & Play Services)
   - **Other options:** [BiTGApps](https://bitgapps.github.io) or official [MindTheGapps](https://wiki.lineageos.org/gapps/)
   
   Select **Apply from ADB** again and sideload your GApps package (e.g. NikGapps Core):
   ```bash
   adb -d sideload NikGapps-core-arm64-15-20260204-signed.zip
   ```
   Confirm `Yes` if prompted with signature verification warning.
5. Select **Reboot system now**.

---

## References

- [LineageOS Wiki for pyxis](https://wiki.lineageos.org/devices/pyxis/install/variant2)
- [NikGApps Official Site](https://nikgapps.com)
- [BiTGApps Official Site](https://bitgapps.github.io)
- [MindTheGapps / LineageOS GApps Guide](https://wiki.lineageos.org/gapps/)
- [MiForge / MiUnlockTool GitHub](https://github.com/MiForge/MiUnlockTool)
