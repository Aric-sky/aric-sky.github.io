layout: post
title: es6中export和export default的作用、区别
comment: true
tags: [前端,JS]
date: 2018-02-06 20:26:18
updated: 2018-02-06 20:26:18
---

------
<!-- more -->

## 作用：
export和export default实现的功能相同，即：可用于导出（暴露）常量、函数、文件、模块等，以便其他文件调用。

## 区别：

1. export导出多个对象，export default只能导出一个对象
2. export导出对象需要用{ }，export default不需要{ }，如：
```bash
　　export {A,B,C};

　　export default A;

```
3. 在其他文件引用export default导出的对象时不一定使用导出时的名字。因为这种方式实际上是将该导出对象设置为默认导出对象，如：

- 假设文件A、B在同级目录，实现文件B引入文件A的导出对象deObject：

```bash
　　　文件A：export default deObject

　　　文件B：import deObject from './A'

　　　或者：

　　　import newDeObject from './A'
```