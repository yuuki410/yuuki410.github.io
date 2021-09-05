---
title: 关于VSCode字符编码
date: 2021-09-05 20:26:50
categories:
- help
- Windows
tags: 
- VSCode
- Unicode
- Windows
---

Windows上跑node.js时报错：
```
PS C:~> node index.js
C:~\index.js:1
��v


SyntaxError: Invalid or unexpected token
    at Object.compileFunction (vm.js:344:18)
    at wrapSafe (internal/modules/cjs/loader.js:1106:15)
    at Module._compile (internal/modules/cjs/loader.js:1140:27)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1196:10)
    at Module.load (internal/modules/cjs/loader.js:1040:32)
    at Function.Module._load (internal/modules/cjs/loader.js:929:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:71:12)
    at internal/main/run_main_module.js:17:47
```

经查，确认为UTF-16 LE编码所引起的问题。

VSCode疑似默认文件为UTF-16 LE，悲剧。

解决时切换其编码至UTF-8重新存储即可。