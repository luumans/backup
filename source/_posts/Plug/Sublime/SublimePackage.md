title: Sublime插件
date: 2015-12-21 18:29:10
description: 来到GitHub这么长时间，才开始真真的了解GitHub，这个国外的代码托管平台，充满着大牛的身影。
categories:
- Plug
tags:
- Sublime
toc: true
author:
comments:
original:
permalink:
---
　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->
### 安装有两个办法：
>1、直接把插件放到它的安装路径对应文件包packages里面去，不知道在哪的可以直接打开 preferences->Browse packages，放进去之后软件会自动检测。

>2、使用命令方式直接让软件自己下载安装。（使用package control组件）（前提：先安装下面那个package control插件)

>按下Ctrl+Shift+P调出命令面板，输入install， 调出 Install Package 选项并回车，然后在列表中选中要安装的插件。

>下载拷贝：然后把它放到package文件包中。没用过Github的朋友不知道在哪里下载。Download ZIP。然后把它解压，把文件夹放进package文件包，然后它就能检测到包啦！

>代码安装：Ctrl+shift+p、输入install、选择package install 过几秒会弹出另一个框。然后在输入框中输入你想要的插件关键字安装吧！大致就是这样，简单明了。下面介绍其他常用插件，安装方式同理！

### package control（包裹组件）
通过快捷键 ctrl+` 或者 View > Show Console 菜单打开控制台，然后粘贴对应的版本代码进去，然后回车让它安装。

适用于 Sublime Text 3：

```
import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
适用于 Sublime Text 2：

```
import urllib2,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler()) ); by = urllib2.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); open( os.path.join( ipp, pf), 'wb' ).write(by) if dh == h else None; print('Error validating download (got %s instead of %s), please try manual install' % (dh, h) if dh != h else 'Please restart Sublime Text to finish installation')
```
当然了，有些时候这样你安装不进去的，就只能自个去下载安装包放到package文件里边去了（就是上边我说的那个文件夹)如果在Preferences → Package Settings 中看到 Package Control 这一项，说明安装成功。


# 插件整理

## 代码整理：

### Tag（代码格式化）
全选Ctrl+A，Ctrl+Alt+F

### HTMLBeautify（）
格式化HTML。

### HTML/CSS/JS Prettify（代码格式化）
能够格式化css html和js。
注意：格式化的文件路径中不能有中文，不然会报找不到node的错误（windows下）。

### YUI Compressor（压缩JS和CSS文件） 
### PHPTidy（整理与排版PHP代码）
### JsFormat（JS格式化） 
很多网站的JS代码都进行了压缩，一行式的甚至混淆压缩，这让我们看起来很吃力。而这个插件能帮我们把原始代码进行格式的整理，包括换行和缩进等等，是代码一目了然，更快读懂

```bash
Ctrl+Alt+F JS格式化
```
## 注释：

### DocBlockr（代码自动注释生成） 
标准的注释，包括函数名、参数、返回值等，并以多行显示，手动写比较麻烦

```bash
输入/*、/**然后回车，还有很多用法，请参照
```
### HtmlTidy（清理与排版你的HTML代码）
### AutoPEP8（）
格式化Python代码。

### Alignment安装案例
### Alignment（代码补齐）补齐就是补齐~就像这样

## 代码简写：

### Emmet（Zen Coding 代码自动补齐） [Wiki](http://docs.emmet.io/cheat-sheet/ "Cheat Sheet")
Emmet作为zen coding的升级版，对于前端来说，可是必备插件。

```bash
ctrl+alt+enter：激发zencoding控制台
```

```bash
div#content>ul>li*3>a[href="javascript:void(0);"]{Links$}
<div id="content">
  <ul>
    <li><a href="javascript:void(0);">Links1</a></li>
    <li><a href="javascript:void(0);">Links2</a></li>
    <li><a href="javascript:void(0);">Links3</a></li>
  </ul>
</div>

div.wrapper>div.header+div.main+div.footer：按下Tab，立刻变成
<div class="wrapper">
  <div class="header"></div>
  <div class="main"></div>
  <div class="footer"></div>
</div>
```

### SublimeCodeIntel（代码提示）
自动提示我们，可能要输入HTML代码内容

### snippets（自定制代码补齐机制）
自定制代码补齐机制，


