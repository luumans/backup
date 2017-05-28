title: React Native Flexbox
date: 2017-05-25 18:29:00
description: 
categories:
- CSS
tags:
- Flex
toc: true
author:
comments:
original:
permalink: 
---
![](https://cdn.scotch.io/scotchy-uploads/2015/04/A-Vusial-Guide-to-CSS3-Flexbox-Layout-and-Properties.png)
　　**传统布局：**布局的传统解决方案，基于盒状模型，依赖 display属性 + position属性 + float属性。它对于那些特殊布局非常不方便，比如，垂直居中就不容易实现。
　　**缺点：**缺少语义，不够灵活
2009年，W3C提出了一种新的方案----Flex布局，可以简便、完整、响应式地实现各种页面布局。目前，它已经得到了所有浏览器的支持，这意味着，现在就能很安全地使用这项功能。
　　**Flex布局：**是Flexible Box的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。
- [X] Add Frontend Roadmap
- [X] Add Backend Roadmap
- [X] Add DevOps Roadmap
- [ ] Add relevant resources for each
<!-- more -->

# Basics

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。

<div class="box">
  <div class="item"><span>1</span></div>
  <div class="item"><span>2</span></div>
  <div class="item"><span>3</span></div>
</div>
<style type="text/css">
	.box {
		width: 50%;
		margin: auto;
		height: 200px;
		background-color: #ffd550;
		border: 1px solid #aaa;
	}
	.box .item {
		width: 50px;
		height: 50px;
		background-color: #999;
		border: 1px solid #000;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.box .item span{
		width: 40px;
		height: 40px;
		margin: 5px;
		border-radius: 50%;
		text-align: center;
		line-height: 40px;
		display: inline-block;
		background: #ffd550;
	}
	.w3{
		height: 100px;
	}
	.w4{ height: 210px;font-size: 30px; }
</style>

## flex-direction
用于指定Flex主轴的方向，继而决定 Flex子项在Flex容器中的位置

取值：row | row-reverse | column | column-reverse

row：默认值，表示水平方向从左到右排列，此时水平方向轴线为主轴
row-reverse：与row相反
column：表示垂直方向从上到下排列，此时垂直方向轴线为主轴
column-reverse：与column相反

### row

<div class="box div1">
  <div class="item"><span>1</span></div>
  <div class="item"><span>2</span></div>
  <div class="item"><span>3</span></div>
</div>
<style type="text/css">
	.box.div1 {
	  display: flex;
	}
	.item {
	}
</style>

### row-reverse

<div class="box div2">
  <div class="item"><span>1</span></div>
  <div class="item"><span>2</span></div>
  <div class="item"><span>3</span></div>
</div>
<style type="text/css">
	.box.div2{
	  display: flex;
	  flex-direction: row-reverse;
	}
</style>

### column

<div class="box div3">
  <div class="item"><span>1</span></div>
  <div class="item"><span>2</span></div>
  <div class="item"><span>3</span></div>
</div>
<style type="text/css">
	.box.div3{
	  display: flex;
	  flex-direction: column;
	}
</style>

### column-reverse

<div class="box div4">
  <div class="item"><span>1</span></div>
  <div class="item"><span>2</span></div>
  <div class="item"><span>3</span></div>
</div>
<style type="text/css">
	.box.div4{
	  display: flex;
	  flex-direction: column-reverse;
	}
</style>

## flex-wrap
用于指定Flex子项是否换行

取值：nowrap | wrap | wrap-reverse

nowrap：默认值，表示不换行，Flex子项可能会溢出
wrap：表示换行，溢出的Flex子项会被放到下一行
wrap-reverse：表示反方向换行

注：从示例图中不难发现，换行以后两行之间产生了很大的间距，这个问题，在后面介绍的 align-content 属性中可以得到很好的解决。

### nowrap

<div class="box div5">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div5{
	  display: flex;
	}
</style>

### wrap

<div class="box div6">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div6{
	  display: flex;
	  flex-wrap: wrap;
	}
</style>

### wrap-reverse

<div class="box div7">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div7{
	  display: flex;
	  flex-wrap: wrap-reverse;
	}
</style>

## flex-flow
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
flex-flow: <flex-direction> || <flex-wrap>;

### column wrap

<div class="box div0">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div0{
	  display: flex;
	  flex-flow: column wrap;
	}
</style>

## justify-content
用于指定主轴上Flex子项的对齐方式

取值：flex-start | flex-end | center | space-between | space-around

flex-start：默认值，表示与行的起始位置对齐
flex-end：表示与行的结束位置对齐
center：表示与行中间对其
space-between：表示两端对齐，中间间距相等。要注意特殊情况，当剩余空间为负数或者只有一个项时，效果等同于flex-start
space-around：表示间距相等，中间间距是两端间距的2倍。要注意特殊情况，当剩余空间为负数或者只有一个项时，效果等同于center

### flex-start

<div class="box div8">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
</div>
<style type="text/css">
	.box.div8{
	  display: flex;
	  justify-content: flex-start;
	}
</style>

### flex-end

<div class="box div9">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
</div>
<style type="text/css">
	.box.div9{
	  display: flex;
	  justify-content: flex-end;
	}
</style>

### center

<div class="box div10">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
</div>
<style type="text/css">
	.box.div10{
	  display: flex;
	  justify-content: center;
	}
</style>

### space-between

<div class="box div11">
	<div class="item"><span>1</span></div>
</div>

<div class="box div11">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
</div>

<div class="box div11">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div11{
	  display: flex;
	  justify-content: space-between;
	}
</style>

### space-around

<div class="box div12">
	<div class="item"><span>1</span></div>
</div>

<div class="box div12">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
</div>

<div class="box div12">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div12{
	  display: flex;
	  justify-content: space-around;
	}
</style>

#### cross

<div class="box div11s">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>2</span></div>
</div>

<style type="text/css">
	.box.div11s{
	  display: flex;
	  justify-content: space-between;
	  flex-direction: column;
	}
</style>

<div class="box div12s">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>2</span></div>
</div>

<style type="text/css">
	.box.div12s{
	  display: flex;
	  justify-content: space-around;
	  flex-direction: column;
	}
</style>

## align-items

用于指定垂直方向上Flex子项的对齐方式

取值：stretch | flex-start | flex-end | center | baseline

stretch：默认值，当Flex子项未设置高度或者高度值为auto时，stretch起作用，将Flex子项高度设置为行高度。这里需要注意，在只有一行的情况下，行的高度为容器的高度，即Flex子项高度为容器的高度
flex-start：表示与侧轴开始位置对齐
flex-end：表示与侧轴的结束位置对齐
center：表示与侧轴中间对其
baseline：表示基线对齐，当行内轴与侧轴在同一线上，即所有Flex子项的基线在同一线上时，效果等同于flex-start

### stretch

<div class="box div13">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div13{
	  display: flex;
	  justify-content: space-around;
	  align-items: stretch;
	}
</style>

### flex-start

<div class="box div14">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div14{
	  display: flex;
	  justify-content: space-around;
	  align-items: flex-start;
	}
</style>

### flex-end

<div class="box div15">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div15{
	  display: flex;
	  justify-content: space-around;
	  align-items: flex-end;
	}
</style>

### center

<div class="box div16">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div16{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
</style>

### baseline

<div class="box div17">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div17{
	  display: flex;
	  justify-content: space-around;
	  align-items: baseline;
	}
</style>

<div class="box div17s">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div17s{
	  display: flex;
	  flex-direction: column;
	  justify-content: space-around;
	  align-items: space-around;
	}
</style>

## align-content
该属性只作用于多行的情况下，用于多行的对齐方式

取值：stretch | flex-start | flex-end | center | space-between | space-around

stretch：默认值，当Flex子项未设置高度或者高度值为auto时，stretch起作用，将Flex子项高度设置为行高度。
flex-start：表示各行与侧轴开始位置对齐，第一行紧靠侧轴开始边界，之后的每一行都紧靠前面一行
flex-end：表示各行与侧轴的结束位置对齐，最后一行紧靠侧轴结束边界，之后的每一行都紧靠前面一行
center：表示各行与侧轴中间对其
space-between：表示两端对齐，中间间距相等。要注意特殊情况，当剩余空间为负数时，效果等同于flex-start
space-around：表示各行之间间距相等，中间间距是两端间距的2倍。要注意特殊情况，当剩余空间为负数时，效果等同于center

注：该属性只作用于多行的情况，在只有一行的Flex容器上无效，另外该属性可以很好的处理，换行以后相邻行之间产生的间距。

### stretch

<div class="box div18">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div18{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: stretch;
	}
</style>

### flex-start

<div class="box div19">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div19{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: flex-start;
	}
</style>

### flex-end

<div class="box div20">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div20{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: flex-end;
	}
</style>

### center

<div class="box div21">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div21{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: center;
	}
</style>

### space-between

<div class="box div22">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div22{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: space-between;
	}
</style>

### space-around

<div class="box div23">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div23{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: space-around;
	}
</style>

#### cross


<div class="box div23s">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div23s{
	  display: flex;
	  flex-wrap: wrap;
	  flex-direction: column;
	  align-content: space-around;
	}
</style>

# Flex 作用于子项

## order
该属性用来指定Flex子项的排列顺序，数值越小，越靠前，可以为负数
取值：数值，默认值为0

<div class="box div24">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item o0"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item o0"><span>10</span></div>
	<div class="item"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div24{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: space-around;
	}
	.div24 .o0{
		order: 1;
	}
</style>

## flex-grow
用来指定Flex子项的扩展比例，不可以为负数，Flex容器会根据Flex子项设置的扩展比例作为比率来分配剩余空间

取值：数值，默认值为0，表示即使存在剩余空间，Flex子项也不会扩展

<div class="box div25">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>6</span></div>
	<div class="item grow3"><span>7</span></div>
	<div class="item"><span>8</span></div>
	<div class="item"><span>9</span></div>
	<div class="item"><span>10</span></div>
	<div class="item grow3"><span>11</span></div>
	<div class="item"><span>12</span></div>
	<div class="item"><span>13</span></div>
	<div class="item"><span>14</span></div>
	<div class="item"><span>15</span></div>
</div>
<style type="text/css">
	.box.div25{
	  display: flex;
	  flex-wrap: wrap;
	  align-content: space-around;
	}
	.div25 .grow3{
		flex-grow: 3;
	}
</style>

## flex-shrink
<div class="box div26">
	<div class="item"><span>1</span></div>
	<div class="item g3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item g3"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>15</span></div>
</div>

如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
<div class="box div26">
	<div class="item"><span>1</span></div>
	<div class="item"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item"><span>4</span></div>
	<div class="item"><span>5</span></div>
	<div class="item"><span>15</span></div>
</div>

<style type="text/css">
	.box.div26{
	  display: flex;
	}
	.div26 .item{
		width: 30%;
	}
	.div26 .g3{
		flex-shrink: 0;
	}
</style>


## flex-basis
用来指定Flex子项的占据的空间，不可以为负数

取值：auto | length | percentage | content

auto：默认值，计算规则：检索Flex子项是否设置了width值（或者height值，取决于flex-direction），如果设置了非auto的值，则使用width值（或者height值），若没有则使用content
length：用长度值定义宽度，不可为负数
percentage：使用百分比定义宽度，不可为负数
### length

<div class="box div40">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
	<!-- <div>5</div> -->
</div>
<style type="text/css">
	.box.div40{
	  display: flex;
	}
	.div40 .item{
		width: 30%;
	}
	.div40 .w4{
		flex-basis: 20px;
	}
</style>

## flex
复合属性，是flex-grow 、 flex-shrink 和 flex-basis 的简写属性，用来指定Flex子项如何分配空间

取值：none | 各拆分项属性

none：0 0 auto
auto：1 1 auto
1：1 1 0%
0 auto 或 initial：0 1 auto 即初始值
注意：

flex-grow：默认值为0，若省略则被默认为1
flex-shrink：默认值为1，省略时默认为1
flex-basis：默认值为auto，省略时默认为0%

## align-self
用来单独指定某Flex子项的对齐方式

取值：auto | flex-start | flex-end | center | baseline | stretch

auto：默认值，查找父元素的align-items值，如果没有则取值为stretch
其他值同align-items

### auto

<div class="box div27">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div27{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
	.div27 .w3{
		align-self: auto;
	}
</style>

### flex-start

<div class="box div28">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div28{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
	.div28 .w3{
		align-self: flex-start;
	}
</style>

### flex-end

<div class="box div29">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div29{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
	.div29 .w3{
		align-self: flex-end;
	}
</style>

### center

<div class="box div30">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div30{
	  display: flex;
	  justify-content: space-around;
	  /*align-items: center;*/
	}
	.div30 .w3{
		align-self: center;
	}
</style>

### baseline

<div class="box div31">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div31{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
	.div31 .w3{
		align-self: baseline;
	}
</style>

### stretch

<div class="box div32">
	<div class="item"><span>1</span></div>
	<div class="item w3"><span>2</span></div>
	<div class="item"><span>3</span></div>
	<div class="item w4"><span>4</span></div>
</div>
<style type="text/css">
	.box.div32{
	  display: flex;
	  justify-content: space-around;
	  align-items: center;
	}
	.div32 .w3{
		align-self: stretch;
	}
</style>

# 拓展阅读

- [A Visual Guide to CSS3 Flexbox Properties](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties "CSS3 Flexbox性能的视觉指南")
- [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/ "Flexbox完整指南")
- [CSS Flexible Box Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout "灵活的盒子模型布局")
- [junruchen.github.io](https://github.com/junruchen/junruchen.github.io/wiki "")