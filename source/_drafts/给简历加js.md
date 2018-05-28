---
title: 给简历加js
tags:
---
跟随滚动变化的叫sticky(粘的) navbar

自动高亮

suto scroll smoothly

menu 下滑菜单

auto hide aside 自动化隐藏侧边栏

gapless slides 无缝轮播

loading animation

animate when scroll 滚动出现

# loading 动画
load动画实现很简单，直接使用animation就可以了，创建好html以后，我们可以可以来创建动画可

动画其实都是由关键帧构成的，在css中声明关键帧可以采用这种方式
``` css
@keyframes s{
    0%{
      width: 0px; height: 0px; opacity: 0.4;
    }
    100%{
      width: 100px; height: 100px; opacity: 0;
    }
  }
```

这里的`opacitty`指透明度

然后再需要的选择器下方写
``` css
animation: s 2s linear infinite;
```
其中`linear`指线性增长 `infinites`指无限循环。


创建好以后我们就需要制作loading的逻辑了，我们该怎么判断loading是否已经加载完毕？
如果什么都不做，至少把css写到页面里，在最后使用js再关闭，并不会显示loading动画，我认为这可能是因为dom的执行很快，load只显示了一瞬间
我采用的是`window.onload`在整个页面加载完以后才不显示loading，但是这里缺点是需要所有图片都加载完，我觉得解决办法是监听css是否加载完，或者图片动态加载

## git 使用
git commit --amend  -v .
重写上一次commit 

# menuSlider
用after来添加下划线动画，这样比较好加动画，注意transition动画是根据属性变化，anmation时根据自己定的属性进行变化，最典型的就是:hover之后transition会有回归初始状态的动画，anmation不会。

## 扩展
我们把transition加在变化的属性上面和原来的属性上面有什么区别呢？
我用hover试了下，如果transition写在变化前的选择器上面，那么动画既有从无到有，也有从有到无，时间一致，如果只写在变化后的选择器上，则只有从无到有，不会返回初始状态，时间自己定，如果两者都写了，则变化前的选择器决定回归时的速度样子，变化后的选择器决定从无到有的样子

# 下拉菜单
使用绝对定位可能会遇见子元素换行的原因，这是因为超出了父元素的高度，加个`white-space: nowrap`就可以啦！

菜单有多个选项，我们需要监听每一个选项，我们可以直接给菜单这个父元素绑定事件，也可以为每一个选项设置需要监听，为了方便我们可以设置class 这里涉及到了js事件会冒泡，父元素的同事件也会触发

nextSibling，下一个元素，可能会出现BUG，比如会选择到回车，空格等

nodetype节点类型 3就是文本

记住，tagName返回标签一定返回大写

display: none 会与transition冲突
解决办法可以采用visiblity opacity

在js中使用选择器
``` js
 document.querySelectorAll('nav.menu > ul > li')
 ```
 记住如果需要使用`a[herf="#xx"]`这里herf一定要是双引号

 # 跳转到指定地点
 使用锚点，但因为悬浮窗可能会有BUG，可以给元素加padding等解决

 除此之外就是监听标签，然后
 需要阻止a的默认事件，使用`e.preventDefault( )`

 `a.getAttribute('href')`获得属性名
 直接获取的herf浏览器会自动补全为链接

 `getBoundingClientRect()` 获取相对于视口的各种值

 `offsetTop` 获取绝对于文档流顶部的高度

 `window.scrolloTo(x,y)`

 # 滚动动画
 ``` js
 var a = setTimeout(function , 1000) //设定延时
 var b = setInterval(function , 1000) //设定延时一致做

 //返回这个定时器的id

 window.clearInterval(b)//通过id清除定时器
```
## 箭头函数
语法
``` js
()=> {
  n=n+1
  xxx，函数
}

```

tweenjs 缓动
可以直接引入js

Math.abs(绝对值)

动画帧率可以让浏览器自己确定
``` js
requestAnimationFrame(animate);
```

 # 区域高亮、
 可以采用给特殊标签属性来找到标签
 ``` html
 <div special></div>
 ```

 ``` css
 div[special]
 ```
就可以找到标签啦！js中也可以按照前文怎么做
`.parentNode` 找爸爸
`.children`找儿子们

 # 滑动效果

