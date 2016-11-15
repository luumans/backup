title: React Native Flexbox
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
ReactNative看起来和react.js很像，最近也有不少框架开源，Weex基于Vue.js。ReactNative通过自定义组件，来实现接近原生操作，实现开平台效果。

## Flexbox布局
![](http://www.th7.cn/d/file/p/2016/08/30/532c0a8f1bc8b3d4037a61d7efc61d36.jpg)
### 什么事Flexbox
Flexbox是css 3中引入的布局模型“弹性盒子模型”，通过弹性的方式来对齐和分布容器中的内容空间，使其能够适应不同屏幕的宽度。React Native中Flexbox是这个规范的子集。

### 解决问题
浮动布局
不同宽度屏幕的适配
宽度自动分配
水平垂直居中

### Flexbox模型
![](http://img.caibaojian.com/uploads/2014/05/flexbox.png)
一系列属性的集合：
基本上，项目将制定了以下任一主轴（从 main-start 到 main-end）或十字轴（(从 cross-start 到 cross-end）。

### Flexbox属性讲解

| 容器属性 | 元素属性 |
|   -----  |   -----  |
| flexDirection     | flex |
| flexWrap          | alignSelf |
| alignltems        | |
| justifyContent    | |

#### [flexDirection](http://caibaojian.com/demo/flexbox/flex-direction.html "演示")(适用于父类容器的元素上)
![](http://ww1.sinaimg.cn/mw690/660d0cdfjw1etlhxise4kj218g0xc0vf.jpg)
```bash
flexDirection : row | row-reverse | column | column-reverse（好像不支持）
row：横向布局
column：纵向布局（默认）
```
 
> row：横向布局

```javascript
  import React, { Component } from 'react';
  import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
  } from 'react-native';

  export default class luumans extends Component {
    render() {
      return (
        <View style={styles.flexContainer}>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row1</Text>
          </View>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row2</Text>
          </View>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row3</Text>
          </View>
        </View>
      );
    }
  }

  const styles = StyleSheet.create({
    flexContainer: {
      flexDirection: 'row',
      backgroundColor: '#aaaaaa',
    },
    cell: {
      height: 50,
    },
    welcome: {
      fontSize: 14,
      textAlign: 'center',
      width: 80,
      margin: 10,
      height: 30,
      color: '#FFF',
      backgroundColor: '#000',
    },
  });

  AppRegistry.registerComponent('luumans', () => luumans);
```

> column：纵向布局（默认）

```javascript
  import React, { Component } from 'react';
  import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
  } from 'react-native';

  export default class luumans extends Component {
    render() {
      return (
        <View style={styles.flexContainer}>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row1</Text>
          </View>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row2</Text>
          </View>
          <View style={styles.cell}>
            <Text style={styles.welcome}>Row3</Text>
          </View>
        </View>
      );
    }
  }

  const styles = StyleSheet.create({
    flexContainer: {
      flexDirection: 'column',
      backgroundColor: '#aaaaaa',
    },
    cell: {
      height: 50,
    },
    welcome: {
      fontSize: 20,
      textAlign: 'center',
      margin: 10,
      color: '#FFF',
      backgroundColor: '#000',
    },
  });

  AppRegistry.registerComponent('luumans', () => luumans);
```

#### [flex-wrap](http://caibaojian.com/demo/flexbox/flex-wrap.html "演示")
![](http://ww1.sinaimg.cn/mw690/660d0cdfjw1etlhxjtkfwj218g0xcwhp.jpg)
```bash
flex-wrap: nowrap | wrap

nowrap：当子元素溢出父容器时不换行。
wrap：当子元素溢出父容器时自动换行。
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
  import React, { Component } from 'react';
  import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
  } from 'react-native';

  export default class luumans extends Component {
    render() {
      return (
        <View style={styles.flexContainer}>
            <Text style={styles.welcome}>Row1</Text>
            <Text style={styles.welcome}>Row2</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
            <Text style={styles.welcome}>Row3</Text>
        </View>
      );
    }
  }

  const styles = StyleSheet.create({
    flexContainer: {
      flexDirection: 'row',
      flexWrap: 'wrap',
      backgroundColor: '#aaaaaa',
    },
    
    welcome: {
      fontSize: 14,
      textAlign: 'center',
      width: 80,
      margin: 10,
      height: 30,
      color: '#FFF',
      backgroundColor: '#000',
    },
  });

  AppRegistry.registerComponent('luumans', () => luumans);
```

#### [alignltems](http://caibaojian.com/demo/flexbox/align-items.html "演示")
![](http://ww4.sinaimg.cn/mw690/660d0cdfjw1etlhxkrusyj218g0xcwhn.jpg)
```bash
align-items: flex-start | flex-end | center | stretch

flex-start：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。
flex-end：弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。
center：弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。
stretch：如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。
```

> center：

```javascript
  import React, { Component } from 'react';
  import {
    AppRegistry,
    StyleSheet,
    Text,
    View,
  } from 'react-native';

  export default class luumans extends Component {
    render() {
      return (
        <View style={styles.flexContainer}>
            <Text style={styles.welcome}>Row1</Text>
            <Text style={styles.welcome2}>Row2</Text>
            <Text style={styles.welcome3}>Row3</Text>
        </View>
      );
    }
  }

  const styles = StyleSheet.create({
    flexContainer: {
      flexDirection: 'row',
      flexWrap: 'wrap',
      backgroundColor: '#aaaaaa',
      alignItems: 'center',
    },
    
    welcome: {
      fontSize: 14,
      textAlign: 'center',
      width: 80,
      margin: 10,
      height: 30,
      color: '#FFF',
      backgroundColor: '#000',
    },
    welcome2: {
      fontSize: 14,
      textAlign: 'center',
      width: 80,
      margin: 10,
      height: 40,
      color: '#FFF',
      backgroundColor: '#000',
    },
    welcome3: {
      fontSize: 14,
      textAlign: 'center',
      width: 80,
      margin: 10,
      height: 50,
      color: '#FFF',
      backgroundColor: '#000',
    },
  });

  AppRegistry.registerComponent('luumans', () => luumans);
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
```

> wrap：当子元素溢出父容器时自动换行。

```javascript
```
#### [flex-flow](http://caibaojian.com/demo/flexbox/flex-flow.html "演示")
![]()

#### [justify-content](http://caibaojian.com/demo/flexbox/justify-content.html "演示")
![](http://ww3.sinaimg.cn/mw690/660d0cdfjw1etlhxjven9j218g0xcgp4.jpg)


#### [align-content](http://caibaojian.com/demo/flexbox/align-content.html "演示")
![](http://ww4.sinaimg.cn/mw690/660d0cdfjw1etlhxl3605j218g0xcwip.jpg)

#### [order](http://caibaojian.com/demo/flexbox/order.html "演示")
![]()

#### [flex-grow](http://caibaojian.com/demo/flexbox/flex-grow.html "演示")
![]()

#### [flex-shrink](http://caibaojian.com/demo/flexbox/flex-shrink.html "演示")

#### [flex-basis](http://caibaojian.com/demo/flexbox/flex-basis.html "演示")

#### [flex](http://caibaojian.com/demo/flexbox/flex.html "演示")

#### [align-self](http://caibaojian.com/demo/flexbox/align-self.html "演示")

#### [flexbox3](http://caibaojian.com/demo/flexbox/flexbox3.html "演示")

#### [flexbox4](http://caibaojian.com/demo/flexbox/flexbox4.html "演示")

#### [flexbox5](http://caibaojian.com/demo/flexbox/flexbox5.html "演示")