title: Vue
date: 2017-08-18 18:29:00
description:
categories:
- FrontFrame
tags:
- Vue
toc: true
author:
comments:
original:
permalink: 
---
　　****文章适合有一定vue经验，只是简单介绍项目中的搭建与开发的优化之处。知识点，请自行查阅！
<!-- more -->

# 生命周期顺序
Vue 2.*

beforeCreate 组件实例创建属性计算之前
created 组件实例创建，属性绑定，DOM还未
beforeMount 模板挂载之前
mounted 模板挂载之后
beforeUpdate 组件更新之前
updated 组件更新之后
activated 组件激活
deactivated 组件移除
beforeDestory 组件销毁之前
destoryed 组件销毁之后

# 组件通信
父传子用props,父用子用ref 子调父用$emit,无关系用Bus

# Vuex