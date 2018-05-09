---
title: javaScript初体验
date: 2018-04-24 23:48:52
tags:
categories: javaScript
---
java

监听事件
``` javaScript
div.onekeypress = function(aga){
    console.log('你按压了一个键')
}
```

js创建容器，循环后只会留下最后的内容。
``` javaScript
for(var i=0,kbd;i<keys[index].length;i++){
            kbd = document.createElement('kbd')
            div1.appendChild(kbd)
            kbd.textContent = keys[index][i]
        }
```
要取得正确的元素，可以从传入的形参判断。

localStorage
只允许传入字符串

# canvas画板
api
vh = viewport height
onmousremove事件触发间隔是固定的（上传率）

canvas默认inline-block元素,请记住一定改成块级元素或者对齐，否则高度宽度会有异常！
不建议使用css初始化canvas的宽高，比如使用vh会导致画布整体缩放，像图片一样

canvas大小不能用css指定，请使用canvas.width 和canvas.height

clear
stroke
fill

得到的clientX是在整个文档流当中的位置


document.documentElement.clientWidth 得到整个文档流的宽
高同理

onresize 某个元素大小变了

一个按钮做一件事

window是一个全局变量

wap技术
wap其实是一种语言，一种简化的html


移动端
iphone 3Gs
像素低
那时候把页面缩小，双击放大

模拟为980px宽度

手机触摸是touch事件

如果不支持touch事件，.ontouchxxx会返回undefined，如果支持，返回的是null

in操作符，判断元素是否在里面（特性检测）

触屏设备支持多点触控,就是多个touch数组

移动端优化 禁用缩放
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no" />

overflow: hidden;

设置canvas下载
阻止默认事件 preventDefault
