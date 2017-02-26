title: 前端代码规范Javascript
date: 2016-12-31 18:29:00
description: 
categories:
- Norm
tags:
- Javascript
toc: true
author: 
comments:
original:
permalink: 
---

　　**规范：**你是否常常碰到以下问题：你总是看不懂他写的代码，或者读起来很吃力；你需要改他的代码却无从下手，或总是要去问他这里是什么改了会不会影响其他代码；你和他一起开发一个产品，你总是怕代码和他有冲突或互相影响；你的代码在多次维护任务之后变得越来越臃肿，越来越难以维护。解决以上问题只需一种方法——读我们的规范！
<!-- more -->

## 命名规则
>JavaScript支持大小写
>统一使用单引号
>命名使用驼峰法则
>开头字母大写，表示对象。

### 变量命名

### 变量命名
1. 匈牙利命名：

开头字母用变量类型的缩写，其余部分用变量的英文或英文的缩写，要求单词第一个字母大写。

	For example: long lsum = 0;"l"是类型的缩写；
	s：表示字符串。例如：sName，sHtml；
	n：表示数字。例如：nPage，nTotal；
	b：表示逻辑。例如：bChecked，bHasLogin；
	a：表示数组。例如：aList，aGroup；
	r：表示正则表达式。例如：rDomain，rEmail；
	f：表示函数。例如：fGetHtml，fInit；
	o：表示以上未涉及到的其他对象，例如：oButton，oDate；
	g：表示全局变量，例如：gUserName，gLoginTime；

2. 驼峰式：

第一个单词首字母小写，后面其他单词首字母大写。

	For example:  firstName 

### 函数命名

1. 函数命名：统一使用动词或者动词+名词形式 ---- fnInit()

1. 对象方法命名使用fn+对象类名+动词+名词形式   fnAnimateDoRun() 

1. 某事件响应函数命名方式为fn+触发事件对象名+事件名或者模块名  fnDivClick()

附常用的动词列表：

| 名称 | 含义 | 名称 | 含义 |
| -----|:----:| -----|:----:|
| get | 获取 | set | 设置 |
| add | 增加 | remove | 删除 |
| create | 创建 | destory | 移除 |
| start | 启动 | stop | 停止 |
| open | 打开 | close | 关闭 |
| read | 读取 | write | 写入 |
| load | 载入 | save | 保存 |
| create | 创建 | destroy | 销毁 |
| begin | 开始 | end | 结束 |
| backup | 备份 | restore | 恢复 |
| import | 导入 | export | 导出 |
| split | 分割 | merge | 合并 |
| inject | 注入 | extract | 提取 |
| attach | 附着 | detach | 脱离 |
| bind | 绑定 | separate | 分离 |
| view | 查看 | browse | 浏览 |
| edit | 编辑 | modify | 修改 |
| select | 选取 | mark | 标记 |
| copy | 复制 | paste | 粘贴 |
| undo | 撤销 | redo | 重做 |
| insert | 插入 | delete | 移除 |
| add | 加入 | append | 添加 |
| clean | 清理 | clear | 清除 |
| index | 索引 | sort | 排序 |
| find | 查找 | search | 搜索 |
| increase | 增加 | decrease | 减少 |
| play | 播放 | pause | 暂停 |
| launch | 启动 | run | 运行 |
| compile | 编译 | execute | 执行 |
| debug | 调试 | trace | 跟踪 |
| observe | 观察 | listen | 监听 |
| build | 构建 | publish | 发布 |
| input | 输入 | output | 输出 |
| encode | 编码 | decode | 解码 |
| encrypt | 加密 | decrypt | 解密 |
| compress | 压缩 | decompress | 解压缩 |
| pack | 打包 | unpack | 解包 |
| parse | 解析 | emit | 生成 |
| connect | 连接 | disconnect | 断开 |
| send | 发送 | receive | 接收 |
| download | 下载 | upload | 上传 |
| refresh | 刷新 | synchronize | 同步 |
| update | 更新 | revert | 复原 |
| lock | 锁定 | unlock | 解锁 |
| check | out | 签出 |/check in 签入 |
| submit | 提交 | commit | 交付 |
| push | 推 | pull | 拉 |
| expand | 展开 | collapse | 折叠 |
| begin | 起始 | end | 结束 |
| start | 开始 | finish | 完成 |
| enter | 进入 | exit | 退出 |
| abort | 放弃 | quit | 离开 |
| obsolete | 废弃 | depreciate | 废旧 |
| collect | 收集 | aggregate | 聚集 |



附上一段代码细细品味

