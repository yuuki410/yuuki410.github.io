---
title: Insert your PGP Public Key Block when VIMing
date: 2023-01-07 14:45:07
categories:
- guide
tags:
- Linux
- Vim
- PGP
- zsh
---

Vim功能：按下两次 {% button #,! %} 可执行外部命令并把命令输出插入到光标所在位置。

ZSH功能：可使用`alias`设置命令别名。

```bash ~/.zshenv
alias -g mypgp="gpg --armor --export <PGP-Public-Key-ID>"
```
{% note info %}
您可以自由更改`mygpg`为任何您想要的别名。
{% endnote %}

使用`.zshenv`可以让设置全局生效，只要命令通过zsh执行（除非使用`-f`选项指定了脚本）。

{% note info %}
您可以尝试将上述内容添加到`.zshrc`然后试验一下效果，您将可以在TTY和Terminal环境下使用别名（alias），但是不能在Vim中通过外部命令执行alias。
{% endnote %}

然后，在Vim中尝试：
1. 按下两次 {% button #,! %}
2. 键入`mypgp`

大功告成，你也可以把相同的方法运用在其它命令上。