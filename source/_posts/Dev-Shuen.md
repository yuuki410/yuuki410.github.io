---
title: Shuen开发 疑难笔记
date: 2021-09-11 18:54:01
categories:
- note
tags:
- Node
- Electron
- Webpack
- Chalk
- pnpm
---

记一下折腾[Shuen](https://github.com/Zokhoi/wikidot-autosaver)的经历，顺带方便以后遇到问题快速解决。

## 预准备
需要包管理器：
```bash
npm i -g pnpm yarn
```

需要全局安装的包依赖：
```bash
pnpm i -g cross-env concurrently webpack webpack-cli chalk
```

运行`pnpm i`前需要先：
```bash
pnpm clonewj
```

随后安装：
```bash
pnpm i
```

<!-- more -->

## `@babel/register`
有时可能报错显示缺失`@babel/register`或版本过低，重新安装：
```bash
pnpm uninstall -wD @babel/register
pnpm uninstall -wD @babel-register
pnpm uninstall -wD babel/register
pnpm uninstall -wD babel-register
pnpm i -wD @babel/register
```

## `fs/promises`
使用Node 14以下版本是Electron-build将报错提示`cannot find module fs/promises`。
可应急替换`node_modules`中所有`require("fs/promises")`至`require("fs").promises`。

或安装`Electron-build@v22.10.x`：

> 参：<https://blog.csdn.net/qq_32337279/article/details/118328202>

此外更多等遇到了再更新。