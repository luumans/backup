title: Vue SSR Nuxt 国际化
date: 2018-08-25 18:29:00
description:
categories:
- FrontFrame
tags:
- Nuxt
toc: true
author:
comments:
original:
permalink:
---
Vue I18n is internationalization plugin for Vue.js
<!-- more -->

# 安装

``` node
$ npm install vue-i18n --save
```

# 使用
## ~/nuxt.config.js
引入插件，启动中间件

``` javascript
plugins: ['~/plugins/i18n.js'],
router: {
  middleware: 'i18n',
}
```

## ~/plugins/i18n.js
``` javascript
import Vue from 'vue'
import VueI18n from 'vue-i18n'

Vue.use(VueI18n)

export default ({ app, store }) => {
  let data = {}
  let Locale = store.state.locales
  for (let i = 0; i < Locale.length; i++) {
    data[Locale[i]] = require(`~/locales/${Locale[i]}.json`)
  }
  // Set i18n instance on app
  // This way we can use it in middleware and pages asyncData/fetch
  app.i18n = new VueI18n({
    locale: store.state.locale,
    fallbackLocale: 'en',
    messages: data
  })
	// 自定义页面跳转方法
  app.i18n.path = (link) => {
    return `/${app.i18n.locale}/${link}`
  }
}
```

## ~/middleware/i18n.js
``` javascript
export default function ({ isHMR, app, store, route, params, error, redirect }) {
  const defaultLocale = app.i18n.fallbackLocale
  // If middleware is called from hot module replacement, ignore it
  if (isHMR) return
  // Get locale from params
  const locale = params.lang || defaultLocale
  if (store.state.locales.indexOf(locale) === -1) {
    return error({ message: 'This page could not be found.', statusCode: 404 })
  }
  // Set locale
  store.commit('SET_LANG', store.state.locale)
  app.i18n.locale = store.state.locale
  // If route is /<defaultLocale>/... -> redirect to /...
  if (locale === defaultLocale && route.fullPath.indexOf('/' + defaultLocale) === 0) {
    const toReplace = '^/' + defaultLocale + (route.fullPath.indexOf('/' + defaultLocale + '/') === 0 ? '/' : '')
    const re = new RegExp(toReplace)
    return redirect(
      route.fullPath.replace(re, '/')
    )
  }
}
```

## ~/locales/index.js
创建本地语言库

``` javascript
export default () => {
  return ['en', 'fr', 'cn']
}
```

## ~/locales/fr.json
更加不同页面添加不用的语言

``` javascript
{
  "links": {
    "home": "Accueil",
    "about": "Ã propos",
    "english": "Version Anglaise",
    "french": "Version FranÃ§aise"
  },
  "home": {
    "title": "Bienvenue",
    "introduction": "Ceci est un texte d'introduction en FranÃ§ais."
  },
  "about": {
    "title": "Ã propos",
    "introduction": "Cette page est faite pour vous donner plus d'informations."
  }
}
```

## ~/store/index.js
``` javascript
import Locale from '~/locales'

export const state = () => ({
  locales: Locale(),
  locale: Locale()[0]
})

export const mutations = {
  SET_LANG(state, locale) {
    if (state.locales.indexOf(locale) !== -1) {
	  	console.log(locale)
      state.locale = locale
    }
  }
}
```

# 方法

## 获取
``` javascript
$t('links.english')
```

## 设置
``` javascript
this.$i18n.locale = name
```