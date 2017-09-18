title: CSS3自定义滚动条样式 -webkit-scrollbar
date: 2017-09-12 18:29:00
description:
categories:
- Induce
tags:
- CSS
toc: true
author:
comments:
original:
permalink:
---
webkit支持拥有overflow属性的区域，列表框，下拉菜单，textarea的滚动条自定义样式，所以用处还是挺大的。当然，兼容所有浏览器的滚动条样式目前是不存在的。
<!-- more -->

# 滚动条组成

字符   |   描述
-------|------
::-webkit-scrollbar | 滚动条整体部分
::-webkit-scrollbar-thumb | 滚动条里面的小方块，能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
::-webkit-scrollbar-track | 滚动条的轨道（里面装有Thumb）
::-webkit-scrollbar-button | 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
::-webkit-scrollbar-track-piece | 内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-corner | 边角，即两个滚动条的交汇处
::-webkit-resizer | 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件

``` css
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar{
  width: 16px;
  height: 16px;
  background-color: #F5F5F5;
}
/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track{
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
  border-radius: 10px;
  background-color: #F5F5F5;
}
/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb{
  border-radius: 10px;
  -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
  background-color: #555;
}
```

[demo](http://www.xuanfengge.com/demo/201311/scroll/css3-scroll.html "")

```
<div class="scrollbar" id="style-1">
  <div class="force-overflow"></div>
</div>

/*
 *  STYLE 1
 */
#style-1::-webkit-scrollbar-track{
	-webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
	border-radius: 10px;
	background-color: #F5F5F5;
}
#style-1::-webkit-scrollbar{
	width: 12px;
	background-color: #F5F5F5;
}
#style-1::-webkit-scrollbar-thumb{
	border-radius: 10px;
	-webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
	background-color: #555;
}
```

[demo](http://www.xuanfengge.com/demo/201311/scroll/index.html "")

``` css
*{
	margin: 0;
	padding: 0;
}
#container{
	width: 100%;
	height: 1200px;
	line-height: 600px;
	text-align: center;
	background: #ccc;
}
/*定义滚动条的高宽*/
::-webkit-scrollbar {
  width: 16px;
  height: 16px;
}
/*CSS的坐标系，左上角为(0,0),往右往下为增加，往上往左为减少*/
/*显示滚动条上方的渐增按钮*/
::-webkit-scrollbar-button:start:decrement,
/*显示滚动条上方的渐减按钮*/
::-webkit-scrollbar-button:end:increment {
  display: block;
}
/*隐藏滚动条上方的渐增按钮*/
::-webkit-scrollbar-button:vertical:start:increment,
::-webkit-scrollbar-button:vertical:end:decrement {
  display: none;
}
/* 定义滚动条渐增按扭的样式 */
::-webkit-scrollbar-button:end:increment {
  background-image: url(./images/scroll_cntrl_dwn.png);
}
/* 定义滚动条渐减按扭的样式 */
::-webkit-scrollbar-button:start:decrement {
  background-image: url(./images/scroll_cntrl_up.png);
}
/* 垂直滚动条的第三层轨道的上段 */
::-webkit-scrollbar-track-piece:vertical:start {
  background-image: url(./images/scroll_gutter_top.png), url(./images/scroll_gutter_mid.png);
  background-repeat: no-repeat, repeat-y;
}
/* 垂直滚动条的第三层轨道的下段 */
::-webkit-scrollbar-track-piece:vertical:end {
  background-image: url(./images/scroll_gutter_btm.png), url(./images/scroll_gutter_mid.png);
  background-repeat: no-repeat, repeat-y;
  background-position: bottom left, 0 0;
}
/* 垂直滚动条的滑动块 */
::-webkit-scrollbar-thumb:vertical {
  height: 56px;
  -webkit-border-image: url(./images/scroll_thumb.png) 8 0 8 0 stretch stretch;
  border-width: 8 0 8 0;
}
```
