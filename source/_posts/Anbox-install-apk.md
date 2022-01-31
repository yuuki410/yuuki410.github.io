---
title: Anbox安装apk包
date: 2022-01-31 14:28:43
categories:
- note
tags:
- Anbox
- adb
---

首先确认emulator的连接：
```bash
$ which adb
/usr/bin/adb
$ adb devices        
List of devices attached
emulator-5558	device
```

然后就可以安装apk包了，文件名最好不要带特殊字符，例如汉字：
```bash
adb install Downloads/myapp.apk
```