---
title: 通过 `code-server` 自己搭建 workspace
date: 2022-06-06 09:26:43
categories: guide
tags:
- VS Code
- web server
---

## 安装`code-server`

### 源码运行

首先需要安装`yarn`，可以通过`npm`安装：
```shell
npm i -g yarn
```

也可以使用`apt`，需要添加源再安装：
```shell
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update
sudo apt-get install yarn -y
```

若需要卸载旧版或者不完整的`yarn`，运行：
```shell
sudo apt remove cmdtest
sudo apt remove yarn
```

接着添加`code-server`：
```shell
yarn global add code-server
```
<!-- more -->
编译依赖可能需要一段时间。

### Docker运行

需要先安装好Docker，参考[官方文档](https://docs.docker.com/engine/install/)，本文不做叙述。

拉取镜像：
```shell
docker pull codercom/code-server:latest
```

部署运行Docker：
```shell
docker run -d \
--name=[容器名] \
-e PASSWORD=[密码] \
-e SUDO_PASSWORD=[root密码] \
-p [外部端口]:8080 \
--restart unless-stopped \
codercom/code-server
```

部署后还需要进入容器修改code server配置：
```shell
docker exec [容器名] /bin/bash
```

## 修改配置

编辑`~/.config/code-server/config.yaml`：
```shell
nano ~/.config/code-server/config.yaml
```

修改`password`为你需要的密码，这是网页登陆code server需要的，而非docker和linux密码。

如不需要密码，修改`auth`为`none`。

## 运行code server
直接运行源码：
```bash
code-server
```

Docker需要重启才能应用配置。
