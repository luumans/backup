title: Vue  资源整理
date: 2017-03-25 18:29:00
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
　　**自用笔记：**Vue.js通过简洁的API提供高效的数据绑定和灵活的组件系统。最近在Github上看到了不少Vue的项目，很好奇，决定尝试尝试。
<!-- more -->

### 添加书籍
```
<!DOCTYPE HTML>
<html>
<head>
    <title>导航</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, minimum-scale=1.0, maximum-scale=1.0">
    <!-- 移动端 -->
    <meta name="format-detection" content="email=no">
    <meta name="format-detection" content="telephone=no">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <!-- 介绍 -->
    <meta name="description" content="" />
    <meta name="Keywords" content="" />
    <meta name="author" content="">
    <!-- 清除cache -->
    <meta http-equiv="Expires" content="-1">
    <meta http-equiv="Cache-Control" content="no-cache">
    <meta http-equiv="Pragma" content="no-cache">
    <script src="http://cdnjs.cloudflare.com/ajax/libs/vue/1.0.7/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/vue.resource/1.2.1/vue-resource.min.js"></script>
    <link rel="stylesheet" type="text/css" href="https://cdn.bootcss.com/bootstrap/3.3.5/css/bootstrap.min.css">
</head>
<body>
<style type="text/css">
    table tbody tr:nth-child(2n) button{
        color: #fff;
        background-color: #d9534f;
        border-color: #d43f3a;
    }
</style>
        <div class="container">
            <div class="col-md-6 col-md-offset-3">
                 <h1>Hello Vue.js ! </h1>
                <div id="app">
                    <table class="table table-hover" v-cloak="">
                        <thead>
                            <tr>
                                <th class="text-right" @click="sortBy('id')">序号</th>
                                <th class="text-right" @click="sortBy('name')">书名</th>
                                <th class="text-right" @click="sortBy('author')">作者</th>
                                <th class="text-right" @click="sortBy('price')">价格</th>
                                <th class="text-right">操作</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="book in books">
                                <td class="text-right">{{book.id}}</td>
                                <td class="text-right">{{book.name}}</td>
                                <td class="text-right">{{book.author}}</td>
                                <td class="text-right">{{book.price}}</td>
                                <td class="text-right">
                                    <button type="button" class="btn btn-success" @click="delBook(book)">删除</button>
                                </td>
                            </tr>
                            <tr>
                                <td class="text-right" colspan="5">
                                     <h4>总价：{{sum}}</h4>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    <div id="add-book">
                        <legend>添加书籍</legend>
                        <div class="form-group">
                            <label>书名</label>
                            <input type="text" class="form-control" v-model="book.name">
                        </div>
                        <div class="form-group">
                            <label>作者</label>
                            <input type="text" class="form-control" v-model="book.author">
                        </div>
                        <div class="form-group">
                            <label>价格</label>
                            <input type="text" class="form-control" v-model="book.price">
                        </div>
                        <button class="btn btn-primary btn-block" @click="addBook()">添加</button>
                    </div>
                </div>
            </div>
        </div>
    <script type="text/javascript">
        var app = new Vue({
            el: "#app",
            ready:function(){
                this.$http.get('https://api.myjson.com/bins/r8mm').then(function(data){
                        console.log(data.body)
                        this.$set('books',data.body)
                    },function(response){
                });
            },
            data: {
                sortparam: '',
                book: {
                    id: 0,
                    author: '',
                    name: '',
                    price: ''
                },
                //     books:''
                books: [{
                    id: 1,
                    author: '施耐庵',
                    name: '红楼梦',
                    price: 32.0
                }, {
                    id: 2,
                    author: '曹雪芹',
                    name: '水浒传',
                    price: 30.0
                }, {
                    id: '3',
                    author: '罗贯中',
                    name: '三国演义',
                    price: 24.0
                }],
                sorts: []
            },
            computed: {
                sum: function () {
                    var result = 0;
                    for (var i = 0; i < this.books.length; i++) {
                        result += Number(this.books[i].price)
                    };
                    return result;
                }
            },
            methods: {
                addBook: function () {
                    this.book.id = this.books.length +1;
                    this.books.push(this.book);
                    this.book = '';
                },
                delBook: function (book) {
                    console.log(book)
                    this.books.$remove(book);
                },
                sortBy: function (sortparam) {
                    this.sorts = this.books
                    if(typeof(this.sorts[0][sortparam]) == 'number'){
                        this.sorts.sort(function(tr1,tr2){
                            n1 = parseInt(tr1[sortparam]);
                            n2 = parseInt(tr2[sortparam]);
                            console.log(n1)
                            return n2-n1;
                        });
                    }else{
                        this.sorts.sort(function compareFunction(tr1,tr2){
                            return tr1[sortparam].localeCompare(tr2[sortparam]);
                        });
                    }
                }
            }
        })
    </script>
</body>
</html>
```


### 
```
```

### 
```
```


### 
```
```