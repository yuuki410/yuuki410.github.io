---
title: Raspi VNC连接分辨率调整
date: 2021-12-11 18:42:35
categories:
- Raspi
tags:
- Raspberry Pi
- VNC
- RealVNC
---

树莓派默认的分辨率是480p……其酸爽程度自行体会……

ssh连上树莓派，一句话解决分辨率问题：
```sh
vncserver -geometry 1920x1080
```
当然，这么做只能暂时性地解决问题。其原理是启动了另一个VNC Server进程

一劳永逸的方法是改设置：
```sh
sudo vi /etc/sysconfig/vncservers
```
配置文件简明易懂，记得备份好就行。

> **参考：**
- https://blog.csdn.net/runningtortoises/article/details/51425332

------

新系统的`/etc/sysconfig`貌似移动到了别处，可用`sudo raspi-config` > `2 Display Options` > `D1 Resolution`设置分辨率，这会同时应用到显示器和VNC。