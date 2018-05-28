---
title: DOM API
date: 2018-05-27 19:42:57
tags:
categories: javaScript
---
# 什么是DOM？
`DOM`全称为`Document Object Model`,是js用来操控XHTML,在DOM中，所有标签都是node的子集，文档最开始的属性标识了文档采用的版本，比如`<!DOCTYPE html>`表示是html5,这个属性每个HTML/XHTML文档都必须具有。将文档每个标签以树形的方式组织方便js对 其进行操控，着建里起的模型就叫DOM。

# node
DOM中所有标签及元素等都被抽象地称为`node`节点，比如对于`标签元素`，还具有

# DOM标准
Element对象代表页面中具有的标签，构造函数为Element,整个文档被称为document,对应的构造函数为Document
元素中的文字被Text构造出来

Element、Document、Text都继承自node，其它结点还有注释结点`Comment`这些，不过平时很少用

## nodeType
编号|节点
:--:|:--:
1|标签
3|文字
7|注释
9|document
10|声明文档类型的标签
11|

## 属性
### nodeName
返回节点的名称，除特例外总是返回名称的大写
特例：
* document
直接使用document.nodeName返回的是`#document`，需要使用`document.documentElement.nodeName`，才会正常返回`HTML`

* svg
    svg很特殊，只有它的标签是小写

### innerText
获取到其节点内的所有文本内容(微软发明的)

### textContent
和上面一样，只是不会忽略style和script标签，性能会更好一点。（火狐发明的）

## 方法
### appendChild()
添加儿子
### cloneNode()
默认进行浅拷贝。如果传`true`进行深拷贝。
### contains()
是否包含指定元素
### hasChildNodes()
是否有子节点
### insertBefore()
插入人到元素前面
### isEqualNode()
基本一样
### isSanmeNode()
同一元素
### removeChild()
杀掉儿子，还是会在内存中，可能不会释放内存占用
### replaceChild()
替换成指定的儿子
### normalize()
常规化，把元素不正常的情况正常化

# Document接口
## 属性
### anchors
获得所有a标签，html5被弃用
原因:由于向后兼容原因，该属性只返回拥有name属性的a元素，而不是拥有id属性的a元素

### body
### characterSet
返回使用的字符编码

### childElementCount
返回子元素个数
### domain
返回域名
### location
一个包含地址信息的对象
###  readyState
是否正在下载
### referrer
从哪里访问过来
### visibilityState
该页面是否被显示

### children
### doctype
### documentElement
### fullscreen
### head
### hidden
### images
### links
### onxxxxxxxxx
### origin
### plugins
### readyState
### referrer
### scripts
### scrollingElement
### styleSheets
### title

## 方法

### close()
页面加载完毕后关闭

### write()
向文档内写入东西，如在open时调用write会向文档追加内容，如果已经关闭了调用write会**重写documentn的内容**，这**十分危险！** 不要再任何**异步**或者**延时性**的操作上使用write

### writeln()
写一行

### open()
页面一开始打开的时候启用open,可以向文档内写入数据

### execCommand()
文本编辑器会用到这个

### createDocumentFragment()
### createElement()
### createTextNode()
### exitFullscreen()
### getElementById()
### getElementsByClassName()
### getElementsByName()
### getElementsByTagName()
### getSelection()
### hasFocus()
### querySelector()
返回找到的第一个元素
### querySelectorAll()
以伪数组的形式返回符合的所有元素
### registerElement()

# Element
## 属性
### innerHTML
其中的HTML，被改写有安全风险