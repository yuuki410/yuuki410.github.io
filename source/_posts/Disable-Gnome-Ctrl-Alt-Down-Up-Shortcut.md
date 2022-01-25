---
title: 注销 Gnome Ctrl+Alt+Down/Up 快捷键
date: 2022-01-25 14:10:28
categories:
- guide
tags:
- Gnome
- 快捷键
---

由于Gnome的`Ctrl-Alt-Down/Up`快捷键和Discord冲突，使用体验很糟糕。

## 步骤

```bash
sudo apt-get install dconf-editor
dconf-editor
```

进入目录`/org/gnome/desktop/wm/keybindings/`，查找`switch-to-workspace-down`和`switch-to-workspace-up`，更改其值。

若要禁用则设置为`['disabled']`

## 参考

- https://unix.stackexchange.com/questions/394143/how-to-disable-gnome-ctrlaltdown-and-ctrlaltup-shortcut/420395