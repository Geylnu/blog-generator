---
title: css画个皮卡丘
date: 2018-11-03 13:52:11
tags: css
categories: css
---
# 认识codepen
## 简介
主要是一些设计师做的小作品
# 开始做
大都使用绝对定位，使用一个320宽度的外框（iphone5宽度320）
## 鼻子

输入` left: 50%`进行居中，值得注意的是这个居中只是侧边居中，还需要输入`marign: 宽度一半`来实现真实的居中，更好的不需要输入实际值的方案是`transform: translateX(50%)`

鼻子用`border-radius`做，不过方方这里没原版做的好
## 眼睛 
眼白可以加黑色外框调小（其实条大小也是可行的）
## 嘴

复杂选择器
``` css
// 这似乎是css-leavl-4的标准
.wrapper > :not(:last-child){
}
```

用setTimeout替代setInterval
``` js
let time = 1oo
let id = window.setTimeout(function fn(){
    //do some thing
    window.setTimeout(fn,time)//ok
    //这里或许应该更改id的指，方便clear
},time)
```