## 高亮显示：

### BracketHighlighter
BracketHighlighter高亮显示匹配的括号、引号和标签，BracketHighlighter这个插件能在左侧高亮显示匹配的括号、引号和标签，能匹配的### ] ,  () ,  {} ,  "" ,  '' , <tag></tag>等甚至是自定义的标签，当看到密密麻麻的代码分不清标签之间包容嵌套的关系时，这款插件就能很好地帮你理清楚代码结构，快速定位括号，引号和标签内的

### TrailingSpacer（高亮显示多余的空格和Tab）

## 颜色：

### ColorPicker （调色盘） 

```bash
ctrl+shift+c：需要输入颜色时，可直接选取颜色
```

## CSS：

### CSScomb（CSS属性排序） 
### CSS3_Syntax（css语法高亮）
对css语法高亮的支持，view-syntax-css3选中css3就能使用css3高亮了。必须每条属性都加上分号，并且属性必须小写，不然不会高亮，有点鸡肋啊。

### Prefixr（自动加-webkit前缀）
写 CSS可自动添加 -webkit 等私有词缀，Ctrl+Alt+X触发。

### Autoprefixer（自动加前缀）
可以给css自动加前缀功能

### Goto-CSS-Declaration（CSS文件跳转）
跳转到css文件该class的声明处，方便修改查看，如图下所示，注意对应的css文件要同时打开才行。

## 编码：

### GBK Encoding Support（GBK中文编码）
这个插件还是非常有用的，因为sublime 本身 不支持gbk编码，在utf8如此流行的今天，我们整站还是gbk编码，o(︶︿︶)o 唉，所以全靠这个插件了。

### GBK to UTF8（编码转换）
将文件编码从GBK转换成UTF8，菜单 – File里面找。


## 文档管理：

### AdvancedNewFile（新建文件）

```bash
Ctrl+Alt+N：按路径自动创建文件
```
### AutoFileName（文件名索引）
快捷输入文件名，输入”/”即可看到相对于本项目文件夹的其他文件。

### GotoRecent（历史文档记录）
打开最近的文件，系统有这个功能，但只能看最近8个，有点不爽，按ctrl+e，选择即可。

```bash
ctrl+e：
```

### Nettus+ fetch （管理一些开源框架）
配置文件修改，Ctrl+Shift+P输入Fetch Manage，配置文档。通过输入fetch file，搜索框架名进行导入。

### SideBarEnhancements（侧边栏增强） 
### SyncedSideBar（侧边栏打开文件定位）
支持当前文件在左侧面板中定位，不错。

### Hex-to-HSL-Color Hex（颜色模式转HSL颜色模式）
### advanceNewfile（面板随意添加文件）
按Ctrl+Alt+N，下方输入A\B\test.css就好了，test.css这个文件出现在某个文件夹。

### SublimeTmpl （自定义新建文件） 
默认已经添加了html、css、js等常见类型的面板，按ctrl+alt+h/ctrl+alt+c/ctrl+alt+j可新建这 3钟类型的文件，快捷键在这里\Packages\SublimeTmpl\Default (Windows).sublime-keymap, 模板文件在这里\Packages\SublimeTmpl\templates，可修改。 比如下边简单的html文件


## 语法识别：

### LESS（LESS语法识别）
这是一个非常棒的插件，可以让sublime支持less的语法高亮和语法提示，对于搞less的同学灰常重要，不过多解释。

### SCSS（SCSS语法识别）
支持scss的语法高亮，里面附带了好多CSS Snippet，无论现用或者改造成，都可节省不少时间。

### ejs（ejs语法识别）
支持ejs的语法高亮，里面附带了好多CSS Snippet，无论现用或者改造成，都可节省不少时间。

### node（node语法识别）
支持node的只能语法提示，很赞。

### jQuery（jQuery语法识别）
支持jquery的只能语法提示，很赞。

### JavaScriptNext - ES6 Syntax（ES6语法识别）
提供ES6的语法支持。

### WordPress（WordPress的函数）
集成一些WordPress的函数，对于像我这种经常要写WP模版和插件的人特别有用

### Vintage（Vim模拟）
如果你习惯使用vim，那么可以安装这个插件，这个插件可以让sublime像vim一样。


