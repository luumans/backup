title: React Native 组件
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

我们要想理解React Native应用的基本结构，我们首先需要先了解一些基本的React的概念，比如JSX语法、组件、state状态以及props属性。所以这篇我们重点讲讲Props，state和style样式。今天讲解的内容，都是根据React Native官方文档上的内容来的。


### 组件化：
组件的颗粒度设计主要取决于应用的结构设计。将公共部分拆分复用，提供公共组件。

导出组件Header：
```
module.exports = Header;
```

引入组件Header：
```
const Header = require('./header');
import Header from './header';

class luumans extends Component {
  render(){
    return (
      <Header></Header>
    )
  }
}
```



### View
AppRegistry模式是React Native中最基本的模块，也是最常用的模块。

```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  PixelRatio,
	  View,
	} from 'react-native';

	class luumans extends Component {
	  render(){
	    return (
	      <View style={styles.flexs}>
	        <View style={styles.container}>
	          <View style = {[styles.item,styles.center]}><Text style={styles.title}>酒店</Text>
	          </View>
	          <View style = {[styles.item,styles.lineLeftRight]}>
	            <View style={[styles.flexs,styles.center,styles.lineCenter]}><Text style={styles.title}>海外酒店</Text></View>
	            <View style={[styles.flexs,styles.center]}><Text style={styles.title}>特色酒店</Text></View>
	          </View>
	          <View style = {styles.item}>
	            <View style={[styles.flexs,styles.center,styles.lineCenter]}><Text style={styles.title}>团购</Text></View>
	            <View style={[styles.flexs,styles.center]}><Text style={styles.title}>客栈、公寓</Text></View>
	          </View>
	        </View>
	      </View>
	    )
	  }
	}

	const styles = StyleSheet.create({
	  container:{
	    flexDirection: 'row',
	    height: 80,
	    borderRadius: 5,
	    marginTop: 200,
	    marginLeft: 5,
	    marginRight: 5,
	    backgroundColor: '#FF0067',
	  },
	  item:{
	    flex: 1,
	    height: 80,
	  },
	  title:{
	    fontSize: 16,
	    fontWeight: 'bold',
	    color: '#FFF',
	  },
	  center:{
	    justifyContent: 'center',
	    alignItems: 'center',
	  },
	  flexs:{
	    flex: 1,
	  },
	  lineCenter: {
	    borderBottomWidth: 1/PixelRatio.get(),
	    borderColor: '#FFF',
	  },
	  lineLeftRight: {
	    borderLeftWidth: 1/PixelRatio.get(),
	    borderRightWidth: 1/PixelRatio.get(),
	    borderColor: '#FFF',
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

### Text

案例：
header.js
```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  PixelRatio,
	  Text,
	  View,
	} from 'react-native';

	class Header extends Component {
	  render(){
	    return (
	      <View style={styles.flexs}>
	        <Text style={styles.title}>
	          <Text style={styles.wangyi}>网易</Text>
	          <Text style={styles.xinwen}>新闻</Text>
	          <Text>有态度"</Text>
	        </Text>
	      </View>
	    )
	  }
	}

	const styles = StyleSheet.create({
	  flexs:{
	    marginTop: 25,
	    height: 50,
	    borderBottomWidth: 3/PixelRatio.get(),
	    borderBottomColor: '#EF2D36',
	    alignItems: 'center',
	  },
	  title:{
	    fontSize: 25,
	    fontWeight: 'bold',
	    alignItems: 'center',
	  },
	  wangyi:{
	    color: '#CD1D1C',
	  },
	  xinwen:{
	    color: '#FFF',
	    backgroundColor: '#CD1D1C',
	  },
	});

	module.exports = Header;
