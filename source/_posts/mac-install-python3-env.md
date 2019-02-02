---
title: Mac安装Python3开发环境随记
date: 2018/08/11 12:46:25
categories:
- Notes
tags:
- Mac
- Python
- Environment
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs84jxu86j20jg0aydq3.jpg
---

> 前言：毕业后正式入职公司，公司给发了新电脑Mac，为了在平时空余时间可以学习一下大热的Python语言，虽然自己的Mac已经安装过Python开发环境了，但是为了省去来回切换机器的麻烦，因此准备从头安装Python开发环境，顺便记录一下安装过程和踩过的坑，方便大家。

### 0x00 前言

Mac已经默认为用户安装内置了Python2.7.10，但是现在Python的版本已经到了Python3，所以我们为了开发时体验到Python最新的版本特性，最好安装Python3。但是直接升级系统默认的Python环境，升级步骤复杂不说，而且可能会因为操作不当导致某些问题，所以为了保险起见，我建议额外安装一个Python3环境，与原有的共存。

### 0x01 安装Homebrew

Homebrew是Mac上一款优秀的包管理工具，具体介绍见官网：[Homebrew](https://brew.sh/)

安装很简单，在终端输入如下命令即可：

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

替换Homebrew源，加快安装包的速度，具体步骤这里就不叙述了，详情可以参考：[更换Homebrew的更新源](https://blog.csdn.net/u010275932/article/details/76080833)

### 0x02 安装Python3

我们使用前面安装的Homebrew来安装最新的Python

使用Homebrew安装，在终端输入如下命令：

```bash
brew install python
```

检测Python3是否安装成功：

```bash
python3 -V
```

如显示版本号则表示安装成功

### 0x03 替换pip3源

pip是Python中非常方便易用的安装包管理器，但是在实际下载安装包的时候总是连接不上或者下载速度特别慢，pypi.python.org就是其中一个。

所以，使用pip给Python安装软件时，经常出现Timeout连接超时错误。修改pip连接的软件库可以解决这个问题。

> 注意：pip是对应Python2，最新的Python3对应的则是pip3。不管你用的是pip3还是pip，方法都是一样的。

新建.pip文件夹

```bash
cd ~
mkdir .pip
```

编辑pip.conf配置文件

```bash
sudo vi pip.conf
```

按`i`切换成insert模式，输入以下内容：

```bash
[global]  
  
timeout = 6000  
  
index-url = http://pypi.douban.com/simple/  
  
[install]  
  
use-mirrors = true  
  
mirrors = http://pypi.douban.com/simple/  
  
trusted-host = pypi.douban.com
```

依次按下`esc`，`：`，`w` `q`，退出并保存文件。

### 0x04 使用

可以在终端中使用Python3命令来运行你编写的python代码啦。

```bash
python3 xxx.py
```

你还可以在终端中使用pip3命令来安装你需要的python包。

```bash
pip3 install xxx
```

### 0x05 结束语

到此为止，一个最新、快速、简单的Python3开发环境就基本安装配置好了，现在你可以使用编辑器新建一个py后缀文件，编写你的Python代码，然后使用Python3命令来运行。如果代码中需要用到某些Python包，你也可以pip3命令来安装。

后续更多精彩内容，敬请期待！