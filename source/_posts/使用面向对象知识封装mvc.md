---
title: 使用面向对象知识封装mvc
date: 2018-07-22 14:42:41
tags:
categories:
---
面向对象（Object-Oriented 简称OO）到底是什么？没有人能说清楚

面向对象具体的几个方面
0
NaN
undefined
''
null

## 命名空间
我们通常会遇见这样的js语法 
``` js
var XXX = XXX || {}
```
表示如果xxx没有定义就生成一个新对象，如果定义就是原来的对象

``` js
button.onclick = function(){
    console.log(this)
}
//触发事件得元素
button.addEventListerer('click',function (){
    console.log(this)
})
//该元素
jquery
```

注意不要直接覆盖ptoyotype 这会覆盖原有的construcotor