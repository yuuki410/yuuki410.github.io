---
title: Wine GBK 中文乱码 解决
date: 2023-01-04 14:42:45
categories:
- guide
tags:
- Linux
- wine
- GBK
- GB18030
- GB2312
- Encoding
- 乱码
---

{% note primary danger Image Copyright reserved 图像著作权保留 %}
#### Image Copyright reserved 图像著作权保留
以下两张截图来自易语言编辑器，该界面设计版权归属原作者所有。  
The following two screenshots are from Easy Language editor, and the copyright of the user interface design belongs to the original author.
{% gp 2 %}
{% asset_img before.png %} Before
{% asset_img after.png %} After
{% endgp %}
{% endnote %}

{% gp 2 %}
{% asset_img before.png %} Before
{% asset_img after.png %} After
{% endgp %}

Edit `locale.gen`:
```bash /etc/locale.gen
# Uncomment these line
zh_CN.GB18030 GB18030
zh_CN.GBK GBK
zh_CN.UTF-8 UTF-8
zh_CN GB2312
```

Then, run command:
```bash
sudo locale-gen
```

Start `wine`:
```bash
export LC_ALL=zh_CN.GBK # Change zh_CN.GBK to zh_CN.GB2312 / zh_CN.GB18030 if it not work
wine <excutable.exe>
```
