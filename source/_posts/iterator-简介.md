layout: post
title: iterator 简介
comment: true
tags: []
date: 2018-03-27 09:50:40
updated: 2018-03-27 09:50:40
---

------
<!-- more -->

1、遍历器（Iterator）是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

2、Iterator的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。

3、在ES6中，有些数据结构原生具备Iterator接口（比如数组），即不用任何处理，就可以被for...of循环遍历，有些就不行（比如对象）。原因在于，这些数据结构原生部署了Symbol.iterator属性（详见下文），另外一些数据结构没有。凡是部署了Symbol.iterator属性的数据结构，就称为部署了遍历器接口。调用这个接口，就会返回一个遍历器对象。

4、在ES6中，有三类数据结构原生具备Iterator接口：数组、某些类似数组的对象、Set和Map结构。

5、一个为对象添加Iterator接口的例子。
```bash
let obj = {
  data: [ 'hello', 'world' ],
  [Symbol.iterator]() {
    const self = this;
    let index = 0;
    return {
      next() {
        if (index < self.data.length) {
          return {
            value: self.data[index++],
            done: false
          };
        } else {
          return { value: undefined, done: true };
        }
      }
    };
  }
};

```

6、下面是类似数组的对象调用数组的Symbol.iterator方法的例子。
```bash
let iterable = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3,
  [Symbol.iterator]: Array.prototype[Symbol.iterator]
};
for (let item of iterable) {
  console.log(item); // 'a', 'b', 'c'
}

```
注意，普通对象部署数组的Symbol.iterator方法，并无效果。

7、有一些场合会默认调用Iterator接口（即Symbol.iterator方法），除了下文会介绍的for...of循环，还有几个别的场合。

- 解构赋值
- 扩展运算符(...)
- yield*_yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。
- 由于数组的遍历会调用遍历器接口，所以任何接受数组作为参数的场合，其实都调用。

8、字符串是一个类似数组的对象，也原生具有Iterator接口。

9、遍历器对象除了具有next方法，还可以具有return方法和throw方法。如果你自己写遍历器对象生成函数，那么next方法是必须部署的，return方法和throw方法是否部署是可选的。


