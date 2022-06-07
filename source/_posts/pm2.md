---
title: 使用pm2管理进程实例
date: 2022-06-07 16:40:59
categories: 
#- diary
- guide
#- note
#- source
#- tool
tags:
- pm2
---

## 安装
pm2可以使用yarn安装：
```shell
yarn global add pm2
```

也可以使用npm安装：
```shell
npm install --location=global pm2
```

## 使用
pm2的基础用法：
```shell
pm2 start index.js
```
这将使用`node index.js`启动当前目录下的`index.js`。
<!-- more -->
常用的参数：
| 参数 | 功能 |
| ---: | :--- |
| `-n` `--name` | 指定进程名称，否则自动生成 |
| `-c` `--cron` | 指定一个`cron`表达式，进程将自动在指定的时间重启 |
| `--max-memory-restart` | 指定最大内存占用大小，单位为字节（Byte），达到上限时进程将被重启 |
| `-m` `--mini-list` | 不要格式化输出 |

除了`start`以外，还有以下常用指令：
| 指令 | 功能 |
| ---: | :--- |
| `start` | 启动一个进程 |
| `stop` | 停止一个进程|
| `restart` |
| `list` `ls` `l` | 列出进程 |
| `show` | 指定进程名或id，显示进程的详细信息 |
| `delete` `del` | 停止一个进程，并删除它的配置 |
| `logs` | 显示一个进程的输出（日志） |