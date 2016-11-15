title: React Native IOS环境搭建
date: 2016-12-25 19:29:00
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
## React Native初探
<!-- learn once,write everywhere! -->
ReactNative是Facebook在2015年React开发者大会上公开的应用开发框架，一个可以用React开发原生应用的框架。

### 技术背景
FaceBook => HTML5、NativeApp
HybridApp => Native + Web 混合模式

### 特点：
JSX语法（扩展的JS语法）、组件化模式、Virtual Dom、DataBinding单向数据流、可以实现Chrome的调试

### 基本模式：
每个React应用可视为组件的组合，而每个React组件由属性（Property）和状态（state）来配置，当状态发生变化时更新UI，组件的结构是由虚拟的DOM来维护，确保了实际更新的DOM只包括真正产生了状态变化的部分。

### 同类型的代码：
GoogleSky、Titanium、NativeScript（太重）、鸟巢（支付宝）、BeeFrameWork
综合起来：强大的社区，简单的学习，简单的开发、简单的应用。


### 跨平台开发框架

优点；
1. 跨平台、兼容web、ios、android三大主流平台
1. React调用原生控件，性能优于H5框架
1. 更好的手势识别
1. 实时部署更新，再也不用担心应用市场审核缓慢

设计理念：既拥有Native的用户体验，又保留React的开发效率！

Facebook官方使用React Native开发的应用：Groups、Ads Manager、F8、Adverts Manger、天猫IPad、Chinese Flashcards

### ReactNative提供了那些能力
1. 基于原生UI组件
1. 手势识别
1. 基于FlexBox的CSS布局模式
1. 跨平台开发
1.  基于React、jsx组件化开发模式

## IOS环境搭建

| 环境依赖： |
| -----|
| OSX    |
| Xcode    |
| Node    |
| sublime    |

### 安装Homebrew：
Homebrew, Mac系统的包管理器，用于安装NodeJS和一些其他必需的工具软件。通过homebrew安装Node、watchman、flow

#### 安装[Homebrew](http://brew.sh/ "Homebrew官网")：
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

注：在Max OS X 10.11（El Capitan)版本中，homebrew在安装软件时可能会碰到/usr/local目录不可写的权限问题。可以使用下面的命令修复：

```bash
sudo chown -R `whoami` /usr/local
```

#### 查看是否安装homebrew
```
brew -v
```

### Node
```bash
brew install node
```

### Watchman
Watchman是由Facebook提供的监视文件系统变更的工具。安装此工具可以提高开发时的性能（packager可以快速捕捉文件的变化从而实现实时刷新）。
```bash
brew install watchman 检测文件变化
```

### Flow
Flow是一个静态的JS类型检查工具。译注：你在很多示例中看到的奇奇怪怪的冒号问号，以及方法参数中像类型一样的写法，都是属于这个flow工具的语法。这一语法并不属于ES标准，只是Facebook自家的代码规范。所以新手可以直接跳过（即不需要安装这一工具，也不建议去费力学习flow相关语法）。
```bash
brew install flow 检测js语法
```

```bash
clear 清理屏幕
```


### React Native
React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务
```bash
npm install -g react-native-cli 全局安装react native
```
如果你看到EACCES: permission denied这样的权限报错，那么请参照上文的homebrew译注，修复/usr/local目录的所有权：
```bash
sudo chown -R `whoami` /usr/local
```

