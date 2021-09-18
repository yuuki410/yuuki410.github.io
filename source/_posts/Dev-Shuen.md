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

> 21年9月18日更新
鑿海新增了一個Vite分支
然後也寫好了Readme文件
所以這篇就當回憶和樂子來看吧

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

## `ℹ ｢wdm｣: Compiled with warnings.`
```
$ pnpm start
...
> shuen@ start:main ~/wikidot-autosaver
> cross-env NODE_ENV=development electron -r ./.erb/scripts/BabelRegister ./src/main.dev.ts

      ℹ ｢wdm｣: Compiled with warnings.
      Failed to fetch extension, trying 4 more times
Failed to fetch extension, trying 3 more times
Failed to fetch extension, trying 2 more times
Failed to fetch extension, trying 1 more times
Failed to fetch extension, trying 0 more times
Error: net::ERR_CONNECTION_TIMED_OUT
    at SimpleURLLoaderWrapper.<anonymous> (electron/js2c/browser_init.js:105:6536)
    at SimpleURLLoaderWrapper.emit (events.js:315:20)
19:44:54.557 › APPIMAGE env is not defined, current application is not an AppImage
(node:3993) electron: The default of contextIsolation is deprecated and will be changing from false to true in a future release of Electron.  See https://github.com/electron/electron/issues/23506 for more information
```
挂梯，重新`pnpm i`。

> 参：
- <https://www.cnblogs.com/bettergoo/p/13974178.html>
- <https://segmentfault.com/q/1010000012960184>

<!--
> 参：
- 设置Mode <https://www.webpackjs.com/concepts/mode/>
- <http://www.imooc.com/wenda/detail/395279>
- <https://coding.imooc.com/learn/questiondetail/5NAr19XnWWE6LBEz.html>
-->

## `throw er; // Unhandled 'error' event`
`webpack`报错提示：
```
events.js:
      throw er; // Unhandled 'error' event
      ^
```

> 参：<https://www.cnblogs.com/vientiane/p/9959148.html>

此外更多等遇到了再更新。

最后感叹一句，遇到问题一定要用Google搜索报错输出，其它搜索引擎根本匹配不到输出内容（除非是堆栈等大型网站）。