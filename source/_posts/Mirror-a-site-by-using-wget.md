---
title: Mirror a site by using wget
date: 2023-01-04 09:12:24
categories:
- note
tags:
- Linux
- wget
- Web
---

抓取整个站点：
```bash
wget -m -r -p -np -k -E 'https://example.com'
```

```bash
-r # 递归抓取
-k # 修复绝对链接为相对链接，适合本地浏览
-m # 镜像
-E # 将 MIME TYPE 为 `text/html` 的文档用 `.html` 扩展名保存
```

```bash
-e robots=off # 忽略 robots.txt 进行抓取，请注意这样使用可能违法
```
