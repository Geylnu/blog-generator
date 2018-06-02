---
title: 自己试试写个小小的jQuerydemo
date: 2018-05-28 14:11:48
tags:
- jQuery
categories: javaScript
---

# jQuery怎么做的?
看了一下jquery返回的




不要学jQueryMobile
小技巧：
``` js
methodName = (if x === b) ? 'add':'remove'
objecct[methodName]()
```
# 尝试自己写一个jQuery

# 命名空间
记得给自己函数起个名字，让方法在对象当中，这样不会污染全局变量

# 调用方式
我们更习惯使用`item.getSibling()`,而不是`getSibling(item)`,为了实现前一种效果，我们有两种方式
## 更改node的原型
这是侵入式的
## 新添一个node2
新的node2使用node原来的api
var node2 = node(item3)

## 支持选择器
document.querySelector(string)
只有一个也需要返回一个伪数组

# 初探jQuery
toggleClass('red')

addclass() 可接受字符串，方法，接受方法类似于forEach,返回的式类名

链式操作

## jQuery
* 兼容性好，1.7版本兼容IE6

如果是jQuery构造的，就在对象名字这里加个jQuery