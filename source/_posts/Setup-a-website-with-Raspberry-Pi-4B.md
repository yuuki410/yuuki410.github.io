---
title: 用树莓派4B搭建网站
date: 2022-02-09 16:41:15
categories:
- guide
tags:
- Raspberry Pi
- Ubuntu
- NAT
- Cloudflare
- Cloudflare Tunnel
- eu.org
- DNS
- domain
---

事先声明：
本教程的作者**不提供任何担保也不承担任何附带责任**，若你询问我任何与本教程相关的问题，我可以*不回复*、*不作出正确回复*、*不提供相关资源*。

如果你有足够的时间来折腾这些的话，你可以继续看下去，本教程只需要你拥有：
- 充裕的时间
- 网络连接
- 电力供应
- 电子邮箱
- 一块Raspberry Pi 4B及其配件（Micro SD卡，电源）

当你准备好后，请接着往下阅读。

<!-- more -->

## 树莓派

我建议使用Raspberry Pi 4B，原因是该型号的算力较前有很大提升，建议RAM大小不小于2GB，以防编译某些程序时出错。
你可以前往官网购买：https://www.raspberrypi.com/products/raspberry-pi-4-model-b/
或在其它地方购买（请不要询问我*其它地方*是哪里）。

我建议使用Ubuntu系统，对于新手来说它比较友好。

### 刷写SD卡

#### 选取系统
我推荐使用Raspberry官网提供的刷写工具Raspberry Pi Imager，你可以在官网上下载该工具：https://www.raspberrypi.com/software/

若你看不懂英文并且使用Windows系统，可以直接使用此链接下载：https://downloads.raspberrypi.org/imager/imager_latest.exe

随后在电脑上插入Micro SD卡（你可能会需要使用读卡器），启动该工具，依次单击`选择操作系统`(也可能显示为`CHOOSE OS`) > `Other general-purpose OS` > `Ubuntu`。

你将看到许多相似的选项，注意选择`Ubuntu Server <版本号(一串数字)> LTS (RPi 4)`，Server表示服务器，LTS表示长期支持版本，括弧内的RPi 4表示适用于Raspberry Pi 4，你实际看到的括弧内的文字可能是`RPi 3/4/400`等，只需注意是否包含`4`即可。

特别的，如果有多个选项符合上述要求，请注意粗体文本下面的描述文字，它应该包含`64-bit`（意为64位操作系统）和`arm64`（一种CPU架构，即Raspberry Pi 4B的CPU架构）。

若仍有多个选项符合上述要求，请单击版本号数字最大者。

#### 选取SD卡

此时你已经回到一开始的界面，单击`选择SD卡`(也可能显示为`CHOOSE STORAGE`)，找到你插入的SD卡并单击它，如果你不能分辨哪一个是你插入的SD，请拔出所有连接到电脑的存储设备。

#### 烧录

按下`烧录`(也可能显示为`WRITE`)按钮，等待进度条完成，然后弹出SD卡，插到树莓派上。

这可能需要一段时间，在等待的时间里你可以继续后面的操作。

## 启动树莓派

将树莓派连接上有线网络（我不想提供WiFi连接的教程，它既复杂又不稳定），然后打开路由表，找出名为Ubuntu的设备，记录其IP地址。

此处示例的IP为`192.168.1.107`。

## 安装宝塔面板

若你使用Windows，你需要下载ssh客户端，我建议使用[PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)，若你不懂英文并且使用64位Windows系统，可以直接使用此链接下载：https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.76-installer.msi

此处不提供安装PuTTY的教程。

使用ssh以用户`ubuntu`连接上树莓派，此处示例IP地址为`192.168.1.107`，请根据你的实际情况替换，若你使用Linux则可以输入此命令：
```bash
ssh ubuntu@192.168.1.107
```

第一次连接时，系统将会要求你设置密码，请牢记此密码，否则你需要从<#刷写SD卡>重新开始。

