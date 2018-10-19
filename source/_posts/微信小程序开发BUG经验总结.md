layout: post
title: 微信小程序开发BUG经验总结
comment: true
tags: [前端,微信小程序]
date: 2018-10-19 16:14:44
updated: 2018-10-19 16:14:44
---

------
<!-- more -->

## 1、图片本地资源名称，尽量使用小写命名
- 在引入头部头像图片的时，图片格式的后缀写成了PNG，图片无法显示；后来改成小写之后正常显示了。
```bash
<image class="avatar" src="../../images/avatar.png"></image>

```

## 2、wx.getSystemInfoSync获取windowHeight不准确
- 主要原因在于获取是时机，wx.getSystemInfoSync是在页面初始化的时候就计算了，基本上可以理解为是屏幕高度。所以，最好的方法是使用异步接口，并且在onReady函数中调用。
```bash
  onReady() {
    wx.getSystemInfo({
      success({windowHeight}) {
        // todo
      }
    });
  }
```

## 3、无法获取UnionID的问题

> login获取UID必须满足两个条件：

- 把小程序和公众号都绑定在开放平台；
- 用户必须已经关注公众号。

> 用wx.getUserInfo获取满足一个条件：

- 把小程序和公众号都绑定在开放平台；
```bash
<image class="avatar" src="../../images/avatar.png"></image>

```

### 4、new Date跨平台兼容性问题
```bash
在Andriod使用new Date(“2018-05-30 00:00:00”)木有问题，但是在ios下面识别不出来。
因为IOS下面不能识别这种格式，需要用2018/05/30 00:00:00格式。可以使用正则表达式对做字符串替换，将短横替换为斜杠。var iosDate= date.replace(/-/g, '/');。
```

