---
title: 'Cookie,Session与LocalStorage'
date: 2019-02-02 19:42:11
tags:
- cookie
- LocalStorage
- Session
categories: 网络协议
---
# Cookie由来
谈到Cookie不得不谈到http协议，wiki上的定义⬇
>设计HTTP最初的目的是为了提供一种发布和接收HTML页面的方法

由于这个设计目的，早期http协议被设计为无状态的协议，一个HTTP协议通信过程往往是建立连接>传输内容>关闭连接，整个过程十分简单。

然而万维网发展的比想象中快太多，HTML不再只是单纯的文档，还被用于交互，有了登陆注册等保存状态的需要，然而http协议是无状态的，这个场景需要下，Cookie就诞生了。

# Cookie具体是怎么样的？
Cookie其实就是一段文本信息，由浏览器储存在本地硬盘上，服务器通过`Set-Cookie`设置Cookie，浏览器通过`Cookie`字段附上设置的Cookie信息。

emm，还是看一个完整的请求吧
***
**响应**
```
HTTP/1.1 200 ok
Date: Sat, 02 Feb 2019 14:24:28 GMT
Content-Type: text/html
Content-Length: 182
Connection: keep-alive
Set-Cookie: tgw_l7_route=80f350dcd7c650b07bd7b485fcab5bf7; Expires=Sat, 02-Feb-2019 14:39:28 GMT; Path=/
...
```
**下次请求**
```
GET / HTTP/1.1
Host: example.com
Cookie: tgw_l7_route=80f350dcd7c650b07bd7b485fcab5bf7
...
```

## `Set-Cookie`
服务端通过在响应头中添加`Set-Cookie: xxx=xxx`的方式以键值对的形式设置Cookie，除此之外还有其它字段可以控制Cookie的字段
* `Domain`
指定Cookie从属于那个域名，在请求对应域名时会带上这个Cookie，默认为当前文档域名，**不包含子域名**，若主动设置`Domain`,则一般会包含子域名，例如设置了`Domain=geylnu.com`,则`blog.geylnu.com`也是可以访问这个`geylnu.com`的Cookie。

* `Path`
在域名符合的前提下，如果请求的路径与`Path`的路径相匹配就在请求中附上这个Cookie，路径匹配从前到后匹配，以`/`作为分隔符

* `Expires`
在指定日期Cookie到期，Cookie到期后服务器会自动删除这个Cookie，设置时间格的式为`Thu, 01 Jan 1970 00:00:00 GMT`这样的格林尼治标准时间，没有设置该属性或`Max-age`，Cookie会自动在浏览器关闭后清除。值得注意的是响应头中的时间是基于服务器时间生成的，而客户端的时间可能与服务端不一致，这种情况下使用`max-age`属性设置过期时间是一个更好的选择。

* `Max-age`
在指定秒后Cookie到期，作用等同于`Expires`，当同时设置了`Expires`和`Max-age`时，`Max-age`优先级更大；由于不直接设置时间，规避了服务端时间和客户端时间可能不一致的问题。

* `Http-only`
Cookie只允许浏览器发出请求时在`Cookie`中附带上，通过`Set-Cookie`修改，不允许通过js脚本得到拿到，这可以有效的防止[XSS攻击](https://zh.wikipedia.org/wiki/%E8%B7%A8%E7%B6%B2%E7%AB%99%E6%8C%87%E4%BB%A4%E7%A2%BC "什么是xss攻击？")

* `Secure`
指定该Cookie只在加密协议（https）才能把这个Cookie发送到服务器，如果设置该属性是通过http协议设置的，浏览器会自动忽略该属性（自动纠错，因为http下设置这个属性没有意义）。如果协议为`https`，该属性默认打开

一个完整的Cookie设置示范：
```
Set-Cookie: sessionId=hihihi; Expirers=Wed, 01 Jan 2020 00:00:00 GMT; Max-age=10000; Http-only; Secure;
```
## js操作方式
读
``` js
document.cookie //返回"xx=xxx; yy=yyy"
```
写
``` js
document.cookie = 'xxx=hihihi'
```
这里由于设定了`get`和`set`属性，读是一次读写全部Cookie，写为一个个Cookie单独设置。

## Session
好了，现在http协议支持将状态保存在客户端了，之后每个客户端都是有名有姓的人了。
但是客户端保存状态信息是不可靠的，客户端可以修改自己本地的Coookie值，Cookie值在非`https`加密协议的情况下，也容易被截获获取到敏感信息（这里吐槽下我校教务系统，密码明文直接写在Cookie中）。

对应的解决方法就是在服务端也维护对应的状态，大部分状态信息都由服务端记录，客户端只需要记录一个由服务端随机生成的sessionId用于服务端识别，这就是Session了，一般Session储存在内存，也有其它实现，甚至无Session的方式。

# LocalStorage
由于Cookie在每个请求中都会被添加，网络开销很大，很多非敏感状态信息也不需要服务器来存储，Cookie的大小有也很小，只有4kb；为了解决这个问题，HTML5规范就提出了LocalStorage，LocalStorage和cookie一样都是存在本地电脑磁盘上，数据形式也是hash表，存储形式也都是字符串。

不过和cookie不同的是LocalStorage不会跟随请求发送，仅限通过浏览器api操作，储存空间一般为5M大小，远大于cookie可以使用的空间，并且没有过期期限，除非用户主动清理会一直存在，通常在能用LocalStorage的地方尽量都用LocalStorage，前端不应该使用Cookie。

# sessionStorage
sessionStorage与LocalStorage基本相同，只是sessionStorage过期不同，另外还有个显著的特点：sessionStorage会在每个窗口创建时初始化一个新的会话，也就是即使是同一个域名下不同窗口，sessionStorage也是不一样的。


## js操作方式

``` js
localStorage.setItem('key','value')
localStorage.remove('key')
localStorage.getItem('key')

localStorage.clear()//移除全部
```