---
title: 在Linux上安全地移除移动存储设备
date: 2022-08-06 21:21:26
categories:
- guide
tags:
- Linux
- mount
- udisksctl
---

首先需要卸载挂载：

```bash
umount <path-to-mount-file>
```

如果你使用`nautilus`文件管理器，其左栏中有一方便的按钮可以轻松卸载设备。

对于更普遍的情况，使用`udisksctl`关闭设备电源：

```bash
udisksctl power-off -p <block-device-path>
```

然后再移除设备。