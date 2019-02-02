---
title: Mac下MySQL5.7连接Navicat中文乱码解决方案
date: 2016/08/08 12:46:25
categories:
- Notes
tags:
- Mac
- MySQL
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs84zsrspj20jg0a0n5l.jpg
---
> 前几天购入mbp不久，准备安装mysql和其他web服务进行web程序开发，结果安装了mysql后运行了以前的一个web程序，在网页里中文显示乱码，但是在navicat里显示中文确实正常的，于是，踩坑之旅由此开始。。。

## 分析原因

首先，以前在Windows 10系统下，使用的`MySQL5.5`，`Navicat`导入`sql`文件后，程序运行没有问题，网页现在中文显示正常，于是可以确定程序源码和数据库sql文件没有问题，于是猜测问题可能出在IDE或MySQL上。

## 寻找IDE MyEclipse问题

- jsp页面

  确认jsp页面头部已有

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" import="java.util.*" pageEncoding="UTF-8"%>
```

<meta>层已包含

```java
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
```

- jdbc dao层
  确保数据库连接

```xml
String url = "jdbc:mysql://localhost:3306/exampleName?Unicode=true&characterEncoding=UTF-8"
```

里包含Unicode=true&characterEncoding=UTF-8

保证以上两点都已修改完成后发现乱码问题还是存在，于是在service层调用dao层代码查询数据库数据后在console里输出，发现输出的乱码与网页上的一致，于是判断乱码问题出在MySQL，与IDE无关。

## 寻找MySQL问题

百度发现网上解决办法已经非常详细，找到一个个人感觉比较详细的解决方案 <http://www.ha97.com/5359.html> 链接里由于是在`Linux系统`，与`mac` 的os系统大同小异，所以只需改动少部分就行，其中第二步中的`my.cnf`文件在`MySQL5.7`下是没有的，需要在`/usr/local/mysql/support-files` 中找到`my-default.cnf`（而在`MySQL5.5/5.6`下在`/usr/local/mysql/support-files` 中任意找一个`.cnf`文件）复制到`/etc` 文件夹下，改成`my.cnf`，然后进行修改。

但是问题来了，修改完`my.cnf`文件，字符集全部变成`utf8`后！字符集全部变成`utf8`后！字符集全部变成`utf8`后！（重要的事情讲三遍）发现还是中文还是乱码。

于是在终端打开`mysql`，进行测试，执行查询结果中文还是乱码，百思不得其解。由于前面都是直接在`navicat`里直接建表，运行`sql`文件，突然想如果直接在终端里执行插入语句，是否能成功。说干就干，结果发现如果直接在终端里插入的中文数据在终端里查询显示正常，到`navicat`里查看结果是？？？后面直接在终端里运行`sql`文件，导入数据，最终发现在网页里中文显示已经正常，终端查询中文也正常。

最终发现是`navicat`出现了问题

## 寻找navicat问题

百度一波发现有说`mac`下`navicat`新建连接是编码选择`auto`而不是`utf`8即可，一试，果然可以，但是还是不清楚为什么`mac`下的`navicat`跟`Windows`下不一样，这个问题是比较奇葩。

至此，乱码问题已解决。