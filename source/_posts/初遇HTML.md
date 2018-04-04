---
title: 初遇HTML
date: 2018-04-02 16:00:23
tags:
categories: HTML
---
## HTML是什么？
HTML是由Tim Berners-Lee（李博士）创立的一种标记语言，早期很像如今在使用的Markdown,用一些具有语义的标签来帮助文档进行描述、跳转。李博士还创立了W3C(万维网联盟)，目前HTML都由此基金会进行维护，迭代。


## MDN
W3C负责制定HTML的推荐标准，HTML的标准文档在[这里](https://www.w3.org/TR/html52/),这个文档事无巨细的说明了HTML的等等细节，然而也因此显得特别冗长，难以阅读，**Mozilla基金会**就在此基础上归纳简化了原文档，推出了MDN，汇集众多的Mozilla基金会产品和网络技术开发文档，对HTML有疑惑时，可以方便的查询此文档。

## HTML的具体语法

HTML通常包含一对开始标签与结束标签
如：
``` html
<h1>标题<h1/>
```
`<h1>`为开始标签 `<h1/>`为结束标签
部分特殊的标签没有结束标签，我们称其为**空标签**
如：
``` html
<img href="#" alt="测试文本">
```
同样的此类标签还有
* <area\>
* <base\>
* <br\>
* <col\>
* <colgroup\>
* <command\>
* <embed\>
* <hr\>
* <input\>
* <keygen\>
* <link\>
* <meta\>
* <param\>
* <source\>
* <track\>
* <wbr\>

空标签不需要具有结束标签

在css里也有一个特殊的元素，叫做可替换元素

可替换元素的展现不由css来控制，而是由内容自己来决定。

典型元素有
* <textarea\>
* <input\>
* <img\>
* <object\>
* <video\>

具体参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)