```
index.android.js
```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  PixelRatio,
	  View,
	} from 'react-native';

	// const Header = require('./header');
	import Header from './header';


	class List extends Component {
	  render(){
	    return(
	      <View style={styles.listItem}>
	        <Text style={styles.listItemFont}>{this.props.title}</Text>
	      </View>
	    );
	  }
	}

	class ImportantNews extends Component {
	  show(title){
	    alert(title);
	    console.log(title);
	  }
	  render(){
	    var news = [];
	    for(var i in this.props.news){
	      var text=(
	        <Text
	          onPress={this.show.bind(this,this.props.news[i])}
	          numberOfLines={1}
	          style={styles.newsItem}
	          key={i}
	        >{this.props.news[i]}</Text>
	      );
	      news.push(text);
	    }
	    return(
	      <View style={styles.flexs}>
	        <Text style={styles.newsTitle}>今日要闻</Text>
	        {news}
	      </View>
	    );
	  }
	}

	class luumans extends Component {
	  render(){
	    return(
	      <View style={styles.flexs}>
	        <Header></Header>
	        <List title='这些 Android 技术会很火'></List>
	        <List title='为什么整个互联网行业都缺前端工程师？'></List>
	        <List title='Android 开发中的日常积累'></List>
	        <List title='一个神奇的控件'></List>
	        <ImportantNews
	          news={[
	            '找到问题了 注解框架没有获取到控件id :sweat:',
	            '我之前也遇到过，可能是一个bug吧，不知道怎么解决',
	            '非常喜欢。准备看着你的打一遍，能看懂，但是自己就敲不出来了，谢谢分享',
	            '不知道怎么上的首页',
	          ]}>
	        </ImportantNews>
	      </View>
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexs:{
	    flex: 1,
	  },
	  listItem:{
	    height: 40,
	    marginLeft: 10,
	    marginRight: 10,
	    borderBottomWidth: 3/PixelRatio.get(),
	    borderBottomColor: '#DDD',
	    justifyContent: 'center',
	  },
	  listItemFont:{
	    fontSize: 16,
	  },
	  newsTitle:{
	    fontSize: 20,
	    fontWeight: 'bold',
	    color: '#CD1D1C',
	    marginLeft: 10,
	    marginTop: 10,
	  },
	  newsItem:{
	    fontSize: 15,
	    lineHeight: 40,
	    marginLeft: 10,
	    marginRight: 10,
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```

### Navigator

#### 属性

##### configureScene 
可选的函数，用来配置场景动画和手势。会带有两个参数调用，一个是当前的路由，一个是当前的路由栈。然后它应当返回一个场景配置对象
```
(route, routeStack) => Navigator.SceneConfigs.FloatFromRight
```
| 环境依赖： |
| -----|
| PushFromRight (默认)       | 
| FloatFromRight             | 
| FloatFromLeft              | 
| FloatFromBottom            | 
| FloatFromBottomAndroid     | 
| FadeAndroid                | 
| HorizontalSwipeJump        | 
| HorizontalSwipeJumpFromRight  | 
| VerticalUpSwipeJump        | 
| VerticalDownSwipeJump      | 

```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  PixelRatio,
	  Navigator,
	  Image,
	  View,
	  ScrollView,
	} from 'react-native';

	import Xinlang from './app/page/xinlang';

	class List extends Component {
	  constructor(props) {
	    super(props);
	    this.state = {};
	  }
	  _pressButton(){
	    const {navigator} = this.props;
	    //为什么这里可以取得 props.navigator?请看上文:
	    //<Component {...route.params} navigator={navigator} />
	    //这里传递了navigator作为props
	    if(navigator){
	      navigator.push({
	        name: 'Xinlang',
	        component: Xinlang,
	      })
	    }
	  }
	  render(){
	    return(
	      <ScrollView style={styles.flex}>
	        <Text style={styles.listItem} onPress={this._pressButton.bind(this)}>这些Android技术会很火</Text>
	        <Text style={styles.listItem} onPress={this._pressButton.bind(this)}>为什么整个互联网行业都缺前端工程师？</Text>
	        <Text style={styles.listItem} onPress={this._pressButton.bind(this)}>一个神奇的控件</Text>
	      </ScrollView>
	    );
	  }
	}
	class Detail extends Component {
	  constructor(props) {
	    super(props);
	    this.state = {};
	  }
	  _pressButton(){
	    const {navigator} = this.props;
	    if(navigator){
	      //很熟悉吧，入栈出栈~ 把当前的页面pop掉，这里就返回到了上一个页面:List了
	      navigator.pop();
	    }
	  }
	  render(){
	    return(
	      <ScrollView style={styles.flex}>
	        <Text style={styles.listItem} onPress={this._pressButton.bind(this)}>name</Text>
	      </ScrollView>
	    );
	  }
	}
	class luumans extends Component {
	  render(){
	    let defaultName = 'List';
	    let defaultComponent = List;
	    return (
	      <Navigator
	        initialRoute = {{name: defaultName, component: defaultComponent}}
	        //配置场景
	        configureScene = {
	          (route) => {
	            //这个是页面之间跳转时候的动画，具体有哪些？可以看这个目录下，有源代码的: node_modules/react-native/Libraries/CustomComponents/Navigator/NavigatorSceneConfigs.js
	            return Navigator.SceneConfigs.PushFromRight;
	          }
	        }
	        renderScene = {
	          (route,navigator) => {
	            let Component = route.component;
	            return <Component {...route.params} navigator = {navigator} />
	          }
	        }
	      />
	    );
	  }
	}

	const styles = StyleSheet.create({
	  flexs:{
	    flex: 1,
	  },
	  listItem:{
	    height: 40,
	    marginLeft: 10,
	    marginRight: 10,
	    borderBottomWidth: 3/PixelRatio.get(),
	    borderBottomColor: '#DDD',
	    justifyContent: 'center',
	    lineHeight: 30,
	    fontSize: 16,
	  },
	});

	AppRegistry.registerComponent('luumans', () => luumans);
```
### NavigatorIOS

### TextInput

#### 属性

##### placeholder

```
placeholder="Red placeholder text color"
```

##### placeholderTextColor

```
placeholderTextColor="red"
```

##### defaultValue

```
defaultValue="Same BackgroundColor as View "
```

```
```
##### 

```
```

##### 

```
```


##### 

```
```


##### 

```
```


[]( "")
