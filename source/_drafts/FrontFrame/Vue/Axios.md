title: Axios初探
date: 2017-08-25 18:29:00
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

　　**Ajax**，对于数据请求大家一定不会陌生。项目中常常使用的技术都有哪些，接下来我们将着重介绍Axios的使用，配置。
<!-- more -->

# 概况
I have been working with React for the last two years. One of the questions many React beginners ask: "What's the React way to fetch data from the server" or "How should I make AJAX calls in React"? To answer your question, as many developers would tell you, React is just a view library and you are free to use whatever you are comfortable with. However, this doesn't help much - especially when the JavaScript landscape is changing so rapidly. So, in this article I will try to answer this basic question and list down 5 simple libraries for making AJAX calls in React.

## Ajax请求
### jQuery $.ajax
This is a quick & dirty way to make AJAX calls. In the former, official React tutorial, they use jQuery to fetch data from the server. If you are just starting out and are playing around with React, this can save you a lot of time. Many of us are already familiar with jQuery. So, it won't take a lot of time to understand and use it in React. Here is how a simple API call is made, with jQuery:
```
loadCommentsFromServer: function() {
	  $.ajax({
	  url: this.props.url,
	  dataType: 'json',
	  cache: false,
	  success: function(data) {
	    this.setState({data: data}); // Notice this
	  }.bind(this),
	  error: function(xhr, status, err) {
	    console.error(this.props.url, status, err.toString());
	  }.bind(this)
	});
}
```
P.S. Snippet is from React's former official tutorial.

It's the same old jQuery's $.ajax used inside a React component. Notice how this.setState() is called inside the success callback. This is how you update the state with data obtained from the API call.

However, jQuery is a big library with many functionalities - So, it doesn't make sense to use it just for making API calls (Unless you are already using it for a bunch of other tasks). So, what's the alternative? What should we use? The answer is fetch API.

### Fetch API
Fetch is a new, simple and standardised API that aims to unify fetching across the web and replace XMLHttpRequest. It has a polyfill for older browsers and should be used in modern web apps. If you are making API calls in Node.js you should also check out node-fetch which brings fetch to Node.js.

