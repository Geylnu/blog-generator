---
title: 自己试试写个jQuery小demo
date: 2018-05-28 23:58:54
tags:
- jQuery
categories: javaScript 
---
## 要达到的效果
我需要这个小demo可以在接受Node的实例和选择器，并且可以立即调用相关的操作方法

## 侵入式或者非侵入式？
我其实可以直接在Node的原型对象上加上我自己写的方法，其它什么都不用做，不过这种方式会霸占变量名，而且更改了原来的原型对象，很容易带来冲突 ，所以可以选择自己新增一个方法，从这个方法里调用原来Node的方法，不对原来的Node做任何变动，这种方式叫就非侵入式。

## 具体实现
这里其实有两种实现方法，一种是直接用函数构造出具有相关方法属性的对象，一次搞定。
``` js
function xxx(){
    let x ={}
    ...
    //找到的Node实例加到x种
    x.a =function(){
        ...
    }
    return x
}
```
另一种是构造出相关属性的对象，再构造一个拥有相关方法的原型，将__proto__指向原型
``` js
let yyy ={
    x:function (){
        ...
    } 
}
function xxx(){
    let x ={}
    ...
    //找到的Node实例加到x种
    x.__proto__=yyy
    return x
}
```
后面这种方式会更好一点,不过先使用前一种种方式实现一次
``` js
window.jQuery = function (nodeOrSelection) {
    let elements = {}
    if (typeof nodeOrSelection === 'string') {
        let temp = document.querySelectorAll(nodeOrSelection)
        for (let i = 0; i < temp.length; i++) {
            elements[i] = temp[i]
        }
        elements.length = temp.length
    } else if (nodeOrSelection instanceof Node) {
        elements[0] = nodOrSelection
        elements.length = 1
    }
    elements.addClass = function (nodeClass) {
        for (let i = 0; i < elements.length; i++) {
            elements[i].classList.add(nodeClass)
        }
    }
    elements.setText = function (text) {
        for (let i = 0; i < elements.length; i++) {
            elements[i].textContent = text
        }
    }
    return elements
}
window.$ = jQuery

$('li').addClass('red')
$('li').setText('hi')
```
这里还可以写个自己的forEach，这样不用每次都自己写遍历了。
OK.
