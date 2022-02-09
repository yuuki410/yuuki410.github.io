---
title: Raspi NAT内网穿透
date: 2021-12-11 19:27:46
categories:
- note
tags:
- Raspberry Pi
- NAT
- Cloudflare
- Cloudflare Tunnel
---

首先选择你要使用的NAT服务商：
- [Clouflare Tunnel (Argo Tunnel)](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup)
- [Cpolar](https://www.cpolar.com/)
- [NAT123](www.nat123.com/)
- [Ngrok](https://ngrok.com/)
- [Ngrok CC](https://ngrok.cc/)

下面只叙述Cloudflare Tunnel的用法。

## 注册CF账户

## 下载服务端
你可以在[这里](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation)找到适合你的设备的包。

## 登录账户

在命令行执行：
```sh
cloudflared tunnel login
```

这会打开浏览器要求你选择一个域名来部署。

## 创建隧道

```sh
cloudflared tunnel create <隧道名>
```

### 创建隧道配置

将以下信息写入一个`yaml`文件中：
```yaml
tunnel: <隧道UUID，就是你执行了创建隧道后返回的那串，填隧道名也可以>
credentials-file: (通常是) /用户目录/.cloudflared/<隧道UUID>.json
```

### DNS记录

```sh
cloudflared tunnel route dns <隧道UUID或隧道名> <域名，如www.example.com>
```
注意这里的域名必须是你托管在cf上的。

### 运行隧道

```sh
cloudflared tunnel --config <配置文件路径> run
```

## 作为服务

当然，我们不能让隧道每次都要手动启动，也不能开个screen放着。

执行下面的命令即可注册系统服务：
```sh
cloudflared --config <配置文件路径> service install
```
经过实测，不知道为什么在ubuntu服务器上死活识别不了默认配置文件路径，所以这里就不放默认方法了。

启动服务的命令：
```sh
sudo systemctl start cloudflared
```

设置开机启动服务：
```sh
sudo systemctl enable cloudflared
```