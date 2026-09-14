---
created: 2024-10-31
title: Unbrick/debrick router TP-LINK WR841ND v9
layout: post
tags: [tplink,spi]
category: [TP-LINK]
author: khaido
comments: true
---

# Unbrick/debrick router TP-LINK WR841ND v9 using UART, U-Boot console and TFTP server

1. Set ipaddr and serverip environment variables
```
setenv ipaddr 192.168.1.21
setenv serverip 192.168.1.13
```

2. Download and store in RAM proper image for your router, using tftpboot command in U-Boot console. 
Download [u-boot.bin](https://khaidox.github.io/assets/files/u-boot.bin)
```
tftpboot 0x80800000 step_run/u-boot.bin
```



3. Erase appropriate FLASH space for new U-Boot image (this command will remove default U-Boot image!):
```
erase 0x9f000000 +0x20000
```

4. Now your router does not have U-Boot, so do not wait and copy to FLASH the new one, stored earlier in RAM:
```
cp.b 0x9f000000 0x80800000 0x30000
```

5. If you are sure that everything went OK, you may reset the board:
```
reset
```

# Log boot

```
U-Boot (Dec  3 2024 - 17:22:25)

DRAM:  32 MB

 relocating to address 81ff8000 
 Compressed Image at 9f0059e0 
 Disabling all the interrupts
   Uncompressing UBoot Image ... 
U-Boot uncompress address 80010000
 Uncompression completed successfully with destLen 80928
 U-Boot Load address 80010000
 

U-Boot (khaid - Build from LSDK-20241203_17h22m17s_khaid at Dec  3 2024 - 17:22:23)

ap143 - Honey Bee 1.1

DRAM:   32 MB
Flash Manuf Id 0xef, DeviceId0 0x40, DeviceId1 0x17
Flash:  4 MB
Using default environment

In:    serial
Out:   serial
Err:   serial
Net:   ath_gmac_enet_initialize...
ath_gmac_enet_initialize: reset mask:0xc02200
Scorpion ---->S27 PHY*
S27 reg init
GMAC: cfg1 0x800c0000 cfg2 0x7114
eth0: ba:be:fa:ce:08:41
athrs27_phy_setup ATHR_PHY_CONTROL 4:0x1000
athrs27_phy_setup ATHR_PHY_SPEC_STAUS 4:0x10
eth0 up
Honey Bee ---->  MAC 1 S27 PHY*
S27 reg init
ATHRS27: resetting s27
ATHRS27: s27 reset done
GMAC: cfg1 0x800c0000 cfg2 0x7214
eth1: ba:be:fa:ce:08:41
athrs27_phy_setup ATHR_PHY_CONTROL 0:0x1000
athrs27_phy_setup ATHR_PHY_SPEC_STAUS 0:0x10
athrs27_phy_setup ATHR_PHY_CONTROL 1:0x1000
athrs27_phy_setup ATHR_PHY_SPEC_STAUS 1:0x10
athrs27_phy_setup ATHR_PHY_CONTROL 2:0x1000
athrs27_phy_setup ATHR_PHY_SPEC_STAUS 2:0x10
athrs27_phy_setup ATHR_PHY_CONTROL 3:0x1000
athrs27_phy_setup ATHR_PHY_SPEC_STAUS 3:0x10
eth1 up
eth0, eth1
Setting 0x181162c0 to 0x4b97a100
is_auto_upload_firmware=0
Autobooting in 1 seconds
## Booting image at 9f020000 ...
```