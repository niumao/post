---
title: install android-studio on debian xfce GUI
date: 2018-05-06 17:56:23
tags:
---
1.browser https://developer.android.com/studio/ and get download
2.unzip in the /usr/local/
3.open the /usr/local/bin/ and direct cmd the `sh studio.sh`
4.start new program
5.open tool -> avd manager and create new avd
6../emulator -avd Pixel_API_27 -use-system-libs 
7.apt-get install libstdc++ if not show
