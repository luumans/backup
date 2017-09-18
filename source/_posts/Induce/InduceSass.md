title: 深入Sass
date: 2017-08-18 18:29:00
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
**SASS**是一种CSS的开发工具，提供了许多便利的写法，大大节省了设计者的时间，使得CSS的开发，变得简单和可维护。
你可以用它开发网页样式，但是没法用它编程。也就是说，CSS基本上是设计师的工具，不是程序员的工具。在程序员眼里，CSS是一件很麻烦的东西。它没有变量，也没有条件语句，只是一行行单纯的描述，写起来相当费事。
<!-- more -->

# 基础

## 变量与选择器
### 变量
变量的定义一般以$开头，某个变量的作用域仅限于他们定义的层级以及子层级。如果变量是定义在所有嵌套选择器之外的，那么他们可以在各处被调用。

```scss
$color1: #aeaeae;
.div1{
  background-color: $color1;
}
```
```css
.div1 {
  background-color: #aeaeae;
}
<!-- /*# sourceMappingURL=test.css.map */ -->
```

> 变量的作用域
如果希望某个在子选择器中定义的变量能够成为全局变量，可以使用!global关键字：

```scss
#main {
  $width: 5em !global;
  width: $width;
}

#sidebar {
  width: $width;
}
```

#### 嵌套引用
```scss
$side: top;
$radius: 10px;
.round-#{$side} {
  border-#{$side}-radius: $radius;
  -moz-border-#{$side}-radius: $radius;
  -webkit-border-#{$side}-radiux: $radius;
}
```

```css
.round-top {
  border-top-radius: 10px;
  -moz-border-top-radius: 10px;
  -webkit-border-top-radiux: 10px;
}
```

#### 变量计算
```scss
$left: 20px;
.div1{
    margin-left:$left+12px;
}
```

> 计算的类型

```scss
p {
  <!-- Plain CSS, no division -->
  font: 10px/8px;
  $width: 1000px;
  <!-- Uses a variable, does division -->
  width: $width/2;
  <!-- Uses a function, does division -->
  width: round(1.5)/2;
  <!-- Uses parentheses, does division -->
  height: (500px/2);
  <!-- Uses +, does division -->
  margin-left: 5px + 8px/2px;
  <!-- In a list, parentheses don't count -->
  font: (italic bold 10px/8px);
}
```

### 选择器

#### 嵌套
```scss
.div1{
  .span1{
    height: 12px;
  }
  .div2{
    width: 16px;
  }
}
```

```scss
p{
	border:{
		color: red;
	}
}
```
注意: border后面必须加上冒号

#### 父元素引用
允许使用&引用父元素
```scss
.div1{
  &:hover{
    cursor: hand;
  }
}
```

## 代码重用
### 继承
SASS允许一个选择器，继承另一个选择器。
```scss
.class1{
  font-size:19px;
}
.class2{
  @extend .class1;
  color:black;
}
```

```css
.class1, .class2 {
  font-size:19px;
}
.class2 {
  color:black;
}
```

> 注意：如果在class2后面有设置了class1的属性，那么也会影响class2

```scss
.class1{
    font-size:19px;
}
.class2{
    @extend .class1;
    color:black;
}
.class1{
    font-weight:bold;
}
```

### 占位符
```scss
%class1{
  font-size:19px;
}
.class2{
  @extend %class1;
  color:black;
}
```

```css
.class2{
  font-size:19px;
  color:black;
}
```

### 引用外部
```scss
@import "_test1.scss";
@import "_test2.scss";
@import "_test3.scss";
```

### Mixin&Include
Mixin是SASS中非常强大的特性之一。定义mixin时，需要在前面加@mixin，使用时需要添加@include来引用该mixin。

```scss
@mixin left {
	float: left;
	margin-left: 10px;
}

div {
	@include left;
}
```

```css
div {
	float: left;
	margin-left: 10px;
}
```


#### 边距设置
```scss
@mixin common($value1,$value2,$defaultValue:12px) {
  display:block;
  margin-left:$value1;
  margin-right:$value2;
  padding:$defaultValue;
}

.class1 {
  font-size:16px;
  @include common(12px,13px,15px);
}

.class2 {
  font-size:16px;
  @include common(12px,13px);
}
```

#### 浏览器前缀设置
```scss
@mixin rounded($vert, $horz, $radius: 10px) {
	border-#{$vert}-#{$horz}-radius: $radius;
	-moz-border-radius-#{$vert}#{$horz}: $radius;
	-webkit-border-#{$vert}-#{$horz}-radius: $radius;
}

#navbar li {
	@include rounded(top, left);
}
#footer {
	@include rounded(top, left, 5px);
}
```


## 编程式方法
### 流程控制

#### 条件语句

> @if

```scss
p {
	@if 1 + 1 == 2 {
		border: 1px solid;
	}
	@if 5 < 3 {
		border: 2px dotted;
	}
}
```

> @else

```scss
@if lightness($color) > 30% {
	background-color: #000;
} @else {
	background-color: #fff;
}
```

#### 循环语句
> for循环

```scss
@for $i from 1 to 5 {
  .border-#{$i} {
    border: #{$i}px solid blue;
  }
}
```

