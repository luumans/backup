title: 响应式布局
date: 2017-08-25 18:29:00
description: 
categories:
- Induce
tags:
- Sass
toc: true
author:
comments:
original:
permalink: 
---
　　**移动适配：**目前非常流行自适应设计与响应式设计，而且经常让人混淆，自适应设计不应与自适应布局混为一谈，它们是完全不一样的概念。
<!-- more -->

# 媒体查询(media query)
## 基础
only(限定某种设备)
screen 是媒体类型里的一种
and 被称为关键字，其他关键字还包括 not(排除某种设备)

| media type | 设备类型 |
|  ---- | ---- |
| all | 所有设备 |
| screen | 电脑显示器 |
| print | 打印用纸或打印预览视图 |
| handheld | 便携设备 |
| tv | 电视机类型的设备 |
| speech | 语意和音频盒成器 |
| braille | 盲人用点字法触觉回馈设备 |
| embossed | 盲文打印机 |
| projection | 各种投影设备 |
| tty | 使用固定密度字母栅格的媒介，比如电传打字机和终端 |

| media feature | 设备特性 |
|  ---- | ---- |
| width | 浏览器宽度 |
| height | 浏览器高度 |
| device-width | 设备屏幕分辨率的宽度值 |
| device-height | 设备屏幕分辨率的高度值 |
| orientation | 浏览器窗口的方向纵向还是横向，当窗口的高度值大于等于宽度时该特性值为portrait，否则为landscape |
| aspect-ratio | 比例值，浏览器的纵横比 |
| device-aspect-ratio | 比例值，屏幕的纵横比 |

```
@media screen and (max-width:400px){ 
    .class  {
         background:#ccc; 
     }
 }
 <link rel="stylesheet" media="screen and (max-width: 400px)" href="mini.css" />

@media screen and (min-width:800px){
  .class
  {
    background:#666;
  }
}

<!-- Device Width：若浏览设备的可视范围最大为480px，则下方的CSS描述就会立即被套用：(注：移动手机目前常见最大宽度为480px，如iPhone or Android Phone) -->
@media screen and (max-device-width:480px){
  .class
  {
    background:#000;
  }
}
<link rel="stylesheet" media="screen and (max-width: 400px)" href="mini.css" />
<link rel= "stylesheet"  media= "only screen and (-webkit-min-device-pixel-ratio: 2)"  type= "text/css"  href= "iphone4.css"  />

<!-- 针对iPad的Portrait Mode(直立)与Landscape Mode(横躺) -->
<link rel="stylesheet" media="all and (orientation:portrait)" href="portrait.css">
<link rel="stylesheet" media="all and (orientation:landscape)" href="landscape.css">

<!-- 上面针对了所有设备，这段是只(only)针对彩色屏幕设备 -->
<!-- ipad -->
@media only screen and (min-width:768px)and(max-width:1024px){}

<!-- iphone -->
@media only screen and (width:320px)and (width:768px){}
```
## 页面尺寸

| Name | Javascript |
|  ---- | ---- |
| 网页可见区域宽 | document.body.clientWidth |
| 网页可见区域高 | document.body.clientHeight |
| 网页可见区域宽 | document.body.offsetWidth (包括边线和滚动条的宽) |
| 网页可见区域高 | document.body.offsetHeight(包括边线的宽) |
| 网页正文全文宽 | document.body.scrollWidth |
| 网页正文全文高 | document.body.scrollHeight |
| 网页被卷去的高 | document.body.scrollTop |
| 网页被卷去的左 | document.body.scrollLeft |
| 网页正文部分上 | window.screenTop |
| 网页正文部分左 | window.screenLeft |
| 屏幕分辨率的高 | window.screen.height |
| 屏幕分辨率的宽 | window.screen.width |
| 屏幕可用工作区高度 | window.screen.availHeight |
| 屏幕可用工作区宽度 | window.screen.availWidth |
| 屏幕设置 | window.screen.colorDepth 位彩色 |
| 屏幕设置 | window.screen.deviceXDPI 像素/英寸 |

## 屏幕尺寸

| 物理像素 | 设备像素 | 设备像素比 |
|  ---- | ---- | ---- |
|  window.screen.width | document.body.clientWidth | window.devicePixelRatio |

## iPhone界面尺寸

|  设备 | 设备像素 | 物理像素 | 设备像素比 | flexbile |
|  ---- | ---- | ---- | ---- | ---- |
| iPhone6P | ---- | ---- | ---- | ---- |
|  iPhone6 | ---- | ---- | ---- | ---- |
|  iPhone5 | ---- | ---- | ---- | ---- |
|  iPhone4 | ---- | ---- | ---- | ---- |
|  iPhone  | ---- | ---- | ---- | ---- |



- [移动设备尺寸规范汇总](http://www.iaxure.com/2696.html)




