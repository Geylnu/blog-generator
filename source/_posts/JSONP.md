---
title: JSONP
date: 2018-06-10 19:32:03
tags: JSON
categories: javaScript
---
# 为什么要使用JSONP？
在早期，访问html页面必须进行跳转，页面的展示也依靠点击超链接，由于加载一个新页面，这不可避免的需要进行刷新。
有些利用页面种嵌入`iframe`，点击链接/按钮`iframe`访问页面，达到页面不刷新的目的，这种实现方式很丑陋。

而JSONP是一种更方便的更新数据方式，类似于现在的`ajax`

# JSONP具体是怎么做的?
1. 创建一个随机的函数名，
2. 创建一个`<script>`,并将`src`指向要访问的后端服务器地址，如`/?callback=函数名`
3. 将`<script>`加入文档就会立即向指定地址发起请求
4. 服务器收到请求，将数据写入自己要返回的内容中，如`函数名(参数)`，
5. 客户端收到，立即执行了`<script>`中的内容，也就是`函数名(参数)`,通过这种方式就完成了动态的更新。

JSONP参数可以是任意的，不过为了统一，一般使用`callback=`这种调用。

JSONP的请求是跨域的，后端服务器只要实现了`callback`调用，就可以直接使用。但同时由于使用的是动态加载`<script>`资源，JSONP只能使用`get`方式请求资源。

jQuery对其进行了封装，调用方式如下
``` js
$.ajax({
    url: "xxxx"
    dateType: 'jsonp'
    success: function(A){
        ...
    }
})
```
我的简单实现可以在[这里](https://github.com/Geylnu/nodeServerDemo/blob/master/main.js)

