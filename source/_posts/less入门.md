---
title: less入门
date: 2018-07-26 14:41:48
tags: 
- 前端工程化
- less
categories: css
---
# 变量
## 定义变量
* 颜色
``` less
@bgColor: #34c5c5;

body{
    background: @bgColor;
}
```

* url
``` less
@url: "./xxx/"

body{
    background: url("@{url}dog.png")
}
```
最后url就会被合成，方便改路径

* 声明值
``` less
@background: {background:red;};

body{
    @background();
}
```

# 嵌套
可以把选择器写在括号中，表示下一级选择器
``` less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```
解析完毕后
``` css
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```
&表示当前选择器的父选择器
# 混合
可以在css中使用其它选择器里定义的属性，比如
``` less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered;
}
```