```
//类名
var ClassName = function(){
	//私有变量
	var _FieldName = "Test Field";
	//属性
	this.PropertyName = "Test Property Name";
	//私有方法
	var functionName = function(){
	return "";
}
A：加 _ 下划线前缀  

this.PublicFunctionName = function(pTestName){//公有方法 pTestName:参数
    //局部变量
    var condition = "condition";
    //判断
    if(condition){
        return functionName();
    }else{}
    B：小写开头
    //数组
    var nameCol = ["a","b"]; 
    //数组项
    var nameItem = nameCol[0]; 
    //循环
    for(var i = 0; i < nameCol.length; i++){
    }
    C:大写开头
    var selectName = "item";
    //选择
    switch(selectName){
        case "item":
        break;
    }
    D：加小写p前缀
}
```


### 匿名函数
要避免全局变量泛滥， 可以考虑使用匿名函数， 把不需要在外部访问的变量或者函数限制在一个比较小的范围内。

```
例如以下代码:
<script>
    function func1(){
        var list = ["a", "b", "c"];
        for(var i = 0; i < list.length; i++){
            //..
        };
    }
    func1(); //　自动运行
</script>
```

这段代码的作用是在页面加载的时候自动执行某些操作， 并不需要被外部调用， 但是它执行过后却留下了一个全局的函数。

```
像这种情况， 非常有必要使用匿名函数：
<script>
    (function func1(){
        var list = ["a", "b", "c"];
        for(var i = 0; i < list.length; i++){
            //..
        };
    })(); //　自动运行
</script>
```

### 匿名函数的格式：

```
(function(){
    // 代码块
})();
如果要带参数的话：
(function(arg1, arg2, argN){
    //..
})(arg1, arg2, argN);
```
 
### 命名空间
还有另外一个方法可以减少全局变量， 那就是命名空间， 在JS中可以用"对象-属性"来模拟命名空间；

```
window.com = {}
window.com.bytter = {}
window.com.bytter.hello = function(){
    alert("hello");
}
```

如果你要给某个已经存在的页面增加功能， 可能要增加十多个函数， 而那个页面已经存在大量的全局变量和函数， 甚至还有函数跟你新增的函数同名， 怎么办？
命名空间是比较好的选择， 你可以创建一个命名空间， 把你的新函数都放在那个命名空间之下， 例如：
```
<button type="button" onclick="pg.check.userAcc()">检查用户名</button>
<button type="button" onclick="pg.check.userAcc()">检查帐号</button>
<script>
    window.pg = {}
    window.pg.check = {
        // 检查用户名
        userName: function(){
            //..
        },
        // 检查帐号
        userAcc: function(){
            //..
        }
    };
</script>
```
### 互不干扰

结合上述的匿名函数和命名空间的使用， 可以把一个页面中自己维护的代码与其它的代码分隔开来， 将与外部代码的耦合降低到最小。

```
<script>
    // 页面命名空间
    window.pg = {}


    // 检查用户信息
    // 作者：张三
    // 最后更新： 2012.12.31
    (function(){

        // 私有变量
        var a, b, c;

        // 检查用户信息的相关方法
        window.pg.check = {
            // 检查用户名
            userName: function(){
                //..
            },
            // 检查帐号
            userAcc: function(){
                //..
            }
        };

    })(); // end 张三的代码


    // xxx功能
    // 作者：李四
    // 最后更新： 2012.1.1
    (function(){
        window.pg.xxx = {
            //..
        }
    })(); // end 李四的代码

</script>
```

### 变量声明
变量必须在使用前用var进行声明；
变量的声明应该放在代码块或者函数的头部；
可在一行内声明多个变量， 但要考虑美观易读；

```
// 银行名称, 银行帐号 
var type, acc;

// 银行名称, 银行帐号
var type = "中国银行", acc = "xxxxxx";
var type = "中国银行",   // 银行名称
    acc = "xxxxxx";      // 银行帐号
尽量使用易于理解的变量名，如：
var bankType = "中国银行",
    bankAccount = "xxxxxx"; 
### 命名
普通变量名和函数名采用"小驼峰式命名法"， 如：firstName、lastName
作为构造函数的函数采用"大驼峰式命名法"， 如：
var Person = function(){
    //..
}

var me = new Person();
常量用大写和下划线表示，如：
var ROOT_PATH = "/v10/";
分号
每条语句必须使用分号结尾（特别是需要压缩的js，省略分号常常会导致压缩失败）；
大括号
if语句、函数定义、try语句等带大括号的结构， 左大括号应紧跟前面的部分：
// good
var Person = function(){
    //..
}

// bad
var Person = function()
{
    //..
}
使用复合语句时不要省略大括号{}
// good
for(var i = 0; i < ary.length; i++){
    list.push(ary[i]);
}

// bad
for(var i = 0; i < ary.length; i++)
    list.push(ary[i]);
以提高代码可读性为前提，允许例外：
if(!data) return;

if(row) list.push(row);
```

