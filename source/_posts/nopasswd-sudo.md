---
title: sudo命令免密码
date: 2021-09-25 16:30:13
categories:
- guide
tags:
- Linux
- Ubuntu
- sudo
---

> 每次打开apt，改hosts都要输密码，烦死了……

键入命令：
```bash
sudo visudo
```
若不能，直接开启配置文件：
```bash
sudo vi /etc/sudoers
```

## 法一：修改用户权限

于末尾加入行：
```
<YOUR USER NAME>	ALL=(ALL:ALL) NOPASSWD:ALL
```

例如我的用户名为`yuuki410`，在末尾加入行：
```
yuuki410	ALL=(ALL:ALL) NOPASSWD:ALL
```

注意在末尾加入，否则将被用户组设置覆盖。

<!-- more -->

## 法二：修改用户组权限

查询当前用户组：
```bash
groups
```

然后更新用户组设置：
```
%<GROUP NAME>	ALL=(ALL:ALL) NOPASSWD:ALL
```

例如我更新了`sudo`组，便将：
```
%sudo	ALL=(ALL:ALL) ALL
```
替换为：
```
%sudo	ALL=(ALL:ALL) NOPASSWD:ALL
```

`Ubuntu 20`测试通过。