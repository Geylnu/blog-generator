---
title: ajax初探
date: 2018-06-11 23:05:10
tags:
- ajax
categories: javaScript
---
# 如何向后端请求数据？ 
* form表单提交，使用method指定提交方式,action为提交的地址。
* a标签点击能发请求
这两者都要刷新页面或者新开页面

* img可以发请求，但请求的必须是图片 不需要增加到页面
* script和link也可以发起请求，但link和script只能加载指定格式，但是需要加到页面

之后，ie5在js中引入ActiveX对象（api），使js可以直接发起http请求
随后其它浏览器也跟进，取名`XMLHttpRequest`
不到一年，谷歌推出gmail.com,这里可以说前端真正诞生了

ie6后来也成为了安装最多的浏览器60%,ie因此就膨胀了,随后微软拆开ie6的开发人员，只更新安全功能，让chrome也跟了上来

## ajax
Jesse James Garrett 将一下技术取名AJAX（异步的javaScript和 XML
1. 使用XMLHttpRequest发请求
2. 服务器返回xml格式的字符串
3. js解析xml，并更新局部页面(现在用JSON)

使用`XMLHttpRequest`的方法
``` js
myButton.addEventListener('click',()=>{
    let request = new XMLHttpRequest()
    request.open('GET','/XXX') //初始化配置  请求方式忽略大小写  默认异步
    // request.onreadystatechange
    request.send() //发送
    console.log(request.responseText) //响应内容
})
```

网络响应的时间会很长，这段时间已经够javaScript执行很多任务了，所以需要使用异步

readystate
值|描述
:--:|:--:
0|`open()`方法还没有被调用
1|`send()`方法还没有被调用
2|`send()`方法已被调用，响应头和响应状态码已经返回
3|正在下载响应体
4|请求全部完成

onreadystate 要注意放在前面

一般不用管300状态码. 浏览器会自动处理
## 解析数据
可以使用DOM api解析xml，但是很复杂，现在已经过时了。
1所以用什么更好的方法更好的表示数据呢？

后来道格拉斯·克罗克福发明了一种轻量的资料语言[JSON](https://zh.wikipedia.org/zh-hans/JSON)

### JSON语法
JSON支持`object` `array`两种数据组织方式，值支持`string``number``object``array``true``false``null`
没有`undefined`

json没有变量，不支持引用，没有原型链
* string 必须具有`""`
支持转意0符`\\`

注意，永远返回的是字符串，只是将其解析为对象
解析:`JSON.parse(string)`


## 同源策略
同源策略要求协议+端口+域名一模一样才允许发起AJAX请求，保证安全

本质是禁止一个域名的js在未经允许的情况下，不得读取另一个域名的内容。但浏览器并不阻止向另一个域名发送请求。

不是同源的网页不能使用ajax发起请求（实际上发起了，只是不能读取内容），其他都可以

简单规避方法就是使用CORS,服务端设置`Access-Control-Allow-Origin: xxx`响应头 表示允许xxx跨域访问

## 设置header
必须在open()和send()之间调用。
send()可以为post 设置请求内容

getAllResponseHeader获得所有响应头

tcp进行分片传输，因此会先拿到先拿到最开始的状态包，就可以先判断是否需要继续接受等之后的逻辑


传多个变量很容易让别人不知道自己具体想传什么，这里可以采用传个对象的方式，这样就有名字了


设置header 接受多参数

析构赋值，把名字一样的赋到同名变量里
``` js
param = {a:'aga',b:'gag',c:'aga'}
let {a,b,c} = param
```
对于函数可以采用这个方式
``` js
function ({a,b,c}){//取得参数中的abc

}
```

还有很多方式比如对象键名为字符串，不可以调用变量，但是`[x]: sss`可以

自己写回调可能不同库不一样，回调函数名不统一，所以需要规范出来

Promise写法：
``` js
.then() //第一个是成功第二个是失败
```

具体写法
``` js
return new Promise(function (resolve,reject){
 xxx
})
```