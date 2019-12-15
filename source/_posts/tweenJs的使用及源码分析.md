layout: post
title: tweenJs的使用及源码分析
comment: true
tags: [tweenJs,动画,前端]
date: 2019-12-15 10:35:05
updated: 2019-12-15 10:35:05
---

------
<!-- more -->

![xmind](https://img.alicdn.com/tfs/TB1OmQzp4D1gK0jSZFsXXbldVXa-658-319.jpg)


## tweenJs与css动画相比
### 优势
- 更加灵活(链式补间,)
- 可以定义多个动画，循环调用
- 应用场景更广阔

### 弊端
- 动画的的更新需要主动调用更新方法（依赖定时器或者动画主循环函数）
- 性能没css更优


## CSS3中transition和animation的属性


1) transition(过渡动画)
[示例](https://www.w3school.com.cn/tiy/t.asp?f=css3_transition)

- 用法：transition: property duration timing-function delay

|属性 |	含义描述 |
| --- | --- |
|transition-property |	指定哪个CSS属性需要应用到transition效果|
|transition-duration |	指定transition效果的持续时间|
|transition-timing-function	 | 指定transition效果的速度曲线|
|transition-delay	 | 指定transition效果的延迟时间|


2) animation(关键帧动画)
[示例](https://www.w3school.com.cn/tiy/t.asp?f=css3_animation)

- 用法：animation: name duration timing-function delay iteration-count direction fill-mode play-state

|属性 |	含义描述 |
| --- | --- |
|animation-name |	指定要绑定到选择器的关键帧的名称|
|animation-duration |	指定动画的持续时间|
|animation-timing-function |	指定动画的速度曲线|
|animation-delay |	指定动画的延迟时间|
|animation-iteration-count |	指定动画的播放次数|
|animation-direction |	指定是否应该轮流反向播放动画|
|animation-fill-mode |	规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式|
|animation-play-state |	指定动画是否正在运行或已暂停|





## tweenjs介绍


tweenjs 是使用 JavaScript 的一个简单的补间动画库，支持数字、对象的属性和 CSS 样式属性的赋值。

tweenjs 以 <mark>平滑</mark> 的方式修改元素的属性值，需要传递给 tween 要修改的值、动画结束时的最终值和动画花费时间（duration），之后 tween 引擎就可以计算从开始动画点到结束动画点之间值，从而产生平滑的动画效果。


## 示例

```
var box = document.createElement('div');
box.style.setProperty('background-color', '#008800');
box.style.setProperty('width', '100px');
box.style.setProperty('height', '100px');
document.body.appendChild(box);

function animate() {
    requestAnimationFrame(animate);
    TWEEN.update();
}
requestAnimationFrame(animate);

var coords = { x: 0, y: 0 };
var tween = new TWEEN.Tween(coords) 
    .to({ x: 300, y: 200 }, 1000) 
    .easing(TWEEN.Easing.Quadratic.Out) 
    .onUpdate(function() { 
      box.style.setProperty('transform','translate('+coords.x+'px,'+ coords.y+'px)');
    })
    .start(); 

```

[demo](https://wangyuanqi.com/tween/demo.html)

## 示例说明

1、 假设有一个对象 position ，它的坐标为 x 和 y

```
	var position = { x: 100, y: 0 }
```

2、 假设有一个对象 position ，它的坐标为 x 和 y

```
	var tween = new TWEEN.Tween(position)
	tween.to({x: 200}, 1000)
```

3、 创建 tween 对象后，激活它，从而让它开始动画

```
	tween.start();
```
4、 为了平滑的动画效果，需要在同一个循环动画中调用 TWEEN.update 方法

```
	animate();
	function animate(){
	    requestAnimationFrame(animate);
	    TWEEN.update();
	}
```

这个动作将会更新所有被激活的 tweens ，在 1s 内 position.x 将变为 200 。

5、 可以使用 onUpdate 回调函数将结果打印到控制台上

```
	tween.onUpdate(function(){
	    console.log( this.x );
	})
```
这个函数在每次 tween 被更新时都会被调用


## tweenjs 动画


Tween.js 本身不会运行，你需要通过 update 方法明确告诉它什么时候开始运行，推荐在动画主循环中使用该动画，可以调用 requestAnimationFrame 方法来获得良好的图像性能。

[深入理解 requestAnimationFrame](http://blog.wangyuanqi.com/2018/03/09/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3-requestAnimationFrame/)

使用无参数的调用方法，update 将明确当前时间。也可以为 update 方法法明确一个时间。

```
	TWEEN.update(100);
```

意思是"更新时间 = 100 毫秒"。你可以使用它来确保代码中的所有时间相关函数都使用相同的时间值。例如，假设你有一个播放器，并希望同步运行补间。

## 控制 tween 动画


| 方法名        | 功能           | 参数  |
| ------------- |:-------------:|:-----:|
| Tween.start | 控制动画开始 | time (延迟一段时间后触发；可选) |
| Tween.stop  | 控制动画结束  | - |
| TWEEN.update | 手动执行动画的更新   | time (更新动画位于time时间点；可选) |


- chain ==> 制作多个动画，例如一个动画在另一个动画结束后开始，可以通过 chain 来实现
[示例](http://wangyuanqi.com/tween/chain.html)

```
	tweenA.chain(tweenB);  //tweenB 在 tweenA 之后开始动画，故可以制作一个无线循环的动画
	tweenB.chain(tweenA);
```

- repeat ==> 制作循环动画，优于 chain，接收一个用于描述循环次数的参数

```
	tween.repeat(10);
	tween.repeat(infinity);
```

- delay ==> 用于控制动画之间的延迟

```
	tween.delay(1000);
	tween.start()
```

- to ==> 用于控制动画之间的延迟

```
/** Tween传入参数可以读取当前属性值并应用相对值来找出新的最终值 **/
	// 绝对值
	Tween(absoluteObj).to({ x: 100 });
	// 相对值
	Tween(relativeObj).to({ x: "+100" });
	// 数组
	Tween(relativeObj).to({ x: [0, -100, 100] });
```

## 回调函数

可以在每次 tween 循环周期的指定时间点调用自定义的函数

- onStart ==> tween 动画开始前的回调函数
- onStop ==> tween 动画结束后的回调函数
- onUpdate ==> 在 tween 动画每次更新后执行
- onComplete ==> 在 tween 动画全部结束后执行

```
	var tween = new TWEEN.Tween({
	
	}).to({
	
	}).onStart(function(){
	
	}).onUpdate(function(){
	
	})
```


## easing函数

[函数示例](https://wangyuanqi.com/tween/graph.html)

| 函数名        | 效果           |
| ------------- |-------------|
| Linear | 线性匀速运动效果 |
| Quadratic  | 二次方的缓动（t^2）  |
| Cubic | 三次方的缓动  |
| Quartic | 四次方的缓动 |
| Sinusoidal | 正弦曲线的缓动|
| Exponential | 指数曲线的缓动 |
| Circular | 圆形曲线的缓动  |
| Elastic | 指数衰减的正弦曲线缓动  |
| Back | 超过范围的三次方的缓动  |
| Bounce | 指数衰减的反弹缓动  |


## easing类型

- easeIn（In） ==> 加速，先慢后快
- easeOut（Out） ==> 减速，先快后慢
- easeInOut（InOut） ==> 前半段加速，后半段减速

## 使用公式

```
	.easing(TWEEN.Easing.easing函数.easing类型)
```

## tweenjs 源码分析

[源码地址](https://wangyuanqi.com/tween/js/tween.js)

![控制方法](https://img.alicdn.com/tfs/TB1eqebqi_1gK0jSZFqXXcpaXXa-613-815.jpg)

![缓动函数](https://img.alicdn.com/tfs/TB1MZJ.qeL2gK0jSZPhXXahvXXa-614-545.jpg)

![插值函数](https://img.alicdn.com/tfs/TB1S71fqlr0gK0jSZFnXXbRRXXa-847-671.jpg)

## 应用场景总结

|动画类型|应用场景|
| ---- | ---- |
|CSS动画或过度动画|动画需求非常简单|
|Tween.js|动画需要涉及复杂的布局，如：需要将多个补间同步到一起，在完成一些动作之后，循环多次等等|


使用它们来计算平滑曲线作为输入数据


## tip

保持你的onUpdate回调非常轻量级,因为这个函数每秒钟会被调用很多次，所以如果每次更新都要花费很多的代价，那么你可能会阻塞主线程并导致可怕的结果。