Here is what the modified API call looks like :
```
loadCommentsFromServer: function() {
    fetch(this.props.url).then(function(response){
        // perform setState here
    });
}
In most modern React tutorials you will find fetch being used. To know more about fetch, check out the following links :
```
- [David Walsh Blog](https://fetch.spec.whatwg.org/ "")
- [Google Developers](https://developers.google.com/web/updates/2015/03/introduction-to-fetch?hl=en "")
- [WHATWG](https://davidwalsh.name/fetch "")
- [Mozilla](https://developer.mozilla.org/en/docs/Web/API/Fetch_API "")

### Superagent
Superagent is a light weight AJAX API library created for better readability and flexibility. If due to some reason, you don't want to use fetch, you should definitely check this out. Here is a snippet to demonstrate its usage :
```
loadCommentsFromServer: function() {
    request.get(this.props.url).end(function(err,res){
        // perform setState here
    });
}
```
Superagent also has a Node.js module with the same API. If you are building isomorphic apps using Node.js and React, you can bundle superagent using something like webpack and make it available on the client side. As the APIs for client and server are the same, no code change is required in order to make it work in the browser.

### Axios
Axios is a promise based HTTP client for Node.js and browser. Like fetch and superagent, it can work on both client and server. It has many other useful features which you can find on their GitHub page.

Here is how you make an API call using Axios :
```
loadCommentsFromServer: function() {
    axios.get(this.props.url).then(function(response){
      // perform setState here
    }).catch(function(error){
      //Some error occurred
    });
}
```

### Request
This list will be incomplete without request library which was designed with simplicity in mind. With more that 12k GitHub stars, it's also one of the most popular Node.js modules. You can find more about request module on their GitHub page.

Sample usage :
```
loadCommentsFromServer: function() {
    request(this.props.url, function(err, response, body){
          // perform setState here
    });
}
```

- [5 best libraries for making AJAX calls in React](https://hashnode.com/post/5-best-libraries-for-making-ajax-calls-in-react-cis8x5f7k0jl7th53z68s41k1 "在react中5种Ajax数据请求")


My Take
As fetch is the new standardised API to interact with remote resources, I would suggest using it for all your AJAX needs (not only in React, but all types of JavaScript apps).

# Axios Vue.js

## Features
从浏览器中创建 XMLHttpRequest
从 node.js 发出 http 请求
支持 Promise API
拦截请求和响应
转换请求和响应数据
取消请求
自动转换JSON数据
客户端支持防止 CSRF/XSRF

## Installation

```
npm install axios

bower install axios

yarn add axios

https://cdnjs.cloudflare.com/ajax/libs/axios/0.15.3/axios.min.js
```

## Axios API
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
axios.all(iterable)
axios.spread(callback)

```
var config = {
  headers: {'X-My-Custom-Header': 'Header-Value'}
};
axios.get('https://api.github.com/users/codeheaven-io', config);
axios.post('/save', { firstName: 'Marlon' }, config);
```

## Creating an instance
axios.create([config])

## Axios Vue

```
import axios from 'axios'

import qs from 'qs'
import * as Tool from 'UTIL/tool'
// axios 配置
axios.defaults.timeout = 5000
axios.defaults.baseURL = ''
// axios.defaults.headers.common['Authorization'] = `token ${TOKEN}`
// 配置请求头
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'

// 添加请求拦截器（POST传参序列化）
axios.interceptors.request.use((config) => {
  if (config.method === 'post') {
    config.data = qs.stringify(config.data)
  }
  let URL = config.url.split(config.baseURL)
  Tool.open(URL[1])
  return config
}, (error) => {
  Tool.toast('错误的传参', 'fail')
  return Promise.reject(error)
})

// 添加响应拦截器（返回状态判断）
axios.interceptors.response.use((res) => {
  // console.log(res)
  // 拦截器配置
  // if (res.data.success) {
  //   Tool.toast(res.data.msg)
  //   Tool.close()
  //   return Promise.reject(res)
  // }
  Tool.close()
  return res
}, (error) => {
  Tool.toast('网络异常', 'fail')
  Tool.close()
  return Promise.reject(error)
})

export const oGet = (url, params) => {
  return new Promise((resolve, reject) => {
    axios.get(url, params)
      .then(res => {
        resolve(res.data)
      }, err => {
        reject(err)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export const oPost = (url, params) => {
  return new Promise((resolve, reject) => {
    axios.post(url, params)
      .then(res => {
        resolve(res.data)
      }, err => {
        reject(err)
      })
      .catch(err => {
        reject(err)
      })
  })
}

export default {
JSSDK () {
  return oPost(`/wx/sign/query`, {url: window.location.href})
},
  Get (link) {
    return oGet(link)
  }
}
```

### A Common Base Instance
```
import axios from 'axios';

export const HTTP = axios.create({
  baseURL: `http://jsonplaceholder.typicode.com/`,
  headers: {
    Authorization: 'Bearer {token}'
  }
})
```

# vue-resource冲突解决
main.js
```
import axios from 'axios'
Vue.prototype.$http = axios
```

```
this.$http.get(URL).then(response => {
    // success callback
}, response => {
    // error callback
})
```

- [axios](https://www.npmjs.com/package/axios#axiosdeleteurl-config "")
- [Vue.js REST API Consumption with Axios](https://alligator.io/vuejs/rest-api-axios/ "")
- [axios在vue中的简单配置与使用](http://blog.csdn.net/sinat_17775997/article/details/69367204 "")
- [Vue 爬坑之路（六）—— 使用 Vuex + axios 发送请求](http://www.cnblogs.com/wisewrong/p/6402183.html "")
- [Retiring vue-resource](https://medium.com/the-vue-point/retiring-vue-resource-871a82880af4 "")
- [Fetching Data from a Third-Party API with Vue.js and Axios](https://www.sitepoint.com/fetching-data-third-party-api-vue-axios/ "")
- []( "")



Retiring vue-resource
As Vue users, many of you may have used vue-resource for handling ajax requests in your Vue applications. For a long time it’s been thought of as the “official” ajax library for Vue, but today we are retiring it from official recommendation status.
Although listed under the vuejs organization, vue-resource was almost completely written and maintained by the PageKit team. We transferred it into the vuejs organization in the early days because it was great seeing the community contributing libraries solving an essential problem, and we greatly appreciate all the work the PageKit team has put into the project. However, over time we have come to the conclusion that an “official ajax library” is not really necessary for Vue because:
Unlike routing and state-management, ajax is not a problem domain that requires deep integration with Vue core. A pure 3rd-party solution can solve the problem equally well in most cases.
There are great 3rd party ajax libraries that solve the same problem, are more actively improved/maintained, and designed to be universal/isomorphic (works in both Node and Browsers, which is important for Vue 2.0 with its server-side rendering usage).
Given (1) and (2), it’s obvious that we are duplicating the effort and bringing in unnecessary maintenance burdens by keeping vue-resource’s current status. The time we spend on resolving vue-resource issues can be better spent improving other parts of the stack.
Q&A
Does this mean vue-resource is deprecated?
No. It’s just no longer part of the “official recommendation”. The repo will be moved back to pagekit/vue-resource, and will continue to work. It will be up to the PageKit team to decide the long term plan for the library.
Should I Stop Using It?
It’s totally fine to keep using it if you are happy with it. Potential reasons to migrate away include maintenance, universal/isomorphic support and more advanced features.
What Should I Use Then?
You are free to pick whatever you prefer (even just $.ajax), but as a default recommendation — particularly for new users — we suggest taking a look at Axios. It’s currently one of the most popular HTTP client library and covers almost everything vue-resource provides with a very similar API. In addition, it is universal, supports cancellation, and has TypeScript definitions.
If you prefer something lower-level, you can simply use the standard fetch API. Take a look at isomorphic-fetch which is a polyfill that works in both browsers and Node.
Tips for Using Axios with Vue
You need to provide your own Promise polyfill when using Axios, if your target environments do not natively support Promises.
If you’d like to access this.$http like in vue-resource, you can just set Vue.prototype.$http = axios.