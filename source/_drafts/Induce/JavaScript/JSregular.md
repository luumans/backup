title: 正则匹配
date: 2017-02-24 18:29:00
description:
categories:
- JavaScript
tags:
- Regular
toc: true
author:
comments:
original:
permalink:
---
　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
### RegExp 对象
正则表达式是描述字符模式的对象，用于对字符串模式匹配及检索替换，是对字符串执行模式匹配的强大工具。
#### 构造正则表达式
```
var patt=new RegExp(pattern模式,modifiers修饰符);
var reg = RegExp("a","gi");
// 匹配所有的a或A
var reg = new RegExp("\\w+");
注意：当使用构造函数创造正则对象时，需要常规的字符转义规则（在前面加反斜杠 \）。比如，以下是等价的：
```
#### 字面量创建正则表达式
```
var patt=/pattern/modifiers;
var reg = /a/gi;
```
#### 版本
```
function getRE(){
	var re = /[a-z]/;
	re.foo = 'bar';
	return re;
}
var reg = getRE();
re2 = getRE();
console.log(reg === re2);
reg.foo = 'baz';
console.log(re2.foo);

ECMAScript3同一对象
// true
// "baz"
ECMAScript5不同对象
// false
// "bar"
```
### 标点符号
!@#$%^&*({|\/?.+})需要使用\转义

### 修饰词
表达式 | 描述
-------|------
i      | 执行对大小写不敏感的匹配
g      | 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
m      | 执行多行匹配
```
var str='HwwwwLwello orllld lLll!';
console.log(str.match(/l/));
// ["l", index: 8, input: "HwwwwLwello orllld lLll!"]
console.log(str.match(/l/i));
// ["L", index: 5, input: "HwwwwLwello orllld lLll!"]
console.log(str.match(/l/g));
// ["l", "l", "l", "l", "l", "l", "l", "l"]

var str='Hwwwwl\nwello orllld lLll!';
console.log(str);
// Hwwwwl
// wello orllld lLll!
console.log(str.match(/l$/));
// null
console.log(str.match(/l$/m));
// ["l", index: 5, input: "Hwwwwl↵wello orllld lLll!"]
```
### 直接量字符
字符   |   描述
-------|------
字母数字| 自身
\0     | 查找 NUL 字符（\u0000）
\t     | 查找制表符（\u0009）
\v     | 查找垂直制表符（\u000A）
\n     | 查找换行符（\u000B）
\f     | 查找换页符（\u000C）
\r     | 查找回车符（\u000D）
\xdd   | 查找以十六进制数 dd 规定的字符（\x0A => \n）
\uxxxx | 查找以十六进制数 xxxx 规定的 Unicode 字符（\u0009 => \t）
\cX    | 控制字符^X （\cJ => \n）

### 字符类
字符     |   描述
---------|------
.        | 查找单个字符，除了换行和行结束符
\w       | 查找单词字符：[a-zA-Z_0-9]
\W       | 查找非单词字符：[^a-zA-Z0-9]
\s       | 查找空白字符
\S       | 查找非空白字符
\d       | 查找数字：[0-9]
\D       | 查找非数字字符：[^0-9]
[\b]     | 退格直接量
[...]    | 查找方括号之间的任何字符（没有顺序同级）
[^...]   | 查找不在方括号之间的任何字符
[abc]    | 查找abc之间的任何字母
[^abc]   | 查找任何不在abc之间的任何字母
[0-9]    | 查找任何从 0 至 9 的数字
[a-z]    | 查找任何从小写 a 到小写 z 的字符
[A-Z]    | 查找任何从大写 A 到大写 Z 的字符
[A-z]    | 查找任何从大写 A 到小写 z 的字符
[adgk]   | 查找给定集合内的任何字符
[^adgk]  | 查找给定集合外的任何字符

```
var str='12323 orllbld lLll!';
console.log(str.match(/[abc]/));
// ["b", index: 10, input: "12323 orllbld lLll!"]
console.log(str.match(/[ro3]/));
// ["3", index: 2, input: "12323 orllbld lLll!"]
console.log(str.match(/[^abc]/));
// ["1", index: 0, input: "12323 orllbld lLll!"]
```

### 重复字符
字符     | 描述
---------|----
X{n,m}   | 匹配包含 n 或 m 个 X 的序列的字符串。
X{n,}    | 匹配包含至少 n 个 X 的序列的字符串。
X{n}     | 匹配包含 n 个 X 的序列的字符串。
X?       | 匹配任何包含零个或一个 X 的字符串 {0,1}
X+       | 匹配任何包含至少一个 X 的字符串 {1,}
X*       | 匹配任何包含零个或多个 X 的字符串 {0,}

