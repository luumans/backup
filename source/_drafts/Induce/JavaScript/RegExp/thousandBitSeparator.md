title:  为数字添加千位分隔符
date: 2016-01-15 13:58:41
description: 
categories:
- HTML
tags:
- RegExp
toc: true
author:
comments:
original:
permalink: 
---
　　** RegExp：**最近被同事问到js如何实现给长数字添加千位分隔符，即 1344444 ---> 13,444,444 这是一个很常见的前端面试题。看起来简单，刚开始我都懒得写。
<!-- more -->


### 千位分隔符(js 实现)
最近被同事问到js如何实现给长数字添加千位分隔符，即 1344444 ---> 13,444,444 这是一个很常见的前端面试题。看起来简单，刚开始我都懒得写。
仔细一想，挺考逻辑的，实现方法有很多种，可以用三位循环、字符串数组分隔，也可以使用正则。刚开自己用js实现了堆栈，代码太多，不够优雅，同时也暴露了自己原生js的生疏，事后也看到了同事们各样的实现方法，无非都是循环和字符串分隔，于是决心使用更优雅的正则装逼一下。

仔细思考：
输入：数字(考虑数字是否合法、正负号、小数点)、字符串
输出：考虑到使用场景，最好是字符串

测试用例：-1234567.9012
期待输出：-1,234,567.9012

千位分隔符貌似在《精通正则表达式》中讲环视的时候作为经典范例，然而写出来发现js不支持逆序环视,也就是 `(?<=expression) (?<!expression) `这两种是不支持的。

再三考虑和尝试之后得出以下代码，只需一行！完美实现！

```
// 正则
function thousandBitSeparator(num) {
    return num && num
        .toString()
        .replace(/(\d)(?=(\d{3})+\.)/g, function($0, $1) {
            return $1 + ",";
        });
}
console.log(thousandBitSeparator(-1234567.9012));
// -1,234,567.9012
```

### todo
千位分隔符的完整攻略:推酷上这位仁兄使用ES6来实现，总结得不错：
js正则表达式：司徒正美有关js正则的文章，总结得不错
正则基础之——环视(Lookaround): 这篇博客讲解环视的基础知识，很详细（PS：原来C#实现千位分隔符直接用string.Format就可以做到啊囧rz）
--- update 2015年07月03日 ---

经朋友提醒，当测试用例是1000的时候，此正则不能正确标注千位分隔符，现修改如下：

```
function thousandBitSeparator(num) {
  return num && (num
    .toString().indexOf('.') != -1 ? num.toString().replace(/(\d)(?=(\d{3})+\.)/g, function($0, $1) {
      return $1 + ",";
    }) : num.toString().replace(/(\d)(?=(\d{3}))/g, function($0, $1) {
      return $1 + ",";
    }));
}
console.log(thousandBitSeparator(1000));
//1,000
```