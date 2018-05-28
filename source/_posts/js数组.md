---
title: js数组
date: 2018-05-20 23:00:45
tags:
categories: javaScript
---
# 前言
js中window拥有非标准库和标准库，标准库中有Object及相关api,Sting,Boolean等，我们可以先试试这些函数/对象
## Object()
Object()会根据传入的参数返回不同的值
* 基本类型，所有可以被包装的的基本类型都会被包装成包装类型
传入空对象,undefined,或者不传，返回null，传入非空对象返回原对象

使用`new Array()`方式和构造函数没有区别

**这里我们可以总结一下**
如果是基本类型的函数/对象，不加`new`和加`new`是不相同的
如果是对象，比如`Function` 我们这里的`Array`，这两者是等同的

### Function构造函数
`let a = new Function('a','b','a+b')`
参数可以无到任意，但至少有函数体

`function`是个关键字，`Function`是个全局对象

函数有几种声明方式，声明出来的结果也不同
具名函数
``` js
function f(){
    xxx
}
```

匿名函数
``` js
var f = function (){
    xxx
}
```

函数方法体
``` js
new Function('a','b','a+b')
```
### Function.prototype
`.call()`
`.bind()`
`.apply()`

## Number() Boolean() 
 转换为对应的基本类型

# Array是什么？
`Array`对象是用于构造数组的全局对象，属于`Object`

我们一般声明Array采用的是`[gg, aga, aga]`，实际上这种方式等价于`new Array`,前者可以当作语法糖。

# 具体怎么用？
## 构造函数
Array()构造函数会根据传入的不同参数返回不同的数组
1. `let a = Arrray(1)`
    当参数只有一个数字的情况，参数代表数组的长度，返回等于参数长度的数组，这个数组其它元素都没有被声明，包括`0 in a`，它返回undefined。

    请注意，传入NaN会报错

2. `let a = Array('我就要看你要做什么')`
    当参数为一个非数字，返回一个长度为1且`a[0]=`所给字符串数组

3. `let a = Array(3,3)`
    当传入参数大于等于2，按顺序填充新数组并返回

## 方法

###  string.prototype
* `.trim()`
* `.spli()` 分解

### Array.prototype
`.push()` 推入一个(末尾)
`.pop()` 弹出一个(末尾)
`.shift()`
`.join()` 把数组连接起来，并加上分隔符形成字符串返回，默认为`,`号
`.concat(b)` 把数组连接起来返回新数组，也可以和空数组合并，等同于数组复制
`.forEach()` 遍历数组，详见后文
`.map()` 遍历数组，详见后文

Array的关键是继承了`Array.prototype`

### 数组遍历
如果是数组，可以使用`for(let i-0;i<obj.length;i++>)`
如果不关心是否是数组，可以使用`for (let i in obj)`遍历所有键值
还可以使用`forEach()`，`.map()`遍历

#### `forEach()`
`forEcah`接受一个函数，这个函数有是三个值
``` js
function (currentValue,index,array){
    //当前值，索引，数组
}
```
#### `.map()`
和forEach差不多，至少对value的改变会以新数组的形式返回

#### `a.filter()`
和map差不过，就是筛选，ture要，false不要

#### `.reduce()`
``` js
a.reduce( function (sum,n){return sum+=n},0) //返回6
```

### `a.sort()`
一般排序算法使用的都是快排
注意：js中默认排序是按照Unicode的码点进行排序，非常傻逼。

幸运的是，sort接受一个可选参数，支持自定义的比较算法。
函数必须是这种形式的
``` js
function (a,b){
    f (a < b ) {// 按某种排序标准进行比较, a 小于 b
        return -1;//小于0 a排前面
    }
    if (a > b ) {
        return 1;//大于0 b排前面
    }
    return 0;//相等不变，但实现不一
    }
}
```
因此比较数字大小的话，可以这样
``` js
a.sort(function (a,b){return a-b}) // 最小的在最前
```

``` js
a.sort(function (a,b){return b-a}) // 最大的在最前
```
其中forEach使用`this`表示调用者，可以使用`this`来获取调用者的属性

### 伪数组
如果像一个数组，且`_proto_`不等于`Araay.prototype`,那这就是一个伪数组
在js目前遇到的伪数组就是`arguments`这个表示传给函数的参数