```
var str='Hwwwwlllll orlllld lll!';
console.log(str.match(/l{3,5}/));
// ["lllll", index: 5, input: "Hwwwwlllll orlllld lll!"]
console.log(str.match(/l{2,3}/));
// ["lll", index: 5, input: "Hwwwwlllll orlllld lll!"]
console.log(str.match(/l{0,1}/));
// ["", index: 0, input: "Hwwwwlllll orlllld lll!"]
console.log(str.match(/l?/));
// ["", index: 0, input: "Hwwwwlllll orlllld lll!"]
console.log(str.match(/l{1,}/));
// ["lllll", index: 5, input: "Hwwwwlllll orlllld lll!"]
console.log(str.match(/l+/));
// ["lllll", index: 5, input: "Hwwwwlllll orlllld lll!"]

var str='Hwwwwwello orllld llll!';
console.log(str.match(/l{3,5}/));
// ["lll", index: 13, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l{1,4}/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l{2}/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l{4,}/));
// ["llll", index: 18, input: "Hwwwwwello orllld llll!"]

console.log(str.match(/l?/));
// ["", index: 0, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l+/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l*/));
// ["", index: 0, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/w?/));
// ["", index: 0, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/w+/));
// ["wwwww", index: 1, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/w*/));
// ["", index: 0, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l/));
// ["l", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/ll/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/lll?/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]

var str='01';
console.log(str.match(/0?/));
// ["0", index: 0, input: "01"]
// ["", index: 0, input: "01"]
console.log(str.match(/1?/));
```

### 非贪婪重复
尽可能少的匹配：??、+?、*?、{1,4}?
```
var str='Hwwwwwello orllld llll!';
console.log(str.match(/l/));
// ["l", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l?/));
// ["", index: 0, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l+/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l+?/));
// ["l", index: 7, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/lll/));
// ["lll", index: 13, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l{3,4}/));
// ["lll", index: 13, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/l{3,4}?/));
// ["lll", index: 13, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/lll?/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
```

### 锚字符
字符     | 描述
---------|----
^        | 匹配字符串开头(用正则表达式处理多行时匹配行的开始)
$        | 匹配字符串结尾(处理多行时匹配行尾)
\b       | 匹配单词边界
\B       | 匹配非单词边界
(?=p)    | 零宽正向先行断言，要求接下来的字符都与p匹配，但不能包括匹配p的那些字符 (?=p) => p
(?!p)    | 零宽负向先行断言，要求接下来的字符不与p匹配 (?!p) => [^p]
X$       | 匹配任何结尾为 X 的字符串。
^X       | 匹配任何开头为 X 的字符串。
?=X      | 匹配任何其后紧接指定字符串 X 的字符串。
?!X      | 匹配任何其后没有紧接指定字符串 X 的字符串。

```
var str='orllld';
console.log(str.match(/^o/));
// ["o", index: 0, input: "orllld"]
console.log(str.match(/d$/));
// ["d", index: 5, input: "orllld"]
var str='orllld ';
console.log(str.match(/d$/));
// null
console.log(str.match(/\b/g));
// ["", ""]
console.log(str.match(/\B/g));
// ["", "", "", "", "", ""]
console.log(str.match(/d/));
// ["d", index: 5, input: "orllld "]
console.log(str.match(/(?=d)/));
// ["", index: 5, input: "orllld "]
console.log(str.match(/(?!d)/));
// ["", index: 0, input: "orllld "]

var str='JavaScript';
console.log(str.match(/Java(?!Script)([A-Z]\w*)/));
// null
var str='JavaBcript';
console.log(str.match(/Java(?!Script)([A-Z]\w*)/));
// ["JavaBcript", "Bcript", index: 0, input: "JavaType"]
var str='JavaType';
console.log(str.match(/Java(?!Script)([A-Z]\w*)/));
// ["JavaType", "Type", index: 0, input: "JavaType"]

var str='JavaScript';
console.log(str.match(/Java(?=Script)([A-Z]\w*)/));
// ["JavaScript", "Script", index: 0, input: "JavaScript"]
var str='JavaBcript';
console.log(str.match(/Java(?=Script)([A-Z]\w*)/));
// null
```

