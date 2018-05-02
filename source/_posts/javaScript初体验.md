---
title: javaScript初体验
date: 2018-04-24 23:48:52
tags:
categories: javaScript
---
java

监听事件
``` javaScript
div.onekeypress = function(aga){
    console.log('你按压了一个键')
}
```

js创建容器，循环后只会留下最后的内容。
``` javaScript
for(var i=0,kbd;i<keys[index].length;i++){
            kbd = document.createElement('kbd')
            div1.appendChild(kbd)
            kbd.textContent = keys[index][i]
        }
```
要取得正确的元素，可以从传入的形参判断。

localStorage
只允许传入字符串