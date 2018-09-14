layout: post
title: CentOS 7 yum 安装 Nginx
comment: true
tags: [后端,nginx]
date: 2018-09-14 15:00:11
updated: 2018-09-14 15:00:11
---

------
<!-- more -->

> 1.添加Nginx到YUM源

添加CentOS 7 Nginx yum资源库,打开终端,使用以下命令:
```
  sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

```

> 2.安装Nginx

在你的CentOS 7 服务器中使用yum命令从Nginx源服务器中获取来安装Nginx：
```
sudo yum install -y nginx

```

> 3.启动Nginx

刚安装的Nginx不会自行启动。运行Nginx:
```
  sudo systemctl start nginx.service

```
这时就可以通过ip正常访问默认页面了。

> 更多Nginx配置信息

```
//列出nginx的安装文件，快速定位配置文件目录
rpm -ql nginx 

//网站文件存放默认目录
/usr/share/nginx/html

//网站默认站点配置
/etc/nginx/conf.d/default.conf

//自定义Nginx站点配置文件存放目录
/etc/nginx/conf.d/

//Nginx全局配置
/etc/nginx/nginx.conf

//Nginx启动
nginx -c nginx.conf

//Linux查看公网IP
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'


```

### QUESTIONS
> Nginx: [error] open() ＂/usr/local/Nginx/logs/Nginx.pid

```
  /usr/sbin/nginx -c /etc/nginx/nginx.conf
```
