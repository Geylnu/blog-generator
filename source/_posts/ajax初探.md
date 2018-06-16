---
title: ajax初探
date: 2018-06-11 23:05:10
tags:
- ajax
categories: javaScript
---

最开始利用form表单提交，使用method指定提交方式,action为提交的地址。

a标签点击能发请求

这两者都要刷新页面或者新开页面
img可以发请求，但请求的必须是图片 不需要增加到页面
script和link也可以发起请求，但link和script只能加载指定格式，但是需要加到页面

ie5在js中引入ActiveX对象（api），使js可以直接发起http请求
随后其它浏览器也跟进，取名`XMLHttpRequest`

不到一年，谷歌推出gmail.com,这里可以说前端真正诞生了

ie6后拉成为了安装最多的浏览器60%,ie因此就膨胀了,随后微软拆开ie6的开发人员，只更新安全功能

Jesse James Garrett 将一下技术取名AJAX（异步的javaScript和 XML

1. 使用XMLHttpRequest发请求
2. 服务器返回xml格式的字符串
3. js解析xml，并更新局部页面

现在用JSON

浏览器一般返回的都是字符串