### Liquid（Liquid语法识别）
提供Liquid语法支持，如果你也写博客的话不妨试试。

### Smarty（Smarty语法识别）
提供smarty语法的支持。Smarty插件默认的分隔符是{}，如果你使用的分隔符不同可以更改插件目录的Smarty.tmPreferences文件，找到其中的SMARTY_LDELIM和SMARTY_RDELIM，修改为你的分隔符即可。


## 文件传输：

### git
* 最后推荐一个同步插件，这个插件可以在不同的机器同步配置信息和插件，非常方便，但鉴于国内的墙太高，我都是直接把插件给手动备份了，然后直接拖进去，或者直接去github上下载对应的包。
```Ctrl+Shift+p```
<!-- 昨天安装过Android Studio，之后就出现PATH问题。 -->

> status

```bash
Git: status：查看改动的文件
```
> add

```bash
Git: add all：添加所有文件
Git: add ...：添加制定文件
Git: add
```
> commit

```bash
Git: commit：提交描述
Git: commithistroy：提交历史描述
```
将会展示提交内容修改了那些内容。
ctrl+w：关闭文件的同时，commit操作自动触发。

### SFTP（编辑 FTP 或 SFTP 服务器上的文件）
### Package Syncing



## 其他：

### OmniMarkupPreviewer

> Windows, Linux:

```bash
Ctrl+Alt+O: Preview Markup in Browser.
Ctrl+Alt+X: Export Markup as HTML.
Ctrl+Alt+C: Copy Markup as HTML.
```
> OSX:

```bash
commond+control+O: Preview Markup in Browser.
commond+control+X: Export Markup as HTML.
Ctrl+Alt+C: Copy Markup as HTML.
```

### FileDiff（代码对比）
右键标签页，出现FileDiffs Menu或者Diff with Tab…选择对应文件比较即可

### Bracket Highlighter（代码匹配）
可匹配[], (), {}, “”, ”, <tag></tag>，高亮标记，便于查看起始和结束标记

### Clipboard-history（粘贴板历史记录） 
可以保存粘贴的历史，很赞的功能，再也不用担心历史丢失了。ctrl+alt+v可打开历史面板，上下选择即可，安装后会和默认的ctrl+shift+v（粘贴缩进）冲突，干掉这个快捷键。

```bash
Ctrl+alt+v：显示历史记录
Ctrl+alt+d：清空历史记录
Ctrl+shift+v：粘贴上一条记录（最旧）
Ctrl+shift+alt+v：粘贴下一条记录（最新）
```

### CTags（函数跳转）
需要安装[CTags](http://ctags.sourceforge.net/ "DownLoad")

### Terminal（终端）

```bash
Ctrl+Shift+T：增加Open Terminal Here
```

```
{
  // window下终端路径
  "terminal": "E:/Program Files/Git/git-bash.exe",
  // "terminal": "C:\\Program Files\\cmder_mini\\cmder.exe",
  // "parameters": ["/START", "%CWD%"]
  //  window下终端参数
  "parameters": []
  // xterm on GNU/Linux
  // "terminal": "xterm"
  // iTerm on OS X
  // "terminal": "iTerm.sh"
  // iTerm on OS X with tabs
  // "terminal": "iTerm.sh",
  // iTerm2 v3 on OS X
  // "terminal": "iTerm2-v3.sh"
}
```


### Gits（集成 GitHub） 
### lint（语法校验）：
### SublimeLinter（代码错误提示） 总体架构
### Jslint编程风格

### Tradsim（中文繁字体和简体字转换） 
### Terminal
可以sublime中，打开命令行，非常方便哦。

### AllAutocomplete
自动完成插件，可在全部打开的文件中，自动完成。

### HexViewer
提供十六进制文件查看功能。

### MultiEditUtils
扩展多行编辑的功能。

### IMESupport（输入框不更随着光标）


## 主题（Themes）
Sublime Text有大量第三方主题：ayu Theme、sublime material

### SublimeTextTrans（背景透明度）[DownLoad](https://github.com/vhanla/SublimeTextTrans "SublimeTextTrans GIT")

```bash
Ctrl+Shift+1-6 执行调节背景透明度。
```

### SideBarFolders
打开的文件夹都太多了，再用这个来管理文件夹


### Sublime Merger
```bash
```

[]( "")
