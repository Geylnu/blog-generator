---
title: MVC
date: 2018-07-09 22:40:20
tags:
categories:
---
M:Model 
V:view
c:controller

模块化 把每个分成一个个js小文件
每个模块把操作的html作为一个view，作为v（我觉得应该是model）
再生成一个controller，参数是view，接受view进行操作

这里最好将controller封装成一个对象，像下面这样
``` js
var controller = {
    view: null,
    init: function (view){
        this.view = view
        this.bindEvents()
    }

    bindEvents: function (){
        xxx.addListener('xxx',function (){
            xxx
        })
    }
}
```
值得注意的是监听函数中调用的是触发事件本身的元素，也就是这里取this取到的不是controller对象本身，这种情况可以使用箭头函数，箭头函数自身不具有this，使用的this向上寻找到的对象就是正确的了。

同时由于多个模块相互分离，并不知道其它模块的情况，可能出现全局变量污染的情况，因此，需要使用立即执行函数。

js中在ES6前不能使用`{}`包裹创建局部变量，避免全局变量污染可以使用`function`的创建局部变量环境。
``` js
function xxx(){
    ...
}.call()
```
但是这个方案实际还是存在问题，xxx其实也是一个全局变量

升级版就出现了
``` js
function (){

}.call()
```
然而遗憾的是这个方法会报语法错误

再升级版
``` js
!function (){
}.call()
```
使用其它单目运算符也能起到一样的效果，整个包裹大括号也是可行的，但可能出现bug,比如被视为函数实参。
还可以用随机数，不过有点丑陋

同时还可以使用闭包
``` js
!function (){
    var person ={}
    person.name = 'xxx'
    window.getname = function (){
        return person.name
    }
}
```
