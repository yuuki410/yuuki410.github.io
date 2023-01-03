---
title: Anaconda crash command Solution
date: 2023-01-03 20:08:09
categories:
- guide
tags:
- Linux
- Anaconda
- IBus
- python
- terminal
copyright: false
---

{% note danger no-icon %}
#### Copyright reserved 著作权保留
原作者没有特别声明，默认保留所有权利
{% endnote %}

## Anaconda with `clear` command

Anaconda 导致：
- `clear` 命令不起作用
  - 显示 `terminals database is inaccessible`

```bash
$ clear
terminals database is inaccessible
```

> https://github.com/ContinuumIO/anaconda-issues/issues/331

```bash
sudo mv $CONDA_PREFIX/bin/clear $CONDA_PREFIX/bin/clear_old
```

## Anaconda with `ibus`

Anaconda 导致：
- `ibus` 冲突
  - 提示无法导入 `gi` 包

```bash
$ ibus-setup
Traceback (most recent call last):
File "/usr/share/ibus/setup/main.py", line 34, in <module>
from gi import require_version as gi_require_version
ImportError: cannot import name 'require_version'
```

### Solution 1

> https://blog.csdn.net/weixin_30764771/article/details/96301488

```bash
# Check where Anaconda is
whereis anaconda # anaconda: /opt/anaconda/bin/anaconda
# Disable it
sudo chmod 000 /opt/anaconda/
```

### Solution 2

```bash
# Check your $PATH
printenv | grep PATH 
# my $PATH:
# PATH=/opt/anaconda/bin:/opt/anaconda/condabin:/home/yuuki410/.local/bin:/usr/local/sbin:/usr/local/bin:/usr/bin

# Set an new $PATH without Anaconda
export PATH=/home/yuuki410/.local/bin:/opt/gcc-arm-none-eabi-10.3-2021.10/bin:/usr/local/sbin:/usr/local/bin:/usr/bin:/home/yuuki410/.dotnet/tools:/usr/lib/jvm/default/bin:/usr/bin/site_perl:/usr/bin/vendor_perl:/usr/bin/core_perl
WINDOWPATH=1
```