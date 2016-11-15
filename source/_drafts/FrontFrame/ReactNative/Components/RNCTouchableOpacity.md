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

```
import React, { Component } from 'react';
import {
 AppRegistry,
  StyleSheet,
  Text,
  Image,
  ScrollView,
  TouchableOpacity,
  TouchableHighlight,
  TouchableNativeFeedback,
  View
} from 'react-native';
class luumans extends Component {
  render() {
    let PoperImg = ({
      uri: 'https://avatars0.githubusercontent.com/u/17568866?v=3&u=89c7e2490a5b2bdeef3fac922bf78a6fd80d2e09&s=140'
    });
  return (
    <View style={styles.container}>
     <View style={styles.title_view}>
     <Text style={styles.title_text}>
         空间动态
     </Text>
    </View>
    <ScrollView  ref={(scrollView) => { _scrollView = scrollView; }}>
    <View style={styles.three_image_view}>
     <View style={styles.vertical_view}>
        <Image source={PoperImg} style={{alignSelf:'center',width:45,height:45}} />
        <Text style={styles.top_text}>
        好友动态
        </Text>
     </View>
      <View style={styles.vertical_view}>
        <Image source={PoperImg} style={{alignSelf:'center',width:45,height:45}}/>
        <Text style={styles.top_text}>
        附近
        </Text>
     </View>
      <View style={styles.vertical_view}>
        <Image source={PoperImg} style={{alignSelf:'center',width:45,height:45}}/>
        <Text style={styles.top_text} >
        兴趣部落
        </Text>
     </View>
    </View>
    <View style={{height:30,backgroundColor:'#f9f9fb'}}/>
    <TouchableHighlight 
      underlayColor="blue"
      activeOpacity={0.5}
      onPress={()=>{
      console.log('我被点击了');
      }}
    >
      <View style={styles.rectangle_view}>
        <View style={{flexDirection:'row',alignItems: 'center'}}>
          <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
          <Text style={styles.rectangle_text} >
          羽毛球
          </Text>
        </View>
        <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
      </View>
     </TouchableHighlight>
     <TouchableNativeFeedback 
      background={TouchableNativeFeedback.SelectableBackground()}
      onPress={()=>{
      console.log('我被点击了');
      }}
     >
      <View style={styles.rectangle_view}>
        <View style={{flexDirection:'row',alignItems: 'center'}}>
          <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
          <Text style={styles.rectangle_text} >
          火车票
          </Text>
        </View>
        <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
      </View>
     </TouchableNativeFeedback>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        羽毛球
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        火车票
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        羽毛球
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        火车票
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        羽毛球
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        火车票
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        羽毛球
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        火车票
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        羽毛球
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        火车票
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     <View style={styles.rectangle_view}>
      <View style={{flexDirection:'row',alignItems: 'center'}}>
        <Image source={PoperImg} style={{alignSelf:'center',width:30,height:30}}/>
        <Text style={styles.rectangle_text} >
        视频
        </Text>
      </View>
      <Image source={PoperImg} style={{alignSelf:'center',width:20,height:20}}/>
     </View>
     </ScrollView>
     <TouchableOpacity
      activeOpacity={0.2}
      style={styles.button}
      onPress={() => { _scrollView.scrollTo({y: 0}); }}>
      <Text>让我滚回去(注意看按压时透明度的改变)</Text>
    </TouchableOpacity>
    </View>
  );
  }
}
const styles = StyleSheet.create({
  container: {
  flex: 1,
  backgroundColor: '#FFF',
  },
   title_view:{
  flexDirection:'row',
  height:50,
  justifyContent: 'center',
  alignItems: 'center',
  backgroundColor:'#27b5ee',
  },
   title_text:{
  color:'#FFF',
  fontSize:20,
  textAlign:'center'
  },
  three_image_view:{
  paddingTop: 15,
  flexDirection:'row',
  justifyContent: 'space-around',
  alignItems: 'center',
  backgroundColor:'#FFF',
  },
  vertical_view:{
  justifyContent: 'center',
  alignItems: 'center',
  backgroundColor:'#FFF',
  paddingBottom:15,
  },
   top_text:{
  marginTop:5,
  color:'black',
  fontSize:16,
  textAlign:'center'
  },
  rectangle_view:{
  paddingTop:8,
  paddingBottom:8,
  paddingLeft:15,
  paddingRight:15,
  flexDirection:'row',
  justifyContent: 'space-between',
  alignItems: 'center',
  backgroundColor:'#FFF',  
  borderBottomColor:'#dedfe0',
  borderBottomWidth:1,
  },
  rectangle_text:{
  color:'black',
  fontSize:16,
  textAlign:'center',
  paddingLeft:8,
  },
  button: {
  margin: 7,
  padding: 5,
  alignItems: 'center',
  backgroundColor: '#eaeaea',
  borderRadius: 3,
  },
});
AppRegistry.registerComponent('luumans', () => luumans);
```