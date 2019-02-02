---
title: 服务器入门指北
date: 2019/1/18 12:46:25
categories:
- Tutorials
tags:
- Node
- Vps
- Server
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs84zsrspj20jg0a0n5l.jpg
---

### 前言

说起为啥又双叒叕的折腾起新服务器了，具体原因概括起来应该就是以下几点：

- 以前在大学弄的阿里云学生主机过期了，
- 原来的阿里云学生主机上也部署了几个项目，想着还是捡回来的比较好
- 以前单纯为了搭ss买的vps硬盘和内存都太小了，装完ss基本就告罄了，也折腾不了其他东西了
- 国内的主机都有点太贵了，只能退而求其次买个国外的配置稍微好点的主机，一可以多折腾一些小东西，二可以顺便搭个ss，也就可以把以前的那个ss主机停了

总结起来就是一句话：不要怂，就是折腾，干就完了~

### 选择服务器

我选用的是国外比较著名的[Vlutr](https://www.vultr.com/)，洛杉矶主机，CentOs7.0系统，5刀/月的套餐。具体选啥主机，选啥套餐，用啥系统，根据每个人的需求因人而异，我不多做介绍。如果有兴趣的可以看下我同事写的文章，[国外服务器如何选择以及搭建ShadowSocks教程](http://taoweng.site/index.php/archives/99/#directory01186845456699059412)，里面比较详细的介绍了如果选择国外服务器。

### 登录服务器

购买完主机了，就可以开始登录我们的服务器了。由于本人使用的是Mac系统，Mac的终端自带ssh，下面都以Mac系统为主，所以如果使用Windows的同学，可以自行百度搜索如何登录服务器，比较著名的是Putty和Xshell。

打开iTerm终端，username一般默认是root，然后在终端使用`ssh root@ip`命令远程连接服务器，在Vultr主机信息页面复制密码，在终端粘贴即可，**注意，这里输入密码时终端是看不到的，**粘贴完回车即可。

正常情况下，没有意外的话，你的终端应该已经成功进入了服务器的终端命令行输入界面。

但是，如果你输完ip，一直处于空白等待状态，没有提示你输入密码，那这时候要注意了，极有可能是你申请的主机ip和端口被墙了，导致ssh连接不上，这时你就需要检测一下主机是否真的被墙了。

具体可以见[Vultr 能 Ping 但是 SSH 无法连接的解决办法](https://www.vultrcn.com/11.html)，按照里面的方法进行测试，如果确定是被ip和端口被封了，解决方案就是申请新的服务器主机以获取新的ip**（注意：这里操作时必须保留原有的服务器，再额外申请新的服务器，这样才能获得新的ip，如果你先删除原有的服务器，然后在申请，这样申请到的服务器ip是和原先的一样的，）**，同时对新得到的ip继续进行ssh登录，如果ssh登录时提示输入密码，即表示这台服务器的ip是可以使用的，此时可以删除前面申请的无效的服务器，否则重复申请操作直到获取一个可以使用的ip为止。

### 安装宝塔面板

简单好用的 Linux/Windows 面板：https://www.bt.cn/

建议安装一个宝塔面板，方便管理Linux，对于服务器小白特别适用，也适合我这种懒人，如果有服务器大神或者愿意折腾的小伙伴可以跳过。

1. 安装宝塔

- CentOS安装命令：

```bash
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
```

- Ubuntu/Deepin安装命令：

```bash
wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && sudo bash install.sh
```

- Debian安装命令：

```bash
wget -O install.sh http://download.bt.cn/install/install-ubuntu.sh && bash install.sh
```

- Fedora安装命令:

```bash
wget -O install.sh http://download.bt.cn/install/install.sh && bash install.sh
```

中间需要根据提示输入 “y” 或者自定义目录，新手不了解的情况下最好不要动。

2. 安装完终端会显示宝塔登录地址，以及随机生成的用户名密码，在浏览器中输入登录宝塔面板

3. 进入首页，会建议你安装web运行环境：LNMP(Linux + Nginx + MySQL + PHP)或LAMP(Linux + Apache + MySQL + PHP)，这里推荐使用LNMP，使用极速安装，对新手友好。

4. 点击侧边栏的面板设置，修改面板别名、默认端口、面板用户和密码。

5. 点击侧边栏的安全，修改ssh登录端口号。

6. 宝塔常用linux命令大全：https://www.bt.cn/btcode.html，有需要可以查看。

### 搭建 Shadowsocks 服务，科学上网

1. 检测是否安装Python和Pip

```bash
python -V
pip -V
```

如果正确显示python和pip的版本，则跳转第3步进行安装Shadowsocks，否则跳转第2步安装pip

2. 安装pip

```bash
yum install python-setuptools && easy_install pip
```

3. 安装Shadowsocks

```bash
pip install shadowsocks
```

4. 配置服务器参数

生成配置文件：`touch /etc/shadowsocks.json`

编辑配置文件：`vi /etc/shadowsocks.json`

写入以下配置：

```json
{
    "server":"my_server_ip", // 你的服务器ip
    "server_port": 8388, // 端口自定义
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword", // 自定义密码，后续客户端连接需要用到
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

- server：填写你的服务器的ip
- server_port：填写端口，自定义
- password：填写你自定义的密码，后续客户端连接需要用到

5. 开启防火墙端口

进入宝塔面板，点击安全，在下方端口界面出输入8388端口，点击放行即可。

或者使用命令行

```bash
firewall-cmd --permanent --zone=public --add-port=8388/tcp
firewall-cmd --reload
```

6. 后台运行服务

```bash
ssserver -c /etc/shadowsocks.json -d start
```

至此，服务端的搭建就完成了，后续只需要在你的电脑上下载一个客户端配置即可。