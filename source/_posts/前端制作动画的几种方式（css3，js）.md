layout: post
title: 前端制作动画的几种方式（css3，js）
comment: true
tags: [css,js,前端]
date: 2018-11-27 16:43:25
updated: 2018-11-27 16:43:25
---

------
<!-- more -->

## 1.css的transition

transition: property duration timing-function delay;

> property:填写需要变化的css属性如：width,line-height,font-size,color等;
duration:完成过渡效果需要的时间（2s 或者2000ms）
timing-function:完成效果的速度曲线（linear,ease,ease-in,ease-out等等）
timing-delay:动画效果的延迟触发时间（2s 或者2000ms）。

默认值分别为：all 0 ease 0 

```bash
<style type="text/css">
  div {
    width:100px;
    height:100px;
    background:red;
    transition:width 2s;
  } 
  div:hover {
    width:300px;
  }
</style>
<div></div>

```

## 2.css3的animation属性

animation: name duration timing-function delay iteration-count direction;

> name:keyframe的名称，也就是定义了关键帧的动画的名称,这个名称用来区别不同的动画。
duration:完成动画所需要的时间（2s 或者 2000ms）
timing-function:完成动画的速度曲线
delay：动画开始之前的延迟
iteration-count：动画播放次数
direction：是否轮流反向播放动画（normal:正常顺序播放，alternate下一次反向播放）如果把动画设置为只播放一次，则该属性没有效果。

```bash
  <style>
    div {
      width:100px;
      height:100px;
      background:red;
      position:relative;
      animation:mymove 5s infinite;
      -webkit-animation:mymove 5s infinite; /*Safari and Chrome*/
    }
    
    @keyframes mymove {
      1% {left:0px;}
      20%{left:200px;}
      50% {left:300px;}
      100%{left:200px;}
    }
    
    @-webkit-keyframes mymove { /*Safari and Chrome*/
      1% {left:0px;}
      20%{left:200px;}
      50% {left:300px;}
      100%{left:200px;}
    }
  </style>

  <div></div>

```

## 3.Jquery的animate函数

$(selector).animate(styles,options)

```bash
$(myElement).animate({height:'300px',opacity:'0.4'},"slow");

```

## 4.原生js动画

```bash
<div id="odiv" class="odiv">
     <div id="sdiv" class="sdiv">
     </div>
</div>
 
<script language="javascript">
window.onload = function(){
     var odiv = document.getElementById('odiv');
     odiv.onmouseover = function(){
      startMover(0);
 }
 odiv.onmouseout = function(){
      startMover(-200);
 }
}
var timer = null;
function startMover(a){//速度和目标值
     clearInterval(timer);//执行当前动画同时清除之前的动画
     var odiv = document.getElementById('odiv');
 timer = setInterval(function(){
     var speed = (a-odiv.offsetLeft)/10;//缓冲动画的速度参数变化值
 //如果速度是大于0，说明是向右走，那么就向上取整
     speed = speed>0?Math.ceil(speed):Math.floor(speed);
 //Math.floor();向下取整
     if(odiv.offsetLeft == a){
      clearInterval(timer);
     }
 else{
      odiv.style.left = odiv.offsetLeft+speed+'px';
  }
 },30);
}
</script>

```

## 5.插件

网上可以搜到很多封装好的动画插件，这些插件可以直接引入到页面中，通过初试话和配置的方式进行设定，直接在页面中展示动画。

如：waves，textillate.js等等。

 

## 6.使用canvas制作动画

我们还可以使用canvas在浏览器上画图，并且利用其api，制作动画。canvas使用的地方非常多，尤其是在制作h5小游戏上。

同样都是使用编码的方式由前端开发工程师完成动画效果，canvas要比原生js效率高的多，流畅的多。通过画笔的方式，能够轻松的实现更多的动画效果。

至于canvas如何使用，请看我博客中正在连载的教程--html5 canvas常用api总结。

 

## 7.引用gif图片

如果在需求特别紧急，而且动画又特别复杂的情况下，自己没有把握按时实现效果，或者代价太大，真的，别犹豫，上gif图片吧，不要在技术上纠结了，虽然在工程师的角度上这样做很low，但是，用户的体验是没有影响的~所以，别纠结，就是要快！完成最重要了！

