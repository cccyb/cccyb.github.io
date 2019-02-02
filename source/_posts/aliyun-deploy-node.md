---
title: 阿里云部署项目笔记——Node篇
date: 2017/05/10 12:46:25
categories:
- Notes
tags:
- Server
- Node
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs832il5hj20jg0cs10v.jpg
---

最近找了个网易云的API项目，由于是`Node`项目，便计划着部署到服务器上，方便访问，顺便学一学`Node`项目的部署。

## 在服务器上安装Node

先在终端使用`ssh username@ip`命令远程连接服务器。

### 下载源码
你需要在<https://nodejs.org/en/download/>下载最新的`Nodejs`版本，本文以现在的最新的`v6.10.3`为例:

```bash
cd /usr/local/src/
wget http://nodejs.org/dist/v6.10.3/node-v6.10.3.tar.gz
```

若直接下载后续解压出现问题，可以直接在`Nodejs`官网下载编译好的`Linux`二进制包，然后上传到服务器`/usr/local/src/`目录下。

### 解压源码

```
cd /usr/local/src/
tar zxvf node-v6.10.3.tar.gz
```

### 编译安装

```
cd node-v6.10.3
./configure --prefix=/usr/local/node/6.10.3
make
make install
```

这里`make`过程可能比较久，需要耐心等待。

### 配置`NODE_HOME`，进入`profile`编辑环境变量

```
vim /etc/profile
```

设置 `Node.js` 环境变量，在 `export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL` 一行的上面添加如下内容:

```
#set for nodejsexport NODE_HOME=/usr/local/node/6.10.3export PATH=$NODE_HOME/bin:$PATH 
```

`:wq`保存并退出，编译 `/etc/profile` 使配置生效

```bash
source /etc/profile 
```

### 验证是否安装配置成功

```bash
node -v
```

输出 `v6.10.3` 表示配置成功
npm模块安装路径

```
/usr/local/node/6.10.3/lib/node_modules/ 
```

## 传输项目

将需要部署的项目通过可视化工具或者命令行传输到服务器上，具体方法见上一篇博客[阿里云部署项目笔记之Vue篇](http://www.chenyubo.me/2017/05/10/deploy-node-project/)

## 安装pm2

执行命令安装`pm2`来守护进程：

```
npm install pm2 -g
```

## 开启项目

进入项目文件夹内，执行命令：

```
pm2 start app.js
```

至此结束。