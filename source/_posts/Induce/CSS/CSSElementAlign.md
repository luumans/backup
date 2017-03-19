title: 用CSS实现元素垂直居中方案
date: 2017-01-02 18:29:00
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

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->


### 固定高度绝对定位
>使用position:absolute,设置left、top、margin-left、margin-top的属性
这种方法基本浏览器都能够兼容，不足之处就是需要固定宽高。

```
	.one{
		position:absolute;
		width:200px;
		height:200px;
		top:50%;
		left:50%;
		margin-top:-100px;
		margin-left:-100px;
		background:red; 
	}
```

<!-- <div class="one">one</div> -->
<style type="text/css">
	.one{
		position:absolute;
		width:200px;
		height:200px;
		top:50%;
		left:50%;
		margin-top:-100px;
		margin-left:-100px;
		background:red; 
	}
</style>

### 使用position:fixed,同样设置left、top、margin-left、margin-top的属性
```
	.two{
		position:fixed;
		width:180px;
		height:180px;
		top:50%;
		left:50%;
		margin-top:-90px;
		margin-left:-90px;
		background:orange;
	}
```
大家都知道的position:fixed,IE是不支持这个属性的


<!-- <div class="two">two</div> -->
<style type="text/css">
	.two{
		position:fixed;
		width:180px;
		height:180px;
		top:50%;
		left:50%;
		margin-top:-90px;
		margin-left:-90px;
		background:orange;
	}
</style>
### 利用position:fixed属性，margin:auto这个必须不要忘记了。
```
	.three{
		position:fixed;
		width:160px;
		height:160px;
		top:0;
		right:0;
		bottom:0;
		left:0;
		margin:auto;
		background:pink;
	}
```
<!-- <div class="three">three</div> -->
<style type="text/css">
	.three{
		position:fixed;
		width:160px;
		height:160px;
		top:0;
		right:0;
		bottom:0;
		left:0;
		margin:auto;
		background:pink;
	}
</style>
### 利用position:absolute属性，设置top/bottom/right/left
```
	.four{
		position:absolute;
		width:140px;
		height:140px;
		top:0;
		right:0;
		bottom:0;
		left:0;
		margin:auto;
		background:black;
	}
```
<!-- <div class="four">four</div> -->
<style type="text/css">
	.four{
		position:absolute;
		width:140px;
		height:140px;
		top:0;
		right:0;
		bottom:0;
		left:0;
		margin:auto;
		background:black;
	}
</style>
### 文字垂直水平居中
>利用display:table-cell属性使内容垂直居中

```
	.five{
		display:table-cell;
		vertical-align:middle;
		text-align:center;
		width:120px;
		height:120px;
		background:purple;
	}
```

<!-- <div class="five">five</div> -->
<style type="text/css">
	.five{
		display:table-cell;
		vertical-align:middle;
		text-align:center;
		width:120px;
		height:120px;
		background:purple;
	}
</style>
### 文字垂直水平居中
>最简单的一种使行内元素居中的方法，使用line-height属性
这种方法也很实用，比如使文字垂直居中对齐

```
	.six{
		width:100px;
		height:100px;
		line-height:100px;
		text-align:center;
		background:gray;
	}
```

<!-- <div class="six">six</div> -->
<style type="text/css">
	.six{
		width:100px;
		height:100px;
		line-height:100px;
		text-align:center;
		background:gray;
	}
</style>
### 文字垂直水平居中
>使用css3的display:-webkit-box属性，再设置-webkit-box-pack:center/-webkit-box-align:center

```
	.seven{
		width:90px;
		height:90px;
		display:-webkit-box;
		-webkit-box-pack:center;
		-webkit-box-align:center;
		background:yellow;
		color:black;
	}
```
<!-- <div class="seven">seven</div> -->
<style type="text/css">
	.seven{
		width:90px;
		height:90px;
		display:-webkit-box;
		-webkit-box-pack:center;
		-webkit-box-align:center;
		background:yellow;
		color:black;
	}
</style>
### CSS translate偏移
>使用css3的新属性transform:translate(x,y)属性
这个方法可以不需要设定固定的宽高，在移动端用的会比较多，在移动端css3兼容的比较好

```
	.eight{
		position:absolute;
		width:80px;
		height:80px;
		top:50%;
		left:50%;
		transform:translate(-50%,-50%);
		-webkit-transform:translate(-50%,-50%);
		-moz-transform:translate(-50%,-50%);
		-ms-transform:translate(-50%,-50%);
		background:green;
	}
```
<!-- <div class="eight">eight</div> -->
<style type="text/css">
	.eight{
		position:absolute;
		width:80px;
		height:80px;
		top:50%;
		left:50%;
		transform:translate(-50%,-50%);
		-webkit-transform:translate(-50%,-50%);
		-moz-transform:translate(-50%,-50%);
		-ms-transform:translate(-50%,-50%);
		background:green;
	}
</style>
### before元素
>最高大上的一种，使用:before元素

```
	.nine{
		position:fixed;
		display:block;
		top:0;
		right:0;
		bottom:0;
		left:0;
		text-align:center;
		background:rgba(0,0,0,.5);
	}
	.nine:before{
		content:'';
		display:inline-block;
		vertical-align:middle;
		height:100%;
	}
	.nine .content{
		display:inline-block;
		vertical-align:middle;
		width:60px;
		height:60px;
		line-height:60px;
		color:red;
		background:yellow;
	}
```

<!-- <div class="nine"><div class="content">nine</div></div> -->
<style type="text/css">
	.nine{
		position:fixed;
		display:block;
		top:0;
		right:0;
		bottom:0;
		left:0;
		text-align:center;
		background:rgba(0,0,0,.1);
	}
	.nine:before{
		content:'';
		display:inline-block;
		vertical-align:middle;
		height:100%;
	}
	.nine .content{
		display:inline-block;
		vertical-align:middle;
		width:60px;
		height:60px;
		line-height:60px;
		color:red;
		background:yellow;
	}
</style>