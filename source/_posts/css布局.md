---
title: css布局
date: 2018-04-22 15:29:46
tags:
categories: css
---
# 左右布局
``` html
<html>
<body class="clearfix">
<div class="left">左边的东西</div><div class="right">右边的东西</div>
</body>
</html>
```

``` css
div{
  border: 1px red solid;  
}

.clearfix::after{
    content: '';
    display: block;
    clear: both;
}

.left{
  float:left;
  width:49%;
}

.right{
  float:right;
  width:49%;
}
```

效果见[jsBin](http://js.jirengu.com/xilocahoru/1/watch?html,css,output)

# 左中右布局
中间加个`inline-block`的`div`并居中。
``` html
<html>
<body class="clearfix">
<div class="left">左边的东西</div>
<div class="center">中间的东西</div>
<div class="right">右边的东西</div>
</body>
</html>
```

``` css
div{
  border: 1px red solid;  
}

.clearfix::after{
    content: '';
    display: block;
    clear: both;
}

.left{
  float:left;
  width:20%;
}

.right{
  float:right;
  width:20%;
}

body{
  text-align: center;
}

.center{
  display:inline-block;
  margin-left:auro;
  margin-right:auto;
  vertical-align: top;
}
```

效果见[jsBin](http://js.jirengu.com/welotibebi/3/watch?html,css,output)

# 水平居中
* 设置左右边距为auto;
``` css
margin: 0 auto;
```
    优点：简单方便
    缺点：无法对已float元素生效,元素必须具有固定的宽度

* 设置为`inline-block`并`text-align: center`
``` csss
/*父元素*/
text-align: center
/*子元素*/
display: inline-block
```
    inline元素可以直接使用`text-align: center`
    优点：可以对float元素生效
    缺点：容易出BUG，一般需要`vertical-align: top;`修复


# 垂直居中
* vertical-align: center
    缺点:没怎么用，不知道

* 设置行高与高度相等
``` css
height: 50px;
line-height: 50px;
```
    缺点：只适合内联与内联块级元素

# 小技巧
* 使用`::before`和`::after`可以方便的创建新元素再给它加样式
* 可以重新设置盒模型`box-sizing`来方便的设置固定的比例的宽度等
* 可以使用伪类来选择特定状态/性质的元素，如：`:nth-chlid(even)`,选择偶数子元素
