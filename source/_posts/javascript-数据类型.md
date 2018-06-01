layout: post
title: javascript 数据类型
comment: true
tags: [js]
date: 2018-05-23 11:32:48
updated: 2018-05-23 11:32:48
---

------
<!-- more -->
## 一、六种数据类型
数据类型|解释
-|-
Object(引用类型)|包含:Function,Array,Date...
number(原始类型)|表示数字，例如： 42 或者 3.14159。
string (原始类型)|表示字符串，例如："Howdy"
boolean(原始类型)|布尔值，true 和 false
null (原始类型)|一个表明 null 值的特殊关键字。 JavaScript 是大小写敏感的，因此 null 与 Null、NULL或其他变量完全不同。
undefined(原始类型)| 变量未定义时的属性。
Symbol(原始类型)|( 在 ECMAScript 6 中新添加的类型).。一种数据类型，它的实例是唯一且不可改变的。

> 原始类型（基本类型）：按值访问，可以操作保存在变量中实际的值。原始类型汇总中null和undefined比较特殊。
> 引用类型：引用类型的值是保存在内存中的对象。
* 与其他语言不同的是，JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。所以引用类型的值是按引用访问的。

## 二、隐式转换
### 1、+和-
巧用+和-规则转换类型
把变量转换成数字：num-0;
把变量转换成字符串：num+'';
### 2、a==b
```bash
  "1.23" == 1.23
  0 == false
  null == undefined
  new Object() == new Object()
  [1,2] == [1,2]
```
类型相同，同===
类型不同，尝试类型转换和比较：
```bash
  null == undefined 相等
  number == string 转 number 1 == "1.0" //true
  boolean == ? 转 number 1 == true //true
  object == number | string 尝试对象转为基本类型 new String('hi') == 'hi' // true
  其它： false
```
## 三、包装对象
### 一、原始类型没有属性和方法
1.按原始类型和引用类型的定义来说，只有引用类型（对象）才有属性和方法，原始类型是没有属性和方法的。
2.但是我们也能经常看到有下面这样的写法。
```bash
var num = 100
var str = num.toString()
console.log(typeof str) // string
```
3.我们使用 toString() 方法，将 num 这个数值类型转换成了字符串类型，如此我们用 原始类型 num 调用了 toString() 方法，那么是不是原始类型也能调用方法呢？答案是否定的。仍然只有对象才能拥有属性和方法。

### 二、包装对象的概念
1.在JavaScript中，“一切皆对象”，包括三种原始类型的值(数值、字符串、布尔值)，在一定条件下，也会自动转为对象，也就是原始类型的“包装对象”。
2.包装对象是特殊的引用类型。每当读取数字、字符串和布尔值的属性或方法时，创建的 临时对象 称做包装对象。

### 三、小结
1.这三个包装对象作为 构造函数 使用（带有 new）时，可以将 原始类型的值转为对象；
2.作为 普通函数 使用时（不带有 new），可以将任意类型的值，转为原始类型的值。

### 四、包装对象的销毁
1.【注意】一旦包装对象的属性或方法的引用结束，这个新创建的对象就会销毁。
```bash
var str = 'hello'
str.name = 'jack'
console.log(str.name) // undefined
```
2.【说明】在上面的例子中，代码第二行 name 属性赋值时，包装对象就会登场，创建一个 str 对应的临时对象，当然，这行代码执行完成，这个对象也就被销毁。然后在第三行则会创建一个新的包装对象，这个对象当然没有 name 属性，所以输出的是 undefined。

> 把一个基本类型尝试用对象的方式使用它的时候，比如访问length属性，或者增加一些属性的操作时，javascript会把这些基本类型转化为对应的包装类型对象。完成这样一个访问比如a.length返回以后或者a.t设置了以后，这个临时对象会被销毁掉。所以a.t赋值3了以后，再去输出a.t值是undefined。

## 四、类型检测
javascript中类型检测方法有很多：
- typeof
- instanceof
- Object.prototype.toString
- constructor
- duck type

### 1、typeof
最常见的就是typeof:
```bash
  typeof 100  // 'number'
  typeof true // 'boolean'
  typeof function // 'function'
  typeof(undefined) // 'undefined'
  typeof new Object() // 'object'
  typeof [1,2]  // 'object'
  typeof NaN  // 'number'
  typeof null // 'object'
```
> 比较特殊的是typeof null返回“object”。
历史原因，规范尝试修改typeof null返回“null”修改完大量网站无法访问，为了兼容，或者说历史原因返回"object"。
typeof对基本类型和函数对象很方便，但是其他类型就没办法了。
判断一个对象是不是数组？用typeof返回“object”。对对象的判断常用instanceof。

### 2、instanceof
基于原型链操作。obj instanceof Object。
左操作数为对象，不是就返回false,右操作数必须是函数对象或者函数构造器，不是就返回typeError异常。
原理：判断左边的左操作数的对象的原型链上是否有右边这个构造函数的prototype属性。

---
instanceof在判断对象是不是数组，Data，正则等时很好用。
instanceof坑：不同window或iframe之间的对象类型检测不能使用instanceof！

---

### 3、Object.prototype.toString
```bash
  Object.prototype.toString.apply(null)
  // "[object Null]"
  Object.prototype.toString.apply([])
  // "[object Array]"
  Object.prototype.toString.apply(function(){})
  // "[object Function]"
  Object.prototype.toString.apply(undefined)
  // "[object Undefined]"
```
需要注意的是IE6/7/8中 Object.prototype.toString.apply(null)返回“[object Object]”。

### 4、constructor
```bash
  Student.prototype.constructor === Student
  // true
```
任何对象都有constructor属性，继承自原型的，constructor会指向构造这个对象的构造器或者构造函数。
constructor可以被改写，所以使用要小心。

### 5、duck type
比如不知道一个对象是不是数组，可以判断它的length是不是数字，它是不是有join,push这样一些数组的方法。通过一些特征判断对象是否属于某些类型，这个有时候也常用。
![codes](http://p8dyokgbm.bkt.clouddn.com/315302-20170206095340447-2024526735.png)

fuc | 类型检测小结
-|-
Typeof | 适合基本类型及function检测，遇到null失效。
[[Class]] | 通过{}.toString拿到，适合内置对象和基元类型，遇到null和undefined失效(IE678等返回[object Object])。
Instanceof | 适合自定义对象，也可以用来检测原生对象，在不同iframe和window间检测时失效。

### 6、如何检测一个变量是字符串
有另外一种方法：将变量和空字符拼接后再和原来变量做全等判断
```bash
  var str="hello";
  var temp=str+'';
  temp===str
  //true
```
