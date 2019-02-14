---
title: 虚拟DOM
date: 2019-01-15 11:13:39
tags:
- 虚拟DOM
categories: JavaScript
---
# 虚拟DOM是什么？
虚拟DOM其实就是在原有DOM的基础中，在js中再做一层DOM的抽象。
# 虚拟DOM有什么用？
在一些需要大量更改DOM的情况下，比如更新表格内的内容，重新排序等，会引起重绘、重排、引起大量的性能消耗，虚拟DOM就是针对这个问题的一个解决方案，在更改前都是针对虚拟DOM进行操作，不对真实DOM进行更改，更改完成后使用diff算法对比DOM树，只操作需要更改的DOM节点，减小 性能开销。
# 虚拟DOM怎么用？
首先需要创建一个类似的虚拟DOM抽象数据结构
``` js
class VNode {
  constructor(tag, children, text) {
    this.tag = tag
    this.text = text
    this.children = children
  }

  render() {
    if(this.tag === '#text') {
      return document.createTextNode(this.text)
    }
    let el = document.createElement(this.tag)
    this.children.forEach(vChild => {
      el.appendChild(vChild.render())
    })
    return el
  }
}

function v(tag, children, text) {
  if(typeof children === 'string') {
    text = children
    children = []
  }
  return new VNode(tag, children, text)
}
```
这里定义一个简单虚拟node类，拥有本元素和子节点，拥有`render()`函数。

改变使用diff算法对比
``` js
function patchElement(parent, newVNode, oldVNode, index = 0) {
  if(!oldVNode) {
    parent.appendChild(newVNode.render())
  } else if(!newVNode) {
    parent.removeChild(parent.childNodes[index])
  } else if(newVNode.tag !== oldVNode.tag || newVNode.text !== oldVNode.text) {
    parent.replaceChild(newVNode.render(), parent.childNodes[index])
  }  else {
    for(let i = 0; i < newVNode.children.length || i < oldVNode.children.length; i++) {
      patchElement(parent.childNodes[index], newVNode.children[i], oldVNode.children[i], i)
    }
  }
}
```
实际远远比这个复杂，拥有更多细节要处理，不过理解的话够用了。