连接上树莓派后键入以下命令以安装[宝塔面板](https://www.bt.cn/download/linux.html)：
```bash
wget -O install.sh http://download.bt.cn/install/install-ubuntu_6.0.sh && sudo bash install.sh
```

根据提示操作即可。

安装完成时，将会输出面板信息，包括面板访问地址、默认用户、默认密码信息，请记录下这些内容。

### 配置面板

用浏览器打开面板地址，你可以按住Ctrl同时用单击URL以快速打开链接。

输入刚刚的默认用户和密码以登录。

按提示继续默认配置即可。

完成后转到左侧栏目中的`面板设置`，修改`安全入口`、`面板用户`、`面板密码`。

## 申请eu.org域名

这是一个免费二级域名提供商，老牌且可靠，最重要的是，大部分知名服务商都承认它作为根域。

进入它的注册页：<https://nic.eu.org/arf/en/contact/create/>

填写信息并注册，请尽可能写地像真的一样，若你看不懂英文请找人帮忙填写。
请注意使用国外的邮箱，例如GMail、Yahoo等知名邮件服务，切勿使用匿名邮件服务，并牢记你填写的密码。
请注意选中`Private (not shown in the public Whois)`（防止Whois泄露你填写的地址和邮件）和`I have read and I accept the domain policy`（同意域名条款）。

你会在邮箱中收到一封确认邮件，邮件包含一个确认链接和你的登录用户名，请牢记此用户名，然后单击确认链接激活账户。

在<https://nic.eu.org/arf/en/login/>使用刚才的用户名和你的密码登录，单击`New Domain`按钮。

在`Complete domain name`一栏填入你想要注册的二级域名，此处示例使用的是：`yuuki.eu.org`。

转到下方的`Name Server`填写DNS解析服务器信息，然后按`Submit`提交并等待屏幕上出现成功输出。
特别的，若你使用HE.net提供的解析服务，请在`Check for correctness of`一栏勾选`server names`。

然后你需要等待人工审核，这是为什么建议你填写信息时要写得真实些的原因。
审核通过后你会收到电子邮件通知，接着我们就可以继续下一步了。

但是，在大多数情况下，你还暂时没有DNS解析服务可用，因此你需要先注册一个DNS服务。
eu.org要求你要注册的域名已经在一个DNS服务商上具有解析记录，允许为尚未注册的域名提供解析的DNS服务商有：
- DNSPod（国内由腾讯代理）
- HE.net

这里我们选择HE.net。

### 注册HE.net
前往它的注册页面：<https://ipv6.he.net/certification/register.php>

填写信息并注册，随后你会收到确认邮件，单击其中的链接激活账户。

然后打开：<https://dns.he.net/>
在左侧的登录框中填入你的用户名和密码登录。

在左侧`Zone Functions`一栏中单击`Add a new domain`，键入你要注册的eu.org子域名，此处示例使用的是：`yuuki.eu.org`。

单击`Add Domain!`按钮，若你退回了登录界面，重新登录即可。

页面正中会列出你刚刚填入的域名，单击其侧的![Edit Zone](https://dns.he.net/include/images/pencil.png)按钮，若你退回了登录界面，重新登录再单击此按钮即可。

单击`New A`按钮，在`Name`中键入`www`，`IPv4 Address`中键入`8.8.8.8`，然后单击`Submit`，若你退回了登录界面，重新执行上述操作。
（此处键入的解析记录仅作注册用，若你认真填写的话也无妨）

然后你就可以在继续在eu.org申请域名了。

## 注册Cloudflare

接下来我们使用Cloudflare解析域名和内网穿透，前往<https://dash.cloudflare.com/sign-up>以注册账户，若显示界面不是中文，请在右上角找到语言设置按钮切换为中文。

填入电子邮箱和密码，请牢记此密码。

随后你会收到一封确认邮件，单击其中的链接激活账户。

接着前往<https://dash.cloudflare.com/>登录账户，登录成功后你会跳转到控制台，单击`添加站点`，输入你注册的eu.org域名，此处示例使用的是：`yuuki.eu.org`。

Cloudflare会询问你是否导入已有的解析记录，你可以任意选择，随后Cloudflare会要求你更换域名的解析服务，复制它提供的两个解析服务器的域名，然后登录nic.eu.org：<https://nic.eu.org/arf/>

你会看到你申请的eu.org二级域名，单击其右的`►`按钮，接着在`Edit`一栏中单击`Nameservers`，在`Name1`和`Name2`两栏中填入Cloudflare提供的两个解析服务器域名，接着单击`Submit`提交修改。

你可能需要等待几小时让Cloudflare确认你已经将DNS解析服务指向它的服务，确认成功后你会收到电子邮件通知。

## 安装Cloudflared

为穿透[电信级NAT](https://zh.wikipedia.org/wiki/%E7%94%B5%E4%BF%A1%E7%BA%A7NAT)，我们需要使用内网穿透服务，以下知名服务器提供此服务：
- 花生壳
- [Ngrok](https://ngrok.com/)
- Cloudflare Tunnel（另名：Argo Tunnel）

这里我们选择Cloudflare Tunnel。

安装其客户端可参考<https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation>与<https://pkg.cloudflare.com/#ubuntu-title>的教程（英文）。

通过ssh连接上树莓派，执行命令：
```bash
echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/ focal main' |
sudo tee /etc/apt/sources.list.d/cloudflare-main.list
sudo curl https://pkg.cloudflare.com/cloudflare-main.gpg -o /usr/share/keyrings/cloudflare-main.gpg 
sudo apt-get update
sudo apt-get install cloudflared
```

接着执行以下命令，授权Cloudflared访问你的域名：
```bash
cloudflared tunnel login
```

浏览器不会自动开启，请复制命令行上输出的网址，粘贴到浏览器中开启，选择你刚刚注册的域名，此处示例使用的是：`yuuki.eu.org`。

回到ssh，继续执行命令：
```bash
cloudflared tunnel create <隧道名称>
```
隧道名称可以填入你能记住的名字，此处示例使用的是：`yuuki410`。
执行完成后输出的信息请记录下来。

创建一个配置文件：
```bash
cd ~/.cloudflared
touch config.yml
name config.yml
```

粘贴以下内容，然后按Ctrl+O，回车以保存文件：
```yaml
tunnel: yuuki410 # 这是示例，请替换你刚刚填写的隧道名称
credentials-file: # 请填入 /用户目录路径/.cloudflared/<隧道UUID>.json，在刚才的输出信息中有
ingress:
  - hostname: "yuuki.eu.org" # 这是示例，请替换成你申请的eu.org域名
    service: "http://localhost"
  - hostname: "*.yuuki.eu.org" # 这是示例，请替换成你申请的eu.org域名
    service: "http://localhost"
  - service: "http_status:404"
```
接着按Ctrl+X离开编辑器。

回到ssh，继续执行命令：
```bash
cloudflared tunnel route dns <隧道名> <域名>
```

隧道名是你之前填入的隧道名称，此处示例使用的是`yuuki410`；域名是你申请的eu.org域名，此处示例使用的是`yuuki.eu.org`。

执行：
```bash
cloudflared tunnel --config config.yml run
```

在浏览器中打开你注册的域名看看有无内容显示出来。

## 其它

本教程到此结束。

要保持你的树莓派可以访问，可以参考我的另一篇博客：[Raspi NAT 内网穿透§作为服务](https://blog.yuuki.eu.org/note/Raspi-NAT/#%E4%BD%9C%E4%B8%BA%E6%9C%8D%E5%8A%A1)。

要搭建网站，例如博客，请自行上网搜索相应教程。

## 另外

这篇估计是目前为止我写的最长的一篇博客了，如果本文对你有帮助的话，不妨到本博客的Github仓库给个Star：<https://github.com/yuuki410/yuuki410.github.io/>。