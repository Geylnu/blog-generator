---
title: DOM事件
date: 2018-06-04 23:08:26
tags:
- 事件
- DOM
categories: javaScript
---
# DOM标准
## 标准前的标准
在未指定标准前就已经有了事实上的标准
如：
``` js
button.onclick = function{

}
```
## DOM1
有了更多的事件
* blur
* click
* focus
* select

## DOM2
事件被单独列出来，有了拥有了事件流，事件捕获，事件冒泡，事件取消，也是最广泛的版本

# 事件详解
## 声明方式
1. 内联声明
``` html
<script>
    function xxx(){
    }
</script>
<button onclick="xxx"></button>
<button onclick="xxx()"></button>
<button onclick="xxx.call"></button>
```
这种方式比较奇怪的时第一种方式无效，下面两种方式有效，实际上这里等同于`eval(xxx())`

2. js声明
``` js
function xxx(){
    }
button.onclick = xxx
button.addListener('click',xxx,options)
```

3. `onclick`与`addListener`的区别
前者是一个属性，只能赋值一个，后者是一个队列，可以监听多个事件，并按照声明顺序触发。
除此之外，第三个参数还可以指定一些布尔值

## 事件模型
### 捕获与冒泡
在DON的事件模型中，事件传播被分为两个阶段，首先某事件触发后，会把该事件从父标签一级级传递给最终的子标签，这个阶段叫做捕获阶段，最终的子标签得到这个事件后，再把这个事件一级级返回到最上层父标签，这个过程叫做冒泡阶段，默认声明的事件是在冒泡阶段触发。值得注意的是，最终的子标签并不区分冒泡或捕获阶段，声明的多个事件按照声明先后触发。

如果只指定一个布尔值，则指是否在捕获阶段捕获事件
``` js
button.addListener('click',xxx,ture)
```
默认为在冒泡阶段捕获，也就是此值为false

阻止传播：
``` js
function (e){
    e.stopPropagation()
}
```


另外jquery()的阻止默认事件是直接传false
``` js
$(xxx).on('click',false)
```


## 应用
### 怎么达到点击别处关闭浮层的效果?
在`document`的冒泡阶段监听click事件，如果监听到就关闭浮层，而在浮层上监听click事件，阻止click事件冒泡。

我们还可以不使用阻止冒泡，我们在浮层被点击后添加一个0定时器，监听`document`，定时器会在空闲时触发，也就是当前事件传播结束，OK。


实现轮播细节
把第一张和最后一张复制到前面和后面，这里用`clone(true)`进行深克隆
复制后还要让其显示第二张

`hidden()`再`show()`就可以隐藏动画,但是短时间的操作浏览器会合并,`offset()`可以让css立即生效而不计算

jquery还可以监听所有子元素的，为其添加事件
``` js
$('xxx').on('click','button',function())
```
这里 第二个参数是选择器




