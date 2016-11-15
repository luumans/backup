title: React Native Android环境搭建（IOS）
date: 2016-12-25 18:29:00
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
## Android环境搭建
| 环境依赖： |
| -----|
| Git    |
| Node    |
| Python 2    |
| Android Studio    |
| react-native-cli    |
| Microsoft C++ 环境    |
| android 6.0 真机   |

### 安装java JDk
从Java官网下载[Java SE Development Kit 7 Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)并安装。请注意选择x86还是x64版本。推荐将JDK的bin目录加入系统PATH环境变量。（安装JDK、JRE）

#### 配置环境变量：
> 系统变量→新建 JAVA_HOME 变量
	
```bash
C:\Program Files\Java\jdk1.7.0_79
```
> 系统变量→寻找 Path 变量→编辑
	
```bash
%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin;
```
> 系统变量→新建 CLASSPATH 变量→编辑
	
```bash
.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar
```
> java -version 查看Java版本


### 安装Python 2
从官网下载并安装[python 2.7.x](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000 "Python 2.7教程 - 廖雪峰的官方网站")（3.x版本不行）

### Android Studio
[Android Studio](http://www.android-studio.org/index.php/download)