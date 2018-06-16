---
title: JSONP
date: 2018-06-10 19:32:03
tags: JSON
categories: javaScript
---

发出post/get请求后不可避免引起页面刷新，以前使用iframe来避免刷新主页面，现在不用了

前一种方式很丑陋，后来想到了动态创建img/script发起get请求

用onload onerror监听成功还是失败

img可以创建后立即发起请求
而`<script>`必须放进body

`<script>`先调用脚本，再完成onload事件
我们可以onload后再把这个script移除

ajax出来之前用这个

script可以跨域

为了降低耦合，可以直接传一个方法

get: callback=xxx随机数
调用完成删除函数

jquery自己就自己实现了
``` js
$.ajax({
    url: "xxxx"
    dateType: 'jsonp'
    success: function(A){
        ...
    }
})
```

jquery后面还有防止缓存的数字

