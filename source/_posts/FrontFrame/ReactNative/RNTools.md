title: React Native开发工具
date: 2016-12-28 18:29:00
description: 
categories:
- FrontFrame
tags:
- ReactNative
toc: true
author:
comments:
original:
permalink: 
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->

## Sublime 3

Reactnative中没有DOM的概念，只有组件的概念，所以我们HTML标签、DOM操作是无效的。但是组件的生命周期、JSX语法、事件绑定、自定义属性等，ReactNative与React.js是一样的。

### [ReactJS](https://github.com/facebookarchive/sublime-react "")

### [Emmet]( "Ctrl + E")

```
View>Text

<View>
  <Text></Text>
</View>
```

### [Terminal]( "")
上面添加了Terminal插件，在sublime里，直接用快捷键 command+shift+T，打开终端，然后执行如下命令运行 Android 应用程序：

### [react-native-snippets](https://github.com/Shrugs/react-native-snippets "")

### [Babel](https://github.com/babel/babel-sublime "")
babel插件支持ES6语法和JSX语法，要比sublime-react看起来更舒服。出现问题也会提示。

安装：
搜索“Babel”，安装后将jsx文件格式设置成（Syntax -> Open all with current extension as... -> Babel -> JavaScript (Babel)）