### 选择、分组、引用字符
字符     | 描述
---------|----
' '         | 选择，匹配的是该符号左边的子表达式或右边的子表达式
(...)       | 组合，将几个项组合为一个单元，这个单元可通过*、+、?、等符号加以修饰，而且可以记住和这个组合相匹配的字符串以供使用的字符。
(?:...)     | 只组合，把项组合到一个单元，但不记住与该组相匹配的字符
\n          | 匹配包含 X 个 n 的序列的字符串。

(red&|blue&|green)     查找任何指定的选项。
```
var str='Hwwwwwello orllld llll!';
console.log(str.match(/o|w|h/));
// ["w", index: 1, input: "Hwwwwwello orllld llll!"]
```

```
var str='Hwwwwwello orllld llll!';
console.log(str.match(/([Jj]ava(?:[Ss]cript)?)\sis\s(fun\w*)/));
// ["lll", index: 13, input: "Hwwwwwello orllld llll!"]
console.log(str.match(/lll?/));
// ["ll", index: 7, input: "Hwwwwwello orllld llll!"]
```
#### 非捕获性分组
```
reg = /abc{2}/;
// 将匹配abcc  
reg = /(abc){2}/;
// 将匹配abcabc  

// 上面的分组都是捕获性分组  
str = "abcabc ###";  
arr = reg.exec(str);  
alert(arr[1]);
// abc  

// 非捕获性分组 (?:)  
reg = /(?:abc){2}/;  
arr = reg.exec(str);  
alert(arr[1]);
// undefined
```

### RegExp 对象方法
#### source
正则表达式文本
```
var reg = /[a-z]/i;
reg.source
// "[a-z]"
```

#### global
只读布尔值，是否有修饰符g
```
var reg = /[a-z]/i;
reg.global
// false
```

#### ignoreCase
只读布尔值，是否有修饰符i
```
var reg = /[a-z]/i;
reg.ignoreCase
// true
```

#### multiline
只读布尔值，是否有修饰符m
```
var reg = /[a-z]/i;
reg.multiline
// false
```

#### lastIndex
下一次检索开始的位置，用于exec()和test()
```
var reg = /[a-z]/i;
reg.lastIndex
// 0
```

#### compile
把正则表达式编译为内部格式，从而执行得更快。 
```
var str="Every man in the world! Every woman on earth!";

patt=/man/g;
console.log(str.replace(patt,"person"));
console.log(str2+"<br />");

patt=/(wo)?man/g;
patt.compile(patt);
console.log(str.replace(patt,"person"));
```

#### exec
检索字符串中指定的值。返回找到的值，并确定其位置。
```
var date = 'Ubuntu 8';
reg = /^[a-z]+\s+\d+$/i; 
console.log(reg.exec(date));
// ["Ubuntu 8", index: 0, input: "Ubuntu 8"]

reg = /\d+/;
console.log(reg.exec(date));
// ["8", index: 7, input: "Ubuntu 8"]
```

#### test
检索字符串中指定的值。返回 true 或 false。可以修改lastIndex从指定位置开始匹配。
```
var date = 'Ubuntu 8';
var reg = /^[a-z]+\s+\d+$/i;
console.log(reg.test(date));
// true

var date = '1buntu 8';
var reg = /^[a-z]+\s+\d+$/i;
console.log(reg.test(date));
// false
```

### 支持模式匹配的String方法

#### search
可在字符串内检索指定的值，或找到一个正则表达式的匹配，得到第一个位置，没有则返回-1
```
var str='Hello world!';
str.search('l')
str.search(/l/i)

var str='Hello world!';
str.search('l')
str.search(/g/i)
```

#### split
方法用于将字符串转换为数组，参数是字符串转换数组后间隔的参照物，但是有一些复杂的转换就比较麻烦了，这时候我们可以使用正则表达式对字符串进行筛选后再组成数组：
```
str ="some some             \tsome\t\f";
re = /\s+/i;
console.log(str.split(re));
re = /\s+/g;
console.log(str.split(re));

var a = 'a1b2c3d4';
var reg = /[a-z]+/g;
console.log(a.split(reg));
```

