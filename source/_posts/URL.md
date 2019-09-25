layout: post
title: URL组成
comment: true
tags: [js]
date: 2019-05-30 21:59:18
updated: 2019-05-30 21:59:18
---

------

<!-- more -->


### 完整的URL由这几个部分构成：

```
    scheme://host:port/path?query#fragment
```

### 名词解释：

```
    scheme:通信协议.常用的http,https,ftp,maito等.
    host:主机(带端口号). 主机名或IP 地址。
    port:端口号,可选，省略时使用默认端口，如http的默认端口为80。
    path:路径:由零或多个"/"符号隔开的字符串，一般用来表示主机上的一个目录或文件地址。
    query:查询参数,可选，用于给动态网页传递参数，可有多个参数，用"&"符号隔开，每个参数的名和值用"="符号隔开。
    fragment:信息片断，字符串，用于指定网络资源中的片断。(也称为锚点.)
```

### 具体片段使用实例：

````
    const {
        href,   // 整个URl字符串
        protocol,   // 协议部分 http
        host,   // 主机(带端口号) www.home.com:8080
        port,   // 端口
        pathname,   // 路径部分(就是文件地址) /windows/location/page.html
        search,     // ?ver=1.0&id=timlq
        hash    // #love
    } = window.location
```

### 获取url参数数组

```
    var url = "http://127.0.0.1/e/action/ShowInfo.php?classid=9&id=2";

    /**
     * @desc 获取url参数
     * @param {String} _url  url路径
     */
    function parse_url(_url){
        var pattern = /(\w+)=(\w+)/ig;
        var parames = {};
        url.replace(pattern, function(a, b, c){
        parames[b] = c;
        });
        return parames;
    }
    var parames = parse_url(url);

```

### 输入参数名，输出参数值

```
    /**
     * @desc 获取url参数
     * @param {String} name  想要获取的参数名字
     */
    function GetQueryString(name){
         var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
         var r = window.location.search.substr(1).match(reg);
         if(r!=null)return  unescape(r[2]); return null;
    }

    // 调用方式
    var objectId = GetQueryString("objectId")

```

```
    /**
     *  @desc  获取url参数
     *  @param  {String} url为穿过来的链接
     *  @param  {String} id为参数名
     */
    function GetParam(url, id) {
        url = url+ "";
        regstr = "/(\\?|\\&)" + id + "=([^\\&]+)/";
        reg = eval(regstr);
        //eval可以将 regstr字符串转换为 正则表达式
        result = url.match(reg);
        if (result && result[2]) {
            return result[2];
        }
    }

    // 调用方式
    var objectId = GetQueryString(window.location.href,"objectId")

```

```
    /**
     * @desc 获取url参数
     * @param {String} paramName  想要获取的参数名字
     * @param {String} url   访问地址，可以为空：为空时默认为当前url
     */
    function getParameter(paramName, url) {
        var seachUrl = window.location.search.replace("?", "");
        if (url != null) {
            var index = url.indexOf('?');
            url = url.substr(index + 1);
            seachUrl = url;
        }
        var ss = seachUrl.split("&");
        var paramNameStr = "";
        var paramNameIndex = -1;
        for (var i = 0; i < ss.length; i++) {
            paramNameIndex = ss[i].indexOf("=");
            paramNameStr = ss[i].substring(0, paramNameIndex);
            if (paramNameStr == paramName) {
                var returnValue = ss[i].substring(paramNameIndex + 1, ss[i].length);
                if (typeof(returnValue) == "undefined") {
                    returnValue = "";
                }
                return returnValue;
            }
        }
        return "";
    }

    // 调用方式
    var objectId = getParameter("objectId")

```

