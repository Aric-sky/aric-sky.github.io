layout: post
title: call/bind/apply 的区别
comment: true
tags: [前端,js]
date: 2018-02-08 15:05:16
updated: 2018-02-08 15:05:16
---

------
<!-- more -->

```bash
var name = '小刚'
var person = {
    name: '小明',
    fn: function() {
        console.log(this.name + '撸代码')
    }
}
person.fn() // => 小明撸代码
// 如何把它变成  “小刚撸代码”  呢？

// 我们可以用 call/bind/apply 分别来实现
person.fn.call(window) // => 小刚撸代码
person.fn.apply(window) // => 小刚撸代码
person.fn.bind(window)() // => 小刚撸代码

```

显而易见，call 和 apply 更加类似，bind与两者形式不同
那 call 和 apply 的区别在哪呢?

```bash
obj.call(thisObj, arg1, arg2, ...)
obj.apply(thisObj, [arg1, arg2, ...])
// 通过上面的参数我们可以看出， 它们之间的区别是apply接受的是数组参数，call接受的是连续参数。
// 于是我们修改上面的函数来验证它们的区别

var person = {
    name: '小明',
    fn: function(a,b) {
        if ({}.toString.call(a).slice(8, -1) === 'Array') {
            console.log(this.name+','+a.toString()+'撸代码')
        }else{
            console.log(this.name+','+a+','+b+'撸代码')
        } 
    }
}

person.fn.call(this, '小红', '小黑' ) // => 小刚,小红,小黑撸代码
person.fn.apply(this, ['小李', '小谢']) // => 小刚,小李,小谢撸代码

```
那么bind 与call,apply有什么区别呢 ? 
与call和apply不同的是，bind绑定后不会立即执行。它只会将该函数的 this 指向确定好,然后返回该函数

```bash
var name = "小红"
var obj = {
    name: '小明',
    fn: function(){
        console.log('我是'+this.name)
    }
}
setTimeout(obj.fn, 1000) // => 我是小红
// 我们可以用bind方法打印出 "我是小明"
setTimeout(obj.fn.bind(obj), 1000) // => 我是小明
// 这个地方就不能用 call 或 apply 了, 不然我们把函数刚一方去就执行了

// 注意: bind()函数是在 ECMA-262 第五版才被加入
// 所以 你想兼容低版本的话 ,得需要自己实现 bind 函数
Function.prototype.bind = function (oThis) {
    if (typeof this !== "function") {
      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var aArgs = Array.prototype.slice.call(arguments, 1), 
        fToBind = this, 
        fNOP = function () {},
        fBound = function () {
          return fToBind.apply(
              this instanceof fNOP && oThis ? this : oThis || window,
              aArgs.concat(Array.prototype.slice.call(arguments))
          );
        };

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();

    return fBound;
};

```

