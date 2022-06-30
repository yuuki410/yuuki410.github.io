---
title: pkg-config and libcrypto++
date: 2022-06-30 21:31:56
categories:
- note
tags:
- pkg-config
- libcrypto++
---

Ubuntu:  
```bash
sudo apt-get install libcrypto++6 libcrypto++-dev -y
pkg-config --cflags libcrypto++
```

Arch Linux:  
```bash
sudo pacman -Syyu crypto++
pkg-config --cflags libcryptopp
```

----

为什么会有这么多的别名