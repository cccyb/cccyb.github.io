---
title: 阿里云部署项目笔记——Vue篇
date: 2017/04/02 12:46:25
categories:
- Notes
tags:
- Server
- Vue
thumb: https://ws1.sinaimg.cn/large/c542ee77ly1fzs86cvv93j20jg0cztlz.jpg
---

上个月13号弄到了阿里云9.9的学生主机，而且前段时间刚好又完成了练手项目Vue版知乎日报，但是生成的dist文件在Github Pages直接预览会有跨域问题，于是想着能不能使用手上的服务器解决一下这个问题。但是因为种种原因，一直拖着没有去实践，于是趁着清明小长假，花了一点时间折腾了一番，将项目部署至了服务器上，实现了在线预览功能，也了解了在服务器项目部署的大致流程。

## 前期准备

首先你需要有一台阿里云服务器（9.9学生ECS美滋滋，可惜过几个月就没有了，扎心了！）

付费时选择了Windows系统，可以通过阿里云的控制台在线控制，略卡，然后尝试使用Mac下的远程控制软件操作，不知为何失败，故选择重装`CentOS`（`Linux`大法好！锻炼动手能力的时候到了），具体步骤自行搜索。

然后在终端使用`ssh username@ip`命令远程连接服务器，然后输入密码，**注意，这里输入密码时终端是看不到的，输完直接回车即可。**

- 安装`Nginx`：`yum -y install nginx`
- 开启`Nginx`：`nginx -s start`
- 修改`Nginx`配置文件：`vi /etc/nginx/nginx.conf`
- 修改配置文件后重启`Nginx`：`nginx -s reload`
- 直接重启`Nginx`：`nginx -s restart`

至此，在浏览器中输入服务器的公网ip即可访问`Nginx`默认的页面，服务器已准备好。

## 生成部署项目文件

在开发环境下，修改`config/index.js`里的`proxyTable`，解决跨域问题：

```javascript
proxyTable: {
  '/api': {
    target: 'http://news-at.zhihu.com',
    changeOrigin: true,
    pathRewrite: {
      '^/api': '/api/4'
    }
  }
}
```

组件请求方式：

```javascript
axios.get('api/news/latest')
  .then(response => {
    // todo	
  })
  .catch(error => {
    console.log(error);
  });
```

这里设置后请求`api/xxx`将会代理成`http://news-at.zhihu.com/api/4/xxx`。

**注：这里设置这种形式是为了与后面部署的Nginx配置文件相匹配。**

然后，执行`npm run build`生成静态文件夹`dist`。

## 部署项目至服务器

将前面生成的`dist`文件夹传输到服务器。我这里使用了`scp`命令。

- 具体为：`scp -r local_dir username@servername:remote_dir`，把当前目录的`local_dir`目录上传到服务器的`remote_dir`目录。
- 例如：`scp -r test codinglog@192.168.0.101:/var/www/`，把当前目录下的`test`目录上传到服务器的`/var/www/` 目录。具体使用方法可以自行搜索。
- 也可以使用可视化工具，比如`FileZilla`之类…

然后登录服务器，找到并打开`Nginx`的配置文件：`vi /etc/nginx/nginx.conf`

添加新server：

```
server {
        listen       8888;
        server_name  localhost;
        location / {
            root /usr/local/var/www/zhihu-daily/;
            index index.html;
        }
        location ^~ /api/ {
                proxy_pass http://news-at.zhihu.com/api/4/;
                proxy_set_header Host news-at.zhihu.com;
        }
    }
```

- `root`：填写刚刚传输至服务器的静态文件夹路径，这里我将`dist`修改为对应项目名
- `index`：首页文件名
- location：转发代理配置
  - `proxy_pass`：转发地址
  - `proxy_set_header Host`：转发主机名

这样配置完`Nginx`后即可通过请求`http://your_ip/api/xxx`的方式请求数据，同时与前面开发环境下的请求模式匹配，部署时只是改变ip与端口号，后面的匹配规则则不需要改变，故无需生产环境无需修改请求代码。

修改后保存，然后执行命令`nginx -s reload`重启`Nginx`即可。

然后直接在浏览器中输入服务器的ip+端口号即可预览部署好的项目。至此，部署完成！！！