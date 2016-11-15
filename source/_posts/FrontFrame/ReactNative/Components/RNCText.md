title: React Native Text
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

### Text
按官方文档的话来说，Text它也支持嵌套，样式和触摸处理。

#### Props属性

##### accessible bool
文本可以显示的行数
clip is working only for iOS

```
accessible={true}
```

##### numberOfLines number
文本可以显示的行数

```
numberOfLines={1}
```

##### onLayout function 
布局发生变化时调用

```
{nativeEvent: {layout: {x, y, width, height}}}
```

##### onLongPress function 
长按事件

```
onLongPress={this.increaseSize}>
```

##### onPress function
按下或者点击事件

```
onPress={() => console.log('1st')}
```


### 案例

#### 嵌套：
```
	import React, { Component } from 'react';
	import { AppRegistry, Text } from 'react-native';

	class TitleNested extends Component {
	  render() {
	    return (
	      <Text>
	        TitleNested地方
	        <Text style={{fontWeight: 'bold', fontSize: 20}}>
	          I am bold地方
	          <Text style={{color: 'red'}}>
	            and red地方
	          </Text>
	        </Text>
	      </Text>
	    );
	  }
	}

	AppRegistry.registerComponent('TitleNested', () => TitleNested);
```

#### 简书：
```
	'use strict';
	import React, { Component } from 'react';
	import {
	  AppRegistry,
	  StyleSheet,
	  Text,
	  View
	} from 'react-native';
	class TitleStyle extends Component {
	  render() {
	    return (
	      <View style={styles.container}>
	        <View style={styles.title_view}>
	          <Text style={styles.title_text}>简书</Text>
	        </View>
	        <Text numberOfLines={1} style={styles.content_title_text}>不想过低配的人生，请先看看这本书</Text>
	        <View style={styles.source_view}>
	          <Text style={styles.source_text}>余小鱼MsYu</Text>
	          <Text style={styles.source_text}>阅读7975</Text>
	        </View>
	          <Text accessibilityLabel={'Tap me!'} accessible={true} numberOfLines={2} style={styles.content_title_text}>我们熟悉的两种人生姿势：“飞黄腾达”和“赖在地上”。雾满拦江告诉你第三种：两脚不离大地，拼命向上生长。 ——《我不过低配的人生》</Text>
	          <Text style={styles.content_text}>作者是雾满拦江。乍一看，这名字很熟，似乎在哪里见过或听过，但具体不太了解。随即翻开简介，了解到：著名作家，“心学讲武堂”创始人，幽默写史领军人物。他写历史、职场，也写百态人情。其人特立独行、学识颇丰，其文辛辣生猛、犀利幽默，读之可以下酒。代表作有《神奇圣人王阳明》《别笑，这是大清正史》《民国就是这么生猛》《推背图中的历史》等。</Text>
	      </View>
	    );
	  }
	}
	const styles = StyleSheet.create({
	  container: {
	    flex: 1,
	    backgroundColor:'#FFF',
	  },
	   title_view:{
	    flexDirection:'row',
	    height:50,
	    justifyContent: 'center',
	    alignItems: 'center',
	    backgroundColor: '#E45E46',
	  },
	   title_text:{
	    color:'#FFF',
	    fontSize:20,
	    textAlign:'center'
	  },
	  source_view:{
	    flexDirection:'row',
	    height:20,
	    marginTop:10,
	    justifyContent: 'space-between',
	    alignItems: 'center',
	    backgroundColor:'#FFF',
	  },
	  source_text:{
	    color:'#b1b1b1',
	    fontSize:14,
	    marginLeft:25,
	    marginRight:25,
	    textAlign:'center'
	  },
	  content_title_text:{
	    color:'#343434',
	    fontSize:20,
	    marginTop:8,
	    marginLeft:25,
	    marginRight:25,
	    textAlign:'left'
	  },
	  content_text:{
	    color:'#b2b2b2',
	    fontSize:14,
	    lineHeight: 22,
	    marginTop:12,
	    marginLeft:25,
	    marginRight:25,
	    textAlign:'left'
	  },
	});
	AppRegistry.registerComponent('TitleStyle', () => TitleStyle);
```

#### 新浪：
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

<!-- #### StyleSheet样式
##### color color
##### fontFamily ReactPropTypes.string
##### fontSize ReactPropTypes.number
##### fontStyle ReactPropTypes.oneOf(['normal', 'italic'])
##### fontWeight ReactPropTypes.oneOf(
	['normal' /*default*/, 'bold','100', '200', '300', '400', '500', '600', '700', '800', '900']
)
##### Specifies font weight. The values 'normal' and 'bold' are supported for most fonts. Not all fonts have a variant for each of the numeric values, in that case the ##### closest one is chosen.

##### lineHeight ReactPropTypes.number
##### textAlign ReactPropTypes.oneOf(
	['auto' /*default*/, 'left', 'right', 'center', 'justify']
)
##### Specifies text alignment. The value 'justify' is only supported on iOS and fallbacks to left on Android.

##### textDecorationLine ReactPropTypes.oneOf(
	['none' /*default*/, 'underline', 'line-through', 'underline line-through']
)
##### textShadowColor color
##### textShadowOffset ReactPropTypes.shape(
	{width: ReactPropTypes.number, height: ReactPropTypes.number}
)
##### textShadowRadius ReactPropTypes.number
##### androidtextAlignVertical ReactPropTypes.oneOf(
	['auto' /*default*/, 'top', 'bottom', 'center']
)
##### iosfontVariant ReactPropTypes.arrayOf(
	ReactPropTypes.oneOf([
	  'small-caps',
	  'oldstyle-nums',
	  'lining-nums',
	  'tabular-nums',
	  'proportional-nums',
	])
)
##### iosletterSpacing ReactPropTypes.number
##### iostextDecorationColor color
##### iostextDecorationStyle ReactPropTypes.oneOf(
	['solid' /*default*/, 'double', 'dotted','dashed']
)
##### ioswritingDirection ReactPropTypes.oneOf(
	['auto' /*default*/, 'ltr', 'rtl']
) -->
