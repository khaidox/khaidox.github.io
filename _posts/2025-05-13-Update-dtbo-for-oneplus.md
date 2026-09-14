---
created: 2025-05-13
title: Update dtbo.img for Oneplus 13 CN
layout: post
tags: [android,oneplus13]
category: [OnePlus 13]
author: khaido
comments: true
---
```
********************************
**** FLASH AT YOUR OWN RISK ****
********************************
```

This is short article to flash the latest dtbo partition for oneplus CN when using OXYGEN ROOM

# 1. Download OXYGEN ROOM for Oneplus 13
```
# aria2c -j18 https://gauss-componentotacostmanual-in.allawnofs.com/remove-aa2ec8d3d797a62d5becaf0058df2575/component-ota/25/04/28/8747a387d2024e9a924dc8624126d4a2.zip

05/09 16:43:14 [NOTICE] Downloading 1 item(s)
 *** Download Progress Summary as of Fri May  9 16:44:15 2025 *** 
=
[#349ab2 3.5GiB/7.3GiB(47%) CN:1 DL:59MiB ETA:1m7s]
FILE: /content/8747a387d2024e9a924dc8624126d4a2.zip
-

 *** Download Progress Summary as of Fri May  9 16:45:15 2025 *** 
=
[#349ab2 7.0GiB/7.3GiB(94%) CN:1 DL:58MiB ETA:6s]
FILE: /content/8747a387d2024e9a924dc8624126d4a2.zip
-


05/09 16:45:22 [NOTICE] Download complete: /content/8747a387d2024e9a924dc8624126d4a2.zip

Download Results:
gid   |stat|avg speed  |path/URI
======+====+===========+=======================================================
349ab2|OK  |    59MiB/s|/content/8747a387d2024e9a924dc8624126d4a2.zip

Status Legend:
(OK):download completed.
```

# 2. Extract dtbo partition from Android OTA file
```
# otaripper -p 8747a387d2024e9a924dc8624126d4a2.zip --partitions dtbo
```

# 3. Modify dtbo.img for Oneplus13

Using vim dtbo.img, find rmbird string and replace to xxbird

![xxbird](https://khaidox.github.io/assets/img/oneplus13/1.png)

# 4. Flash to Oneplus13

```
# adb reboot bootloader
# fastboot --set-active=a
# fastboot flash dtbo dtbo.img
# fastboot reboot
```

Note: This is a modify dtbo partition, so you can't lock bootloader on your phone

