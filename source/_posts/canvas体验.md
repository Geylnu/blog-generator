---
title: canvas体验
date: 2018-05-08 23:28:27
tags:
categories: javaScript
---
# 必要的初始化
1. canvas默认是一个inline-block元素，这通常会导致一些BUG产生，例如下方有空白，宽度不合理，建议将其显示为`block`元素

2. 让画板覆盖全屏
    这里需要不能使用css为其设置样式，在css中设置的样式实际并没有改变画板内部的大小，只是将画布简单的放大。
``` js
var canvas =document.getElementById('id')
canvas.width = document.documentElement.clientWidth
canvas.height = document.documentElement.clientHeight
```

获得一个具有2D绘制功能的对象
``` js
var ctx = canvas.getContext('2d')
```

# 绘制图像
现在就可以开始绘制图像了
具体如何绘制可以参考官[api文档](http://bucephalus.org/text/CanvasHandbook/CanvasHandbook.html)，官方文档什么都有。

绘制肯定需要鼠标键盘事件了，js监听事件非常简单
``` js 
document.onclick = function (e){
    console.log(e)
}
``` 
这里我们可以从鼠标事件中得到事件触发的相关参数，我们这里只需要clientX和clientY就可以了，它们代表鼠标点击的是当前页面窗口的位置，并不是在canvas中的位置，有时候页面会被用户改变，此时也需要让canvas自动改变
``` js
function autoSetSize(obj) {
    obj.width = getPageSize().width
    obj.height = getPageSize().height
}

window.onresize = function (param) {
    autoSetSize(canvas)
}
```
这样就可以了,getPageSize是前面函数的封装

另外需要注意的是canvas默认背景是透明的，需要自己画个颜色做背景。

# 移动端优化
## 历史渊源
最早的时候手机浏览采用的是一种wap语言，也就是HTML的简化版，到了iphone3Gs，乔布斯把真正的网页搬上了手机，它们把手机模拟成一个宽度为980px的网页，进行等比缩放，用户也可以放大进行点击。

## 优化
但是我们的画板我们并不需要缩放，我们因此就需要禁用它。

``` html
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no" />
```
意思是内容宽度等于设备屏幕的物理宽度，初始比例为一，禁用缩放。
在移动端交互方式也不再一样，产生的是touch事件,touch事件会将每一个点的属性以数组的形式列在touch事件中。

但是我们如何判断其是否支持触摸？
在事件初始化过程中，如果设备支持此事件，事件监听会被置为`null`,而如果不具有，会为`undefined`
``` js
console.log(document.onclick)
```
除此之外，在许多手机浏览器会给页面添加一些滚动的默认效果，比如露出背景，这需要使用preventDefault阻止。