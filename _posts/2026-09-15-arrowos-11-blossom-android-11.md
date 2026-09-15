---
title: ArrowOS 11.0 (Android 11) for Xiaomi Redmi 9A / 9C / 10A (blossom)
date: 2026-09-15 23:10:00 +0700
categories: [Android]
tags: [xiaomi, redmi9a, redmi9c, redmi10a, blossom, arrowos, android11, custom-rom]
author: khaido
comments: true
---

> **Device:** Xiaomi Redmi 9A / 9C / 10A / POCO C3 (`blossom` / `dandelion`)  
> **Specs:** 6.53" (720x1600p) | MediaTek Helio G25 | 2GB RAM + 32GB Storage | 5000mAh Battery  
> **ROM:** ArrowOS 11.0 (Android 11 - 64 Bit) | **Build:** Vanilla / Signed  
> **Purpose:** With entry-level hardware (MediaTek Helio G25 and 2GB RAM), running heavy stock MIUI 12.5 makes the phone very slow and laggy. Flashing a lightweight **Custom ROM (ArrowOS 11.0 Vanilla)** removes unnecessary background apps to restore fast, smooth performance for daily use.

---

## Highlights & Notes

- **Initial OSS Vendor Build**
- **Play Integrity Certified by Default** (Passes SafetyNet/Play Integrity out of the box)
- **May Security Patch Included**
- **Vanilla Build:** Super lightweight and fast for 2GB RAM devices.

---

## Changelogs

- Initial OSS vendor build
- Fixed APN issues
- Fixed VoWiFi
- Fixed Faceunlock
- Includes all fixes from A13 OSS builds

---

## Downloads & Links

- **ROM Package:** [Arrow-v11.0-blossom-20240616-VANILLA-signed.zip](https://onedrive-vercel-index-kohl-eight-30.vercel.app/api/raw/?path=/Arrow-v11.0-blossom-20240616-VANILLA-signed.zip)
- **MIUI 12.5 Firmware (Required):** [MIUI 12.5 Firmware Package](https://t.me/garden_mirror/135)
- **Recovery:** [OrangeFox R11.1 Stable (blossom)](https://t.me/Blossom_Roms/818)
- **GApps (Optional):** [NikGapps for Android 11 (ARM64)](https://t.me/garden_mirror/141)
- **Support Group:** [Telegram Blossom Support](https://t.me/Blossom_Support/321959)

---

## Step-by-Step Flashing Instructions

1. Boot into Custom Recovery (OrangeFox R11.1 recommended).
2. Go to **Wipe** -> Select `Dalvik / ART Cache`, `Cache`, and `Data`.
3. Flash the **MIUI 12.5 Firmware** zip file.
4. Flash the **ArrowOS 11.0 ROM** zip file:
   ```bash
   adb -d sideload Arrow-v11.0-blossom-20240616-VANILLA-signed.zip
   ```
5. *(Optional GApps)* Flash NikGapps ARM64 11.0 zip file:
   ```bash
   adb -d sideload NikGapps-arm64-11.0.zip
   ```
6. Go to **Manage Partitions** -> Select `Data` -> **Format Data** (type `yes`).
7. Select **Reboot to System**.


