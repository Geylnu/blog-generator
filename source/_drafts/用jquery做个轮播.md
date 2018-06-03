---
title: 用jquery做个轮播
tags:
---
内容
样式
行为

MVC模式

使用flex

flex 会自动把内容全部放到一行，在多图片的情况下，会自动压缩宽度，适合一行

使用`align-item: flex-start`不压缩 

jquery使用.css加样式，为什么不用.style？历史遗留问题

jquery推荐使用on('touchstart',function (){})写事件监听

找自己是父元素中的第几个
``` js
var index $(x).index() //搞定
```

.eq(x) 把$返回的第x元素也变成$

.silbing('xx')在兄弟中寻找指定元素

.trigger('click') 触发事件

用户进入英国禁止播放

一种停止再重启，一种轮播的时候就判断时候就判断是否停止

排查问题
一个个排除,二分法

浏览器自带的缩放放大可能会导致bug,比如抖动

可替换元素，在浏览器下载下来前并不知道其宽度，而下载下来后又会导致重排，影响性能，因此尽量写上width