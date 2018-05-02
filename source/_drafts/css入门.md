---
title: css入门
date: 2018-04-05 23:10:42
tags:
categories: css
---
# 引入方式
## 最开始时
直接使用标签比如`<center>居中</center>`

## css
### 内联style属性
`<h1 style="text: align;">居中<h1>`

### <style\>标签
``` html
<head>
    <style>
        body{
            background-color: grey;
        }
    </style>
</head>
```
优点：更简单

### 外部文件(<link\>标签式)
``` html
<link rel="stylesheet" href="./a.css">
```

### import
css中引入另一个css
``` css
@import url(./b.css);
body{
    background-color: grey;
}
```


# 文档流
指文档内元素的流动方向
## 内联元素
内联元素从左向右流，如果遇见阻碍，换行继续流动
内联元素会被换行分割，但是英文单词不一样，每一个单词被看作一个字符‘
word-break,单词能否被换行
一般可选值：break-all break-word 

* 内联元素高度？
内联元素无法指定width和height,也无法指定margin-top及margin-bottom;

内联元素涉及到字体的排版，比较复杂。
其包括
基线，上部，和下部
建议行高，建议行高每种字体都不一样。
line-height: 1.2 自己指定行高
## 块级元素
块级从上往下流，每个都占一行，无限延展。

* 由高度由其内部文档流元素高度总和决定

## 内联块级元素
``` css
display:inline-block
```
空间够就会连起来，像span
但是很多错误，不用它

## 脱离文档流
### 定位
``` css
position: fixed /*相对屏幕定位*/
top: 0px;
left: 0px;
```
这种方式会将宽度缩为每个子元素觉得合适的大小
解决办法`width: 100%`,但除非必要，不要加

``` css
position: relative;/*父元素*/
position: absolute;/*子元素*/

```

相对于某个元素定位，脱离文档流

### float
让元素横起来
``` css
float: left
```

加了float一般都有bug，所以需要使用.clearfix,给所有使用浮动元素的父级添加这个类
``` css
.clearfix::after{
    content: '';
    display: block;
    clear: both;
}
```
# 样式
list-style: none 列表样式为无

除div和span没有默认样式，其它都有默认样式

style给予的样式
Computed计算出来的样式
默认16px

遇到不懂的可以直接去试试，或者查MDN

cs不s需要问为什么，只需要知道显示出来什么样子

可以在开发者工具强制触发hover

子元素超过父元素边界，可以使用添加block来解决
``` css
display: block;
```

inherit 继承

样式默认会从父级继承，浏览器也会给予默认样式

每个标签之间回车，空格，都会变成一个空格

好看的壁纸wall haven(壁纸天堂)

图片背景
``` css
background-position: center center;
background-size: cover;/*盖住所有的面积*/
```


需要写宽度时，写max-width可能是比width更好的办法，因为max-width能够自适应。

水平居中
``` css
margin-left: auto
margin-right: auto
```
line-height和height等同

## css三角形技巧
利用borde重合的地方会以倾斜的方式隔离渲染，可以制造三角形
**注意，最好是个block元素**

更多的技巧可以搜索css tricks
形状相关的写个css tricks shape

## 自己关于盒模型的一些认知
主要由四部分组成
* **内容本身宽度**，手动指定width length也是这个宽度
* **padding**,内边距,要和边框联系起来
* **边框**,设置边框宽度
* **外边距** 外边距宽度。


## 如果元素之间没有合并，可能是因为display:inline-block;

## 可以去iconfont.cn弄图标

## 垂直居中
``` css
vertical-align: top;
```

# 伪元素
所有非空元素都可以具有伪类
伪元素无法被选中

## 做一个阴阳
css3 linear gradient线性渐变
自己不写直接加个generator搜索 工具OK

### animation(动画)
* 声明一个关键帧
``` css
/*一个正方形不断旋转*/
.test{
  border:1px red solid;
  width:50px;
  height:50px;
  animation-duration: 3s;
  animation-name: slidein;
  animation-iteration-count:infinite;
  animation-timing-function:linear
}

@keyframes slidein {
  from {
    transform:rotate(0deg);
  }

  to {
    transform:rotate(360deg);
  }
}
```
具体内容参考阮一峰的[CSS动画简介](http://www.ruanyifeng.com/blog/2014/02/css_transition_and_animation.html)

设置负的margin可以让元素上浮

通过给父元素设置`text-align: center`可以让子内联元素居中

块级元素的上外边距和下外边距有时会合并（或折叠）为一个外边距，其大小取其中的最大者，这种行为称为外边距折叠（margin collapsing），有时也翻译为外边距合并。注意浮动元素和绝对定位元素的外边距不会折叠。

box-sizing: border-box;
重新设置盒模型

css还可以选择子元素偶数元素
``` css
:nth-chlid(even)
```

``` css
:hover
/*伪类*/
```
******
``` css
::
/*伪元素*/
```

如何设居中
margin auto
缺点：元素必须有width

使用display:inline-block再使用文字居中，也可以起到居中作用，但是需要vertical-align:top;否则下方会少一段