#### match
可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配，的数组
```
检索字母l：
var str='Hello world!';
str.match('l')
str.match(/l/i)
// ['l']
index位数
input字符串
length长度

正则匹配数字：
var str="1 plus 2 equal 3"
str.match(/\d+/g)
// ['1','2','3']
length长度

var str="1 plus 2 equal 3"
var reg = /\d+/g
str.match(reg)

var str = "My name is CJ.Hello everyone!"; 
var re = /[A-Z]/;
var arr = str.match(re);
console.log(arr);
re = /[A-Z]/g; 
arr = str.match(re); 
console.log(arr);
re = /\b[a-z]*\b/gi;
str = "on e two three four"; 
str.match(re)
```

#### replace
可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配，并将其替换
$n 匹配第n个匹配正则表达式中的圆括号子表达式文本
$& 匹配正则表达式的子串
$` 匹配子串左边的文本
$' 匹配子串右边的文本
$$ 匹配美元符号
```
str ="z d l";
console.log(str.replace(/(\w)\s(\w)\s(\w)/, '$3 $2 $1'));
// l d z
```

```
str ="some some             \tsome\t\f";
re = /\s+/;
console.log(str.replace(re,"#"));
// some#some             	some	
re = /\s+/g;
console.log(str.replace(re,"@"));
// some@some@some@
```

### 更复杂的用法,使用子匹配 
```
// exec返回的数组第1到n元素中包含的是匹配中出现的任意一个子匹配  
re=/^[a-z]+\s+(\d+)$/i;// 用()来创建子匹配  
arr =re.exec(date); 
console.log(arr[0]);// 整个date,也就是正则表达式的完整匹配  
console.log(arr[1]);// 8,第一个子匹配,事实也可以这样取出主版本号  
console.log(arr.length);// 2  
date = "Ubuntu 8.10";// 取出主版本号和次版本号  
re = /^[a-z]+\s+(\d+)\.(\d+)$/i;// .是正则表达式元字符之一,若要用它的字面意义须转义  
arr = re.exec(date); 
console.log(arr[0]);// 完整的date  
console.log(arr[1]);// 8  
console.log(arr[2]);// 10  
```
### 匹配空行
注：
```
var date = ' \sUbuntu 8 ';
var reg = /\n[\s| ]*\r/g;
console.log(date.match(reg));
```
### 匹配首尾空格
注：
```
var date = 'Ubuntu 8 ';
var reg = /(^\s*)|(\s*$)/;
console.log(date.match(reg));
```



### 匹配整数
注：
```
var date = 'Ubuntu 8';
var reg = /^-?[1-9]\d*$/;
console.log(reg.test(date));
```
### 匹配负浮点数
注：
```
var date = 'Ubuntu 8';
var reg = /^-([1-9]\d*\.\d*|0\.\d*[1-9]\d*)$/;
console.log(reg.test(date));
```
### 匹配浮点数
注：
```
var date = '8';
var reg = /^-?([1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0)$/;
console.log(reg.test(date));
```
### 匹配非负浮点数
注：正浮点数 + 0
```
var date = '8';
var reg = /^[1-9]\d*\.\d*|0\.\d*[1-9]\d*|0?\.0+|0$/;
console.log(reg.test(date));
```
### 匹配非正浮点数
注：负浮点数 + 0
```
var date = '8';
var reg = /^(-([1-9]\d*\.\d*|0\.\d*[1-9]\d*))|0?\.0+|0$/;
console.log(reg.test(date));
```
### 输入字符串
注：由数字、26个英文字母或者下划线组成
```
var date = 'Ubuntu 8';
var reg = /^\w+$/;
console.log(reg.test(date));
```
### 空白行
注：可以用来删除空白行
```
var date = 'Ubuntu 8';
var reg = /\n\s*\r/;
console.log(reg.test(date));
```
### 常用正则表达式

### 两位小数
```
var date = '8.12';
var reg = /^[1-9]+\.{0,1}[0-9]{0,2}$/;
console.log(reg.test(date));
// true
var reg = /^[0-9]+(.[0-9]{2})?$/;
console.log(reg.test(date));
判断小数：
var reg = /^\d+\.\d+$/g;
```
### 任何数字
```
var date = '0888';
var reg = /^[0-9]+$/;
console.log(reg.test(date));
// true
判断纯数字：
var reg = /^\d+$/g;
```
### 指定位数的数字
```
var date = '888';
var reg = /^\d{3}$/;
console.log(reg.test(date));
// true
var date = '888';
var reg = /^\d{2}$/;
console.log(reg.test(date));
// false
```
### 至少n位的数字
```
var date = '888';
var reg = /^\d{3,}$/;
console.log(reg.test(date));
// true
var date = '88';
var reg = /^\d{3,}$/;
console.log(reg.test(date));
// false
```
### m~n位的数字
```
var date = '8888';
var reg = /^\d{3,5}$/;
console.log(reg.test(date));
// true
```
### 匹配非负整数
注：正确格式为：0 1 9 100
```
var date = '011';
var reg = /^(0|[1-9][0-9]*)$/;
console.log(reg.test(date));
// false
```
### 判断后缀
```
var date = 'Ubuntu.pdf';
var reg = /^.+\.pdf$/i;
console.log(reg.test(date));
```
### 匹配中文字符
```
var date = '京东方s';
var reg = /^[\u4e00-\u9fa5]{0,}$/;
console.log(reg.test(date));
// false
```
### 验证一个月的31天
注：正确格式为；"01"～"09"和"1"～"31"。 
```
var date = '01';
var reg = /^((0?[1-9])|((1|2)[0-9])|30|31)$/;
console.log(reg.test(date));
```
### 验证一年的12个月
注：正确格式为："01"～"09"和"1"～"12"
```
var date = '01';
var reg = /^(0?[1-9]|1[0-2])$/;
console.log(reg.test(date));
```
### 中国邮政编码
注：中国邮政编码为6位数字
```
var date = '223805';
var reg = /[1-9]\d{5}(?!\d)/;
console.log(reg.test(date));
```
### ip
注：提取ip地址时有用
```
var date = '192.168.0.1';
var reg = /^([1-9]|[1-9]\d|1\d\d|2[0-4]\d|25[0-5])(\.(\d|[1-9]\d|1\d\d|2[0-4]\d|25[0-5])){3}$/;
console.log(reg.test(date));

var date = '192.168.0.1';
var reg = /\d+\.\d+\.\d+\.\d+/;
console.log(reg.test(date));
```
### Email
注：
Email : /^\w+([-+.]\w+)*@\w+([-.]\\w+)*\.\w+([-.]\w+)*$/
isEmail1 : /^\w+([\.\-]\w+)*\@\w+([\.\-]\w+)*\.\w+$/;
isEmail2 : /^.*@[^_]*$/;
```
var date = 'luuman@qq.com';
var reg = /^\w+@\w+(\.(com|cn|net|org|edu)){1,2}$/g;
console.log(reg.test(date));

var date = 'luuman@qq.com';
var reg = /\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*/;
console.log(reg.test(date));
```
### 验证身份证号
注：15位或18位数字
```
var date = '320825196407065731';
var reg = /^\d{15}|\d{18}$/;
console.log(reg.test(date));
// true
```
### 验证帐号是否合法
注：字母、数字、下划线组成，字母开头，4-16位。
```
var date = 'Ubuntu8';
var reg = /^[a-zA-z]\w{3,15}$/;
console.log(reg.test(date));

var date = 'Ubuntu8';
var reg = /^[a-zA-Z][a-zA-Z0-9_]{3,15}$/;
console.log(reg.test(date));
```
### Phone手机号码
注：只有13、15和18开头的11位手机号码
```
var date = '18961856168';
var reg = /^((13|18)(\d{9}))$|^(14[57]\d{8})$|^(17[07]\d{8})$|(^15[0-35-9]\d{8}$)/;
// var reg = /^[1][358]\d{9}$/;
console.log(reg.test(date));
// true
```
### 网址
注：https、http
```
var date = 'https://zhidao.baidu.com/';
var reg = /^(http|https):\/\/[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&_~`@[\]\':+!]*([^<>\"\"])*$/;
var reg = /^http:// ([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$/;
console.log(reg.test(date));
```
### 电话号码
注：匹配形式如 0511-4405222 或 021-87888822区号-号码，区号以0开头，3位或4位
```
var date = '021-87888822';
var reg = /^((\d{4})-(\d{7}))$|^((\d{3})-(\d{8}))$|^((\d{4})-(\d{8}))$/;
console.log(reg.test(date));

var date = '0511-4405222';
console.log(reg.test(date));

var date = '0527-84405222';
console.log(reg.test(date));
```

[]( "")
[正则表达式的图形工具](https://regexper.com/ "")
[精通JS正则表达式](http:// www.cnblogs.com/aaronjs/archive/2012/06/30/2570970.html "newraina")
[JAVASCRIPT学习笔记之正则表达式](https://smohan.im/blog/3g3lh0 "")
[正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex-1.htm "")
[廖雪峰官网学习](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499503920bb7b42ff6627420da2ceae4babf6c4f2000 "")