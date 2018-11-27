layout: post
title: web容错处理
comment: true
tags: [前端]
date: 2018-11-22 16:23:53
updated: 2018-11-22 16:23:53
---

------
<!-- more -->

## 图片容错

img标签当图片不存在时将触发onerror事件，提供几个方案。
1.常规写法
```bash
// js
function imgError(image) {
    image.onerror = "";
    image.src = "/images/noimage.gif";
    return true;
}
// html
<img src="image.png" onerror="imgError(this);"/>
```
2.内联式:

```bash
<img src="image.png" onerror="this.onerror=null;this.src='/images/noimage.gif';" />
```

3.使用jQuery:
```bash
$("img").error(function () {
  $(this).unbind("error").attr("src", "broken.gif");
})
```

4.动态生成图片并容错的写法
```bash
function addImg(isrc) {
  var Img = new Image();
  Img.src = isrc;
  Img.onload = function () {
      document.body.appendChild(Img);
  }
  Img.onerror = function () {
      console.log("error");
  }
}
```




