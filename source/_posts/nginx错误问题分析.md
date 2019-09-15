layout: post
title: nginx错误问题分析
comment: true
tags: [nginx,后端]
date: 2019-09-15 11:23:04
updated: 2019-09-15 11:23:04
---

------
<!-- more -->

## nginx启动服务提示98: Address already in use错误

1. 首先用lsof －i :80查看80端口被什么程序占用；
2. kill 掉所有的nginx进程
3. 重启nginx


## nginx: [error] open() "/usr/local/nginx/logs/nginx.pid" failed (2: No such file or directory)

```
　　[root@localhost nginx]# nginx -c /etc/nginx/nginx.conf
　　使用nginx -c的参数指定nginx.conf文件的位置
　　[root@localhost nginx]# cd logs/
　　[root@localhost logs]# ll
　　总用量 12
　　-rw-r--r-- 1 root root 1246 12月 9 18:10 access.log
　　-rw-r--r-- 1 root root 516 12月 10 15:39 error.log
　　-rw-r--r-- 1 root root 5 12月 10 15:38 nginx.pid
　　看nginx.pid文件已经有了。
```