### 空格
数值操作符(如, +/-/*/% 等)、比较符（大于、小于、等于）两边留空格；
逗号、冒号、分号后要留一个空格（如果后面还有内容的话）；
行尾不要有空格;
点号前后不要出现空格；
函数名末尾和左括号之间不要出现空格；
字符串
表示字符串用单引号或双引号均可， 建议统一使用双引号，
```
但表示html标签时一律使用单引号， 如：
var html = '<div class="msg" ></div>';
数组
使用简便的方式定义数组：
// good
var list = [1, 2, 3];

// bad
var list = new Array();
list[0] = 1;
list[1] = 2;
list[2] = 3;
```

## 注释规范

### 单行注释

	语法：
	// 这是单行注释

使用方式：
>1. 单独一行：//(双斜线)与注释文字之间保留一个空格。
1. 在代码后面添加注释：//(双斜线)与代码之间保留一个空格，并且//(双斜线)与注释文字之间保留一个空格。
1. 注释代码：//(双斜线)与代码之间保留一个空格。

示例：

```
// 调用了一个函数；1)单独在一行
setTitle();
var maxCount = 10; // 设置最大量；2)在代码后面注释
// setName(); // 3)注释代码
```

### 多行注释

	语法：/* 注释说明 */

使用方法：

>1. 若开始(/*)和结束(*/)都在一行，推荐采用单行注释。
1. 若至少三行注释时，第一行为/*，最后行为*/，其他行以*开始，并且注释文字与*保留一个空格。

示例：

```
/*
* 代码执行到这里后会调用setTitle()函数
* setTitle()：设置title的值
*/
setTitle();
```

### 函数(方法)注释

说明：函数(方法)注释也是多行注释的一种，但是包含了特殊的注释要求，参照 javadoc(百度百科)。

语法：

	/** 
	* 函数说明 
	* @关键字 
	*/

常用注释关键字：(只列出一部分，并不是全部)

示例：

| 注释名 | 语法 | 示例 |
|:----| -----| -----|
| @name    | @name 名称 | @name WacthClock |
| @author  | @author 作者 邮箱 | @author Luuman <luumans@qq.com> |
| @brief   | @brief  描述 | @brief this is watch for clock in canvas. |
| @dateTime| @dateTime 时间 | @dateTime 2016-11-27 |
| @moreInfo| @moreInfo 链接 | @moreInfo luuman.github.io/[link] |
| @version | @version XX.XX.XX | @version 1.0.3 |
| @param   | @param 名 {[type]}  描述信息 | @param name {String} 传入名称 |
| @return  | @return {[type]} 描述信息 | @return {Boolean} true:可执行;false:不可执行 |
| @example | @example 示例代码 | @example WacthClock({}); |

```
/**
 * @name     WacthClock              watch clock js
 * @author   Luuman                  <luumans@qq.com>
 * @brief    this is watch for clock in canvas.
 * @dateTime 2016-11-27
 * @moreInfo luuman.github.io/[link]
 * @version  1.0.0
 * @param    {[type]}
 * @param    {[type]}
 * @param    {[type]}
 * @return   {[type]}
 * @example WacthClock({});
 */
```
```
/**
* 合并Grid的行
* @param grid {Ext.Grid.Panel} 需要合并的Grid
* @param cols {Array} 需要合并列的Index(序号)数组；从0开始计数，序号也包含。
* @param isAllSome {Boolean} ：是否2个tr的cols必须完成一样才能进行合并。true：完成一样；false(默认)：不完全一样
* @return void
* @author polk6 2015/07/21 
* @example
* _________________                             _________________
* |  年龄 |  姓名 |                             |  年龄 |  姓名 |
* -----------------      mergeCells(grid,[0])   -----------------
* |  18   |  张三 |              =>             |       |  张三 |
* -----------------                             -  18   ---------
* |  18   |  王五 |                             |       |  王五 |
* -----------------                             -----------------
*/
function mergeCells(grid, cols, isAllSome) {
    // Do Something
}
```
 
[JavaScript 开发规范](http://www.cnblogs.com/polk6/p/4660195.html "")
[javascript学习笔记(一)基础知识](http://www.codeweblog.com/javascript%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0-%E4%B8%80-%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/ "")