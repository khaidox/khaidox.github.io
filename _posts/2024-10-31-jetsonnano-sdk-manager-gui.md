---
created: 2024-10-31
title: Open NVIDA SDK Manager GUI within Docker container
layout: post
tags: [jetson]
category: [Jetson Nano]
author: khaido
comments: true
---

A few days ago, I needed to use SPI on the Jetson Nano and flash its device tree. According to the document of Nvidia, I need a host OS with Ubuntu 18.04 to compatible with jetson nano using JetPack 4.x. This is challenging becasue I am using Ubuntu 24.04. Therefore, I dectected to run Nvidia SDK Manager GUI within a Docker container to resolve the required environment

# 1. Installtion
```
docker pull okok97/sdkmanager-gui
```

# 2. Running on Linux

```
xhost + >/dev/null; sudo docker run \
	-it --rm \
	--net=host \
	--privileged \
	--ipc=host \
	-v /dev/bus/usb:/dev/bus/usb/ \
	-v /dev:/dev \
	-v /tmp/.X11-unix/:/tmp/.X11-unix \
	-v $(pwd)/nvidia:/home/nvidia/ \
	-e DISPLAY=$DISPLAY \
	okok97/sdkmanager-gui
```
You can login with QR code on right top corrner off login screen.

![sdk_gui](https://khaidox.github.io/assets/img/jetson/nvidia_sdk_gui.png)