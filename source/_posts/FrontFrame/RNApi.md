title: React Native API
date: 2016-12-27 18:29:00
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

## AppRegistry
AppRegistry模式是React Native中最基本的模块，也是最常用的模块。
AppRegistry模块是React Native应用JavaScript运行的入口。应用的跟组件应用使用AppRegistry.registerComponent进行注册自己。然后原生系统就可以进行加载运行bundle文件包，最后就会可以调用AppRegistry.runApplication进行运行起来应用。
当一个视图被摧毁的时候，为了结束应用可以调用AppRegistry.unmountApplictionComponentAtRootTag方法。其中该方法中的参数和runApplication中的要一样，该规则一定要遵守哦~

注：AppRegister模块需要在其他模块导入之前尽可能早点被导入进来来让JS环境可以正常运行。

### 属性方法
#### registerConfig(config:Array<AppConfig>)  static 静态方法, 进行注册配置信息

#### registerComponent(appKey:string,getComponentFunc:ComponentProvider)  static静态方法，进行注册组件

```
AppRegistry.registerComponent('luumans', () => luumans);
```

#### registerRunnable(appKey:string,func:Function)  static静态方法 ，进行注册线程

#### registerAppKeys()  static静态方法，进行获取所有组件的keys值

#### runApplication(appKey:string,appParameters:any)  static静态方法, 进行运行应用

#### unmountApplicationComponentAtRootTag()  static静态方法，结束应用

[]( "")