```css
/* line 149, ../sass/style.scss */
.border-1 {
  border: 1px solid blue;
}

/* line 149, ../sass/style.scss */
.border-2 {
  border: 2px solid blue;
}

/* line 149, ../sass/style.scss */
.border-3 {
  border: 3px solid blue;
}

/* line 149, ../sass/style.scss */
.border-4 {
  border: 4px solid blue;
}
```

> while循环

```scss
$i: 1;
@while $i < 5 {
  .border-#{$i} { border: #{$i}px solid blue; }
  $i: $i + 1;
}
```
```css
/* line 156, ../sass/style.scss */
.border-1 {
  border: 1px solid blue;
}

/* line 156, ../sass/style.scss */
.border-2 {
  border: 2px solid blue;
}

/* line 156, ../sass/style.scss */
.border-3 {
  border: 3px solid blue;
}

/* line 156, ../sass/style.scss */
.border-4 {
  border: 4px solid blue;
}
```

> each命令，作用与for类似

```scss
@each $item in add, update, remove, share {
  .icon-#{$item} {
    background-image: url("/image/#{$item}.jpg");
  }
}
```
```css
/* line 161, ../sass/style.scss */
.icon-add {
  background-image: url("/image/add.jpg");
}

/* line 161, ../sass/style.scss */
.icon-update {
  background-image: url("/image/update.jpg");
}

/* line 161, ../sass/style.scss */
.icon-remove {
  background-image: url("/image/remove.jpg");
}

/* line 161, ../sass/style.scss */
.icon-share {
  background-image: url("/image/share.jpg");
}
```

### 函数
```scss
@function double($n) {
	@return $n * 2;
}

#sidebar {
	width: double(5px);
}
```

```css
#navbar {
  width: 10px;
}
```

#### 颜色函数
SASS提供了一些内置的颜色函数，以便生成系列颜色。
```scss
lighten(#cc3, 10%)
// #d6d65c

darken(#cc3, 10%)
//  #a3a329

grayscale(#cc3)
// #808080

complement(#cc3)
// #33c

```



# 拓展
## Sass 与 SCSS 是什么关系
1. sass受Haml简洁启发，Ruby的语法，没有花括号，没有分号，具有严格的缩进
Sass 从来没有大写过，无论你指的是语法或者这个语言。同时， SCSS 一直是大写的。甚至有一个网站专门来提醒你这件事!

### .sass
```sass
$def-color: #333
body
  font: 100%
  color: $def-color
```

### .scss
```scss
$def-color: #333
body{
  font: 100%；
  color: $def-color；
}
```

## 使用Sass之更高级的媒体查询

```scss
/ _config.scss
$breakpoints: (
  'xs': 'only screen and ( min-width: 480px)',
  'sm': 'only screen and ( min-width: 768px)',
  'md': 'only screen and ( min-width: 992px)',
  'lg': 'only screen and ( min-width: 1200px)',
) !default;
```

```scss
// _mixins.scss
@mixin respond-to($breakpoint) {
  $query: map-get($breakpoints, $breakpoint);
  
  @if not $query {
    @error 'No value found for `#{$breakpoint}`. Please make sure it is defined in `$breakpoints` map.';
  }
  @media #{if(type-of($query) == 'string', unquote($query), inspect($query))} {
    @content;
  }
}
```

### 使用
```scss
// _component.scss
.element {
  color: hotpink;
  @include respond-to(sm) {
    color: tomato;
  }
}
```
### 输出
```css
.element {
  color: hotpink;
}
@media (min-width: 768px) {
  .element {
    color: tomato;
  }
}
```

## 巧用SASS之如何遍历n个子元素并为其设置属性

```html
<div id="main-container">
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
  </ul>
</div>
```



```scss
// 将背景颜色值定义成变量
$red : #FF0000;
$orange : #FFA500;
$yellow : #FFFF00;
$green : #008000;
$bluegreen : #00FFFF;
$blue : #0000FF;
$purple : #800080;

//将背景颜色以键值对的形式存在map中
$bgcolorlist : (
  1: $red,
  2: $orange,
  3: $yellow,
  4: $green,
  5: $bluegreen,
  6: $blue,
  7: $purple
);

// 使用SASS each语法为每一个li设置background-color
@each $i, $color in $bgcolorlist {
  #main-container ul li:nth-child(#{$i}) {
    background-color: $color;
  }
}
```

## 设置rem，控制width
```scss
@function size($size) {
  $width: 375;
  $scale: 10;
  @return ($size / $width * $scale) * 1rem;
}
```

- [sass入门](http://www.w3cplus.com/sassguide/ "")
- [sass](http://sass-lang.com/ "")
- [使用Sass之更高级的媒体查询](https://www.w3ctrain.com/2015/12/02/sass-media-query/ "")
- [学习SASS笔记](http://blog.csdn.net/qishuixian/article/details/54578212 "")
- [巧用SASS之如何遍历n个子元素并为其设置属性](https://segmentfault.com/a/1190000005942514 "")
- [几个实用的Sass mixins](https://www.w3ctrain.com/2016/02/21/useful-sass-mixins/ "")
- [sass-svg 一个内联 SVG 的 SASS 库](https://aotu.io/notes/2017/01/19/sass-svg/?o2src=juejin&o2layout=compat "")
- [跟随动画的高效实现方法：GreenSock 和 Web Animations API](http://svgtrick.com/tricks/78c525bfb98a36b85481f5d423f2301a "")
- []( "")
- []( "")
- []( "")
