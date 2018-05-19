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