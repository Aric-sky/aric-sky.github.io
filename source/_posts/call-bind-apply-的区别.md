layout: post
title: call/bind/apply 的区别
comment: true
tags: [前端,js]
date: 2018-02-08 15:05:16
updated: 2018-02-08 15:05:16
---

------
<!-- more -->

- 利用call和apply来实现,this就是call和apply对应的第一个参数,如果不传值或者第一个值为null,undefined时this指向window
```bash
function foo() {
   console.log(this);
}
foo.apply('我是apply改变的this值');//我是apply改变的this值
foo.call('我是call改变的this值');//我是call改变的this值
```
### call和apply定义
调用方法,用一个对象替换掉另一个对象(this)
对象.call(新this对象,实参1,实参2,实参3.....)
对象.apply(新this对象,[实参1,实参2,实参3.....])
### call和apply用法
1.间接调用函数,改变作用域的this值
2.劫持其他对象的方法
```bash
var foo = {
  name:"张三",
  logName:function(){
    console.log(this.name);
  }
}
var bar={
  name:"李四"
};
foo.logName.call(bar);//李四
实质是call改变了foo的this指向为bar,并调用该函数
```
3.两个函数实现继承
```bash
function Animal(name){   
  this.name = name;   
  this.showName = function(){   
    console.log(this.name);   
  }   
}   
function Cat(name){  
  Animal.call(this, name);  
}    
var cat = new Cat("Black Cat");   
cat.showName(); //Black Cat
```
4.为类数组(arguments和nodeList)添加数组方法push,pop
```bash
(function(){
  Array.prototype.push.call(arguments,'王五');
  console.log(arguments);//['张三','李四','王五']
})('张三','李四')
```
5.合并数组
```bash
let arr1=[1,2,3]; 
let arr2=[4,5,6]; 
Array.prototype.push.apply(arr1,arr2); //将arr2合并到了arr1中
```
6.求数组最大值
```bash
Math.max.apply(null,arr)
```
7.判断字符类型
```bash
Object.prototype.toString.call({})
```
### bind
bind是function的一个函数扩展方法，bind以后代码重新绑定了func内部的this指向,不会调用方法,不兼容IE8
```bash
var name = '李四'
 var foo = {
   name: "张三",
   logName: function(age) {
   console.log(this.name, age);
   }
 }
 var fooNew = foo.logName;
 var fooNewBind = foo.logName.bind(foo);
 fooNew(10)//李四,10
 fooNewBind(11)//张三,11  因为bind改变了fooNewBind里面的this指向
```

---

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

