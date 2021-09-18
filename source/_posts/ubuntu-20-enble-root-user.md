---
title: Ubuntu 20启用root账户
date: 2021-09-18 19:43:12
categories:
- guide
tags:
- Linux
- Ubuntu
- root
---

無聊，就是想把root賬戶給打開。

## 設置root賬戶密碼
```bash
sudo passwd root
```

## 修改`50-ubuntu.conf`
```bash
sudo gedit /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf
```

修改這兩個：
```config
greeter-show-manual-login=true
all-guest=false
```

<!-- more -->

`20`以前的好像到此就成功了。

## 修改`gdm-autologin`
```bash
sudo gedit /etc/pam.d/gdm-autologin
```

行首加`#`註釋掉：
```
auth required pam_succeed_if.so user != root quiet_success
```

## 修改`gdm-password`
```bash
sudo gedit /etc/pam.d/gdm-password
```

行首加`#`註釋掉：
```
auth required pam_succeed_if.so user != root quiet_success
```

## 修改`/root/.profile`
```bash
sudo gedit /root/.profile
```

替換`mesg n 2> /dev/null || true`爲`tty -s&&mesg n || true`。

