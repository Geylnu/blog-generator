---
title: javaScript入门
date: 2018-05-09 23:37:03
tags:
- IE6
categories: javaScript
---
# 历史
1991年 李爵士发明万维网
1992年 其同事发明html和css
1993年 成立w3c
1995年 网景公司成立 能够执行脚本

js之父Brendan Eich接到任务发明一种名为Mocha(咖啡配抹茶)的脚本语言，它需要看起来像java。js发布后，Unicode发布UTF-8，这就导致js不能完整兼容UTF-81996年微软模仿JS发明了Jscript。
在IE5.5微软推出JS发请求。
2004年Gmail利用这个功能做了一个网页上的程序，这让JS的地位大大提高。

这时就出现了前端（以js为生）

制约JS发展的问题
* 大量全局变量
* 缺少标准库

出现了ECMAScript 5,Es4胎死腹中
Es5做了个小升级，具体谷歌

rails社区（主要使用ruby）发明了coffeeScript，非常好用，ES6就迫在眉睫。

JS集大家之所长。原创之处不优秀，优秀之处非原创

# 兼容性
IE8部分兼容ES5,IE7及以下则不兼容

# 更新
ES7及以后，每年一更。

ESnext 还未正式公布的特性，可以使用webpack打包，使其支持。

# js数据类型
7 种数据类型
1. number
2. string
3. boolean
4. symbol（ES6新增）
5. null
6. undefined
7. object

## number
支持:
* 十进制 1
* 浮点数 .1 //0.1
* 科学计算法 1.23E2
* 二进制 0b011
* 八进制 0214542 //以0开头并且没有大于7的数
* 十六进制 0x5116D

## string
'你好' "你好"
### 转义
为了避免冲突使用
* \t 制表符
* \n 回车
* \\ \

多行字符串 \就可以了，但是如果后面接字符串，就会出现错误，且不容易发现。
也可以用 +号，+号最好，这样易于阅读的

ES6新增加了字符串语法
```  js
var a =`123456
7890`
```

## boolean
* ture
* false

### &&
全为真才为真，因为自动类型转换的缘故，实质上可以通过这种方式来简写`if`
``` js
var a = ture
var b = 'z'
a&&b //返回b的值

a = false
a&&b //返回a的值
```
在这里，a为ture的情况下,b的布尔值就等同整个函数的布尔值，所以返回b和返回运算后的布尔值是没有区别的，a为false的情况下同理

**注意，这种方式的`if`会难以阅读**
### ||
一个为真就为真
简写`if`的形式和上面相反

## symbol
有点类似java的enum

## null
指针不指向任何地址，也就是地址为0。

## undefined
变量初始化会被初始化为undefined，不使用null是因为设计时认为null是数字0，这可能导致误解。

## Object
除Object外都为基本类型,Object以hash表的方式组织对象,就是基本类型的组合。
注意`[]`表示声明一个空数组，`typeof []` 返回`Object`,但是以{0:'a',1:'b'}，也是可以以数组的形式访问,但是`instanceof Array`返回false

### 声明方式
只支持以字符串声明key，注意最后以`,`结尾是ES5的新特性，ES3不支持，IE7及一下只支持IE
``` js
var ob = {
    name: 'aarG',
    age: 18,
}
```
空字符也可以是key，调用使用['']
key不加引号视为标识符，不可以以数字开头
同时，如果key符合标识符，可以使用.来更方便的取得

### 更改方式
* `in` 判断key是不是在对象当中，js声明的全局变量实际都绑定在window上，可以用`in`判断是否声明过
* `delete` 删除一个键值对
* `for(var key in person)` 遍历键值

### 遍历方式
``` js
for (var key in Object){
    console.log(key )
}
```

``` js
Object.keys()
```
得到一个可枚属性组成的数组

# typeof
返回对象的类型和方法

注意 `typeof null`返回Object

# 去TM的js类型转换

## xx.toString()
* number 以字符串的形式输出，可选参数为以何种进制输出数字
    注意：以`1.toString()`的方式是不可行的
* boolean ture或者false
* null 空指针，直接报错
* undefined 直接报错
* Object 总是输出[object Object]，除非重写覆盖了`toString()`方法 
* {} 语法不支持

**小技巧：** xx + ""
js发现`+`运算符，会检测算子是否有运算符，有就转为字符串
* 1 + '1'   "1"
* ture + '' "true"
* {} + ''   0 //不懂，可能是对象转为0，空字符串再转为0

## window.String()
支持null undefined {},其它和toString()一致

## window.Boolean()
7种类型种，只有以下几种是false
* number NAN 0
* null
* undefined
* Object 全为true 
* String ''

快速转boolean `!!`

## window.Number()
* boolean 0或1
* null 0
* undefined NaN
* Object 调用valueOf，根据返回的值做决定，如果返回对象则为NaN
* string 如果为空字符串返回0，如果为数字则一个个解析，有非数字就返回NaN，其它返回NaN，特殊的是参数为011会被认为是11

## window.paparseInt()
* 参数为字符串，会从头开始寻找数字，直到遇到非数字为止，没有找到返回NaN
* 如果开头为空格，会省略空格
* 空字符返回NaN
* 所有其它类型返回NaN

## window.parseFloat()
和paparseInt()一致，只是不会取整


**小技巧**
* '1' - 0
* +'1'
* -()
都可以


# 内存模型
内存具有内存颗粒，一般每个内存颗粒有512Mb,多个内存颗粒组成内存容量

## 内存分配
浏览器(chrome)启动后会占用大量内存，并且每个新开页面都独立分配内存，这些内存又会分配给HTML+css，js，网络，定时器这些，一般JS内存只能申请到100Mb

JS种内存分为两个区，一个区分为数据区，一个区分为代码区，我们这里只考虑数据区。

数据区又分为两个部分，一个为stack(栈)，一个为heap(堆)。
js第一步先看声明了哪些变量，进行提升，这样方便内存分配
基本类型存在stack中，而Object由于数据量大，经常添加更改属性，不方便内存分配，所以Object就新开个部分存对象，声明对象后就把内存地址给变量，用指针来找对象，堆里新增加A数据是用引用的方式存储的，也就可以方便的添加删除了。

数字64位，字符16位（2个字节）

测试一下
``` js
var a = {n:1}
var b = a
a.x = a = {n:2}

a.x //undefined
b.x //[Object Object]
```
这里的关键是a.x这里，解释器确定变量是从左到右，先确定赋给谁，再进入表达式，所以a.x还是之前的a

### 垃圾回收
如果一个对象没有被引用，就会被标记可以回收，在浏览器需要的时候就会被回收。但在IE6，在多个对象一级一级引用的时候，即使父对象不存在了，子对象却不会被回收,这会导致内存泄漏，因此需要这样做
``` js
window.onunload = function{
    document.onclick = null
    //还要有所有的其它监听事件
}
```

### 深拷贝&浅拷贝
你变我也变就是浅拷贝
你变我不变就是深拷贝（所有的引用都被复制，和原对象及其子对象没有任何关联）