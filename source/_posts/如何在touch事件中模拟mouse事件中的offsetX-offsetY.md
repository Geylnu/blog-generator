---
title: 如何在touch事件中模拟mouse事件中的offsetX offsetY
date: 2019-07-29 22:49:31
tags: 
- 移动端兼容
- 原生事件
categories: JavaScript
---
# 1. 起因
最近想制作一个在散开的纸面画画的效果，纸面会按一定角度倾斜，就像下面这样↓
![canvas](./canvas.png)
也就是给canvas元素加了`transform: roate(-2deg)`

# 2. 电脑端实现
在电脑端很容易实现,鼠标事件中有`offsetX Y`，能够获得鼠标位置相对于目标节点的位置，可以简单理解为相对于元素左上角的坐标。获得坐标后，再按坐标绘制到Canvas上，一切就OK了，不过到了移动端就有点麻烦了。

# 3. 移动端实现
移动端touch事件有以下几个属性
* ClientX Y 相对于视口的坐标
* pageX Y 相对于页面左上角原点的坐标
* screenX Y 相对于屏幕的坐标标
* movementX Y 相对于上一次坐标的坐标

然而就是没有`offsetX Y`，怎么办，自己模拟试试？

## 3.1 第一次模拟
1. 获得`pageX Y`
2. 通过`offsetTop left`计算元素左上角的顶点位置`vertex`
3. 计算相对坐标

代码如下：
``` js
function getVertexPosition(el) {
    let currentTarget = el
    let top = 0
    let left = 0
    while (currentTarget !== null) {
        top += currentTarget.offsetTop
        left += currentTarget.offsetLeft
        currentTarget = currentTarget.offsetParent
    }
    return { top, left }
}

let vertex = getVertexPosition(canvas)
canvas.addEventListener('touchmove',(e)=>{
    let offsetX = e.pageX-vertex.left
    let offsetY = e.pageX-vertex.top
})
```
在页面没有设置任何`transform`属性的情况下，这个代码是生效的，能获得正确的坐标点。
然而如果父元素或目标元素有任何`transform`属性，坐标就会错误，比如本文中的canvas元素，设置了`transform: roate(-2deg)`,坐标就发生了偏移。
随后搜索了一下是否有类似的解决方案，结果都是这样的代码，都不能对`transform`元素生效。

## 3.2 偏移原因
偏移原因是因为变换后，元素的坐标轴已经改变了，而我们得到的坐标还是变换前的坐标，自然而然就发生错误了。
![坐标图](./reason.png)
如图所示，实际触摸点是绿色，本应也绘制在绿色坐标点，但是我们现在的坐标系仍然是之前的坐标系，就导致了绘制点相对于原来的坐标点绘制，也即紫色，导致绘制偏移。
解决办法也很简单，将触摸点按照新坐标系计算得到值，就是真实的相对坐标了。
**步骤：**
1. 获得点击坐标（pageX pageY）
2. 获得旋转角度 旋转中心
3. 将旋转中心于元素顶点坐标相加，得到整个页面的旋转中心
4. 旋转相同负角度（逆运算），将点击坐标还原到真实的坐标系
5. 减去顶点坐标，获得真实相对坐标。

## 第二次模拟
1. 首先获得计算后的样式，获得`transform`相关属性
``` js
let style = window.getComputedStyle(el)
console.log(style) 
```
    获得的`transform`属性如下
    ``` 
    transform: "matrix(0.999391, -0.0348995, 0.0348995, 0.999391, 0, 0)"
    transformBox: "view-box"
    transformOrigin: "160px 240px"
    transformStyle: "flat"
    ```
    关于`transform:matrix`以及如何理解查看[张鑫旭的文章](https://www.zhangxinxu.com/wordpress/2012/06/css3-transform-matrix-%E7%9F%A9%E9%98%B5/)。
    简而言之，就是`transform`属性都是通过`transform:matrix`进行矩阵计算得到坐标的。

2. 解析transform属性
    由于这里涉及到矩阵运算，需要引入[math.js](https://mathjs.org/)

    ``` js
    let transform = style.transform
    let transformOrigin = style.transformOrigin

    let origin = { x: 0, y: 0 }
    let matrix = math.ones([3, 3])
    if (transform !== 'none') {
        let originArray = transformOrigin.split(' ')
        origin.x = parseInt(originArray[0])
        origin.y = parseInt(originArray[1]) //矩阵的坐标变化是基于变换中心得。

        let matrixString = transform.match(/\(([^)]*)\)/)[1]
        let stringArray = matrixString.split(',')
        let temp = []
        stringArray.forEach((value) => {
            temp.push(parseFloat(value.trim()))
        })
        temp = [
            [temp[0], temp[2], temp[4]],
            [temp[1], temp[3], temp[5]],
            [0, 0, 1],
        ]

        matrix = math.inv(temp) //进行逆矩阵
    } else {
        matrix = [[1, 0, 0], [0, 1, 0], [0, 0, 1]]
    }
    ```
    这样我们就得到了原变换矩阵的逆矩阵和变换中心，就能将触摸点正确的还原到原坐标系了

3. 计算
``` js
function computPosition(obj){
    let { matrix, origin, vertex: { top, left },ponit:{x,y} } = obj
    x = x - left - origin.x
    y = y - top - origin.y
    let result = math.multiply(matrix, [x, y, 1])
    x = result[0] + origin.x
    y = result[1] + origin.y
    return (x,y)
}
```
4. 实际实现
    1. 由于子元素会受到父元素变换影响，因此需要遍历所有父元素
    2. 由于`getComputStyle()`和获得顶点坐标有较大性能消耗，最好将相关参数缓存起来
    实际代码实现见此<https://github.com/Geylnu/touch-offset>

## 其它
W3C给出了没有为touch事件添加`offsetX Y`的原因<https://github.com/w3c/touch-events/issues/62>
