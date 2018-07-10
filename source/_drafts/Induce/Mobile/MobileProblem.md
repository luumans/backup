title: 移动端浏览器兼容问题汇总
date: 2015-12-25 18:29:00
description: 
categories:
- Mobile
tags:
- Mobile
toc: true
author:
comments:
original:
permalink: 
---
　　**微信网页开发：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
[]()




深蓝的天空  
深蓝的天空
一只切图喵的日常
移动端浏览器兼容问题汇总
 
深蓝
2018/01/23
 开发相关
大概两年前做过一年微信宣传页（也就是俗称的 H5 ）开发，期间遇到一系列 Bug，现总结成文，由于时间较久远，能回忆起的不多了，现拼凑记忆记录如下，防止遗忘。


微信内置浏览器相关
去除上滑网址提示
微信内置浏览器，默认上滑行为为展示该页面的地址，但在一些微信宣传页面内容易出戏，所以需要阻止该行为。

该方法可能影响其他功能,比如页面滚动，所以需要在不使用的时候去除事件的默认行为的阻止。

function stopPageMove (event) {
    event.preventDefault();
    event.stopPropagation();
}
document.addEventListener('touchmove', stopPageMove, false);
//在不需要的时候，需要取消掉对`touchmove`的事件的默认行为的阻止。
document.removeEventListener('touchmove', stopPageMove);
浏览器表单相关
输输入法遮盖输入框
需要选择合适的页面布局，在有表单的情况下，尽量使用static布局，可以避免一些怪异的遮挡情况。

媒体相关
音乐不能自动播放
iOS 端 Safari 出于节约用户数据流量的考虑不允许自动播放 Audio，后来 Android 也跟进执行了该规则。

In Safari on iOS (for all devices, including iPad), where the user may be on a cellular network and be charged per data unit, preload and autoplay are disabled. No data is loaded until the user initiates it. This means the JavaScript play() and load() methods are also inactive until the user initiates playback, unless the play() or load() method is triggered by user action. In other words, a user-initiated Play button works, but anonLoad="play()" event does not. via Safari HTML5 Audio and Video Guide

有些场景要求页面自动播放背景音乐，这就成问题了。根据 Apple 的解释，也就说必须由用户的动作触发才能播放音乐。解决方案是使用某个按钮或者其他元素并引导用户点击，例如 H5 宣传页面的点击开始。

// run on page load
var button = document.getElementById('button');
var audio = document.getElementById('audio');
 
var onClick = function() {
    audio.play(); // audio will load and then play
};
 
button.addEventListener('click', onClick, false);
至于在微信内置浏览器内无该问题，想必微信对此做了处理。

文字渲染
Font Boosting
web 移动端可能会出现这样的情况：浏览器中字体的实际显示大小，与在CSS中指定的大小不一致。这是由于Font Boosting 计算规则导致的。

.selector {
    max-height: 999999px;
}
或者

.selector{
    max-height: 100%;
}
iOS oveflow:scroll滑动卡顿
在手机页面上，如果对某个 div 或模块使用了 overflow:scroll; 属性，那么在手机上滑动这个模块时会比较卡，解决方案是用 -webkit-overflow-scrolling: touch; 替代。

.selector {
    -webkit-overflow-scrolling: touch;
}
元素点击
由于元素面记过小（移动端一般手指的触点应该大于5cm²），会导致难以点击，使用padding增大元素面积即可

iOS 事件绑定问题
iOS 如果不是 a 或者 button 绑定 click 事件会失效，解决办法 当前元素增加 CSS 属性。

selector {
    cursor: pointer;
} 
Canvas
性能调优
在微信内置浏览器中，由于x5内核的特殊性，canvas的性能会被劣化，可能一些常见的canvas调优技巧并不适用。

canvas 分层渲染，这个需要根据实际场景来判定，尽量减少 canvas 的数量
参考资料
X5内核咨询汇总
微信
« WordPress @ LAMP 踩坑
发表评论
电子邮件地址不会被公开。 必填项已用*标注

评论


姓名 *


电子邮件 *


站点


Powered by WordPress
Theme By 深蓝