---
title: HTML常用标签
date: 2018-04-03 19:25:19
tags:
categories: HTML
---
*注意：由于Markdown会自动将换行转换为<br\>标签，为了避免换行，展现实际效果时的标签事实上都在同一行,这可能会带来一些空格的差异*

# <div\>
没有实义，主要用来分组，默认样式为块级，会单独占一行
``` html
<div>A</div>
<div>B</div>
```
**效果：**
<div>A</div><div>B</div>
***
# <span\>
没有实义，主要用来分组和其它作用，默认样式为内嵌，与其它元素共存。
``` html
<span>A</span>
<span>B</span>
```
**效果：**
<span>A</span><span>B</span>
***
# <h1\>
表示一级标题，默认会加粗，二级标签使用`<h2>`，以此类推
``` html
<h1>一级标题<h1>
<h2>二级标题<h2>
<h3>三级标题<h3>
```
效果：
<h1>一级标题<h1><h2>二级标题<h2><h3>三级标题<h3>
# <p\>
段落元素，表示一个文本的段落
```
<p>埃菲尔铁塔最初是作为 1889 年世界博览会的入口而兴建的。得名于设计它的著名建筑、结构师古斯塔夫·埃菲尔。埃菲尔铁塔从 1887 年开始动工，直到 1898 年才结束。是目前世界上访问人数最多的付费建筑物，每年有 700 万人登上铁塔的最高处。</p>
```
**效果：**
<p>埃菲尔铁塔最初是作为 1889 年世界博览会的入口而兴建的。得名于设计它的著名建筑、结构师古斯塔夫·埃菲尔。埃菲尔铁塔从 1887 年开始动工，直到 1898 年才结束。是目前世界上访问人数最多的付费建筑物，每年有 700 万人登上铁塔的最高处。</p>
***
# <strong\>
表示强调其中的内容，内容十分重要，会自动加粗，如果只是为了加粗，将其css中的`font-weight`属性设置为`"bold"`是更好的选择。**`em`**和其类似。

*由于Markdown只提供`**xxx**`的方式加粗文本，这被解释为<strong\>xxx</strong\>,所以我这里**`效果：`**的用法是不正确的。*
``` html
<strong>这个文本很重要</strong>
```
**效果：**
<strong>这个文本很重要</strong>
***

# <ul\>
 表示无序列表,用`<li></li>`来表示每一个例子/选项，与其相似的还有`ol`有序列表
 ``` html 
<ul>
    <li>葡萄</li>
    <li>橘子</li>
    <li>香蕉</li>
    <li>菠萝</li>
</ul>
 ```
**效果：**
<ul><li>葡萄</li><li>橘子</li><li>香蕉</li><li>菠萝</li></ul>
***

# <img\>
显示图片，`<img>`是一个空标签，没有带有带`/`的结束标签，且它也是一个可替换标签，它的相关样式由资源自己决定。
``` html
<img src="/2018/03/17/Markdown%E5%85%A5%E9%97%A8/jz.png" alt="紫罗兰永恒花园">
```
**效果：**
<a href="/2018/03/17/Markdown%E5%85%A5%E9%97%A8/jz.png" download="紫罗兰紫罗兰永恒花园">
<img src="/2018/03/17/Markdown%E5%85%A5%E9%97%A8/jz.png" alt="紫罗兰永恒花园" download></a>

***

# <a\>
可访问的链接，使用href指定访问的URL地址。也可以添加`download`属性，表示直接下载url的内容。

*download属性只在当前协议为http/https时有用*

**href有多种状态**
1. **绝对路径**
    以`/`开头，访问的是当前协议当前域名下的绝对地址
    ``` html
    <a href="/2018/03/17/Markdown入门/">我的第一篇博客</a>
    ```
    **效果：**
    <a href="/2018/03/17/Markdown入门/">我的第一篇博客</a>
2. **相对路径**
    使用`.`或`..`可以访问当前目录和上级目录，默认会加上`./`
    ``` html
    <a href="./龙王的工作.jpg">点击查看《龙王的工作》图片</a>
    ```
    **效果：**
    <a href="./龙王的工作.jpg">点击查看《龙王的工作》图片</a>

3. **当前协议URL**
    使用很当前一致的协议补全URL
    ``` html
    <a href="//baidu.com">点击访问百度</a>
    ```
    **效果：**
    <a href="//baidu.com">点击访问百度</a>
4. **指定协议**
    输入一个完整的URL
    ``` html
    <a href="mailto:yanglei1997630@gmail.com">给我发邮件</a>
    ```
    **效果：**
    <a href="mailto:yanglei1997630@gmail.com">给我发邮件</a>
5. **伪协议**
    伪协议是指以`javascript:`为属性值，点击之后执行javascript,
    这在某些方面有特殊的作用。
    如想点击`<a>`
    * **为空值**
    这种方法会导致重新刷新此页面
    ``` html
    <a herf="">这会导致页面刷新</a>
    ```
    **效果：**
    <a href="">这会导致页面刷新</a>

    * **为`#`**
    这种方法不会引起页面刷新，但会跳转到页面顶部
    ``` html
    <a herf="#">这会导致跳转到页面顶部</a>
    ```
    **效果：**
    <a href="#">这会导致跳转到页面顶部</a>

    比较好的解决办法是使用`###`。
    ``` html
    <a herf="###">不会发生任何事，但地址栏会轻微改变</a>
    ```
    **效果：**
    <a href="###">不会发生任何事，但地址栏会轻微改变</a>
    * **javascript空语句**
    另一个办法就是javascript为伪协议，执行一段空javascrpt语句
    ``` html
    <a href="javascript:;">不会发生任何事</a>
    ```
    **效果：**
    <a href="javascript:;">不会发生任何事</a>
***
# <iframe\>
框架元素，主要用来在当前页面嵌入一个新的HTML页面，iframe几乎相当于独立的标签页，拥有自己独立的历史记录等，目前已经很少使用，不过在部分情况下仍然有用处。
``` html
<iframe src="http://baidu.com"><p>iframe未显示时出现的文本<p></iframe>
```
**效果：**
<iframe src="http://baidu.com"><p>iframe未显示时出现的文本<p></iframe>


如想在iframe中打开你需要的页面,需要添加name属性
``` html
<iframe src="http://baidu.com" name="iframe"><p>iframe未显示时出现的文本<p></iframe>
<a href="http://qq.com" target="iframe">点击在iframe中打开qq.com</a>
```
**效果：**
<iframe src="http://baidu.com" name="iframe"><p>iframe未显示时出现的文<p></iframe><a href="http://qq.com" target="iframe">点击在iframe中打开qq.com</a>

除此之外，a标签还拥有其它target属性
1. **_black**
    在新标签页打开
    ``` html
    <a href="http://baidu.com" target="_blank">点击在新标签页内访问百度</a>
    ```
    **效果：**
    <a href="http://baidu.com" target="_blank">点击在新标签页内访问百度</a>

2. **_self**
    在本页内打开
    ``` html
    <a href="http://baidu.com" target="_self">点击在本标签页内访问百度</a>
    ```
    **效果：**
    <a href="http://baidu.com" target="_self">点击在本标签页内访问百度</a>
3. **_parent**
    在父级页面打开
    ``` html
    <a href="http://baidu.com" target="_parent">访问父级</a>
    ```
    **效果：**
    比如页面内嵌套一个iframe，iframe内有一个在这样的标签，是在上级页面打开。
4. **_parent**
    在根页面打开打开
    ``` html
    <a href="http://baidu.com" target="_parent">访问父级</a>
    ```
    **效果：**
    在frame嵌套frame的多级嵌套情况下，无论嵌套多少层，总是在最开始的根页面打开。

除此之外，因为自带的iframe边框比较丑，可以通过`frameborder=0`来禁用。

``` html
<iframe src="http://baidu.com" frameborder="0"><p>iframe未显示时出现的文本<p></iframe>
```
**效果：**
<iframe src="http://baidu.com" frameborder="0"><p>iframe未显示时出现的文本<p></iframe>
***
# <form\>
表单元素，用来提交给服务器表单数据，**<form\>**标签中包含有许多子标签，包括**<input\>** **<select\>** **<textarea\>**等等

form元素使用`action`确定请求的地址，使用`method`确定请求的方式，`method`拥有两种方式，分别为post和get,通过开发者工具可以方便的看见请求的类型,默认使用get请求
``` html
<form action="xxx" method="get">
<input type="submit" >
</form>
```
**效果：**
<form action="xxx" method="get">
<input type="submit" >
</form>

``` html
<form action="xxx" method="post">
<input type="submit" >
</form>
```
**效果：**
<form action="xxx" method="post">
<input type="submit" >
</form>

1. **<input\>**
input拥有许多type值，常见的有text、button、submit、checkbox、radio、password、file等。

    * **submit**
    提交表单的内容
    ``` html
    <form action="xxx" method="post">
    <input type="text" name="userName">
    <input type="text" name="password">
    <input type="submit" value="登录" >
    </form>
    ```
    **效果：**
    <form action="xxx" method="post">
    <input type="text" name="userName">
    <input type="text" name="password">
    <input type="submit" value="登录" >
    </form>

    其中，具有想提交内容的标签必须具有name，否则无法提交内容。
    使用get提交，会直接把内容加到路径后，使用post提交，会添加在header第四部分

    如果表单中没有form标签，没有指定type的<button\>标签也具有提交功能
    ``` html
    <form action="xxx" method="post">
    <button>提交<button>
    </form>
    ```
    **效果：**
    <form action="xxx" method="post">
    <button>提交</button>
    </form>

    * **checkbox**
    多选框
    ``` html
    <span>喜欢吃的水果：<span>
    <label><input type="checkbox" name="lovaFruit" value="苹果">苹果</label>
    <label><input type="checkbox" name="lovaFruit" value="草莓">草莓</label>
    <label><input type="checkbox" name="lovaFruit" value="橘子">橘子</label>
    ```
    **效果：**
    <span>喜欢吃的水果：<span><label><input type="checkbox" name="lovaFruit" value="苹果">苹果</label><label><input type="checkbox" name="lovaFruit" value="草莓">草莓</label><label><input type="checkbox" name="lovaFruit" value="橘子">橘子</label>

    * **radio**
    单选框
    ``` html
    <span>你是男孩还是女孩？</span>
    <label><input type="radio" name="gender" value="girl">女孩</label>
    <label><input type="radio" name="gender" value="boy">男孩</label>
    <label><input type="radio" name="gender" value="LGBT">跨性别者</label>  
    ```
    **效果：**
    <span>你是男孩还是女孩？</span><label><input type="radio" name="gender" value="girl">女孩</label><label><input type="radio" name="gender" value="boy">男孩</label><label><input type="radio" name="gender" value="LGBT">跨性别者</label>
***
2. **<select\>**
下拉列表，使用`disabled`不可选中，`selected`默认选中，`multipl`可以多选
``` html
<select>
<option selected>四川</option>
<option>广东</option>
<option>湖南</option>
<option disabled>台湾</option>
</select>
```

**效果：**
<select><option selected>四川</option><option>广东</option><option>湖南</option><option disabled>台湾</option></select>
***

3. **<textarea\>**
文本域，主要用来输入大量文本，可以使用`cols`指定列数，`rows`指定行数，但是这与字符长度不完全匹配

``` html
<textarea clos="50" rows="50">在这里输入</textarea>
```
<textarea clos="10" rows="10">在这里输入</textarea>

***
# <table\>
表格标签，表格标签分别具有三部分，分别是头`<thead>`，身体`<tbody>`，脚`<tfoot>`，还有管内容的标签`<th>`，`<tr>`，`<td>`。

``` html
<table>
    <thred>
        <th>姓名</th><th>学号</th><th>成绩</th>
    </thred>
        <tr>
            <td>张三</td><td>0101</td><td>99</td>
        </tr>
        <tr>
            <td>李四</td><td>0102</td><td>98</td>
        </tr>
        <tr>
            <td>王麻子</td><td>0103</td><td>95</td>
        </tr>
    <tbody>
    </thred>
    <tfoot>
    </thred>
</table>
```

<table><thred><th>姓名</th><th>学号</th><th>成绩</th></thred><tr><td>张三</td><td>0101</td><td>99</td></tr><tr><td>李四</td><td>0102</td><td>98</td></tr><tr><td>王麻子</td><td>0103</td><td>95</td></tr><tbody></thred><tfoot></thred></table>

还可以通过`<colgroup>`及其子元素`<col>`指定每一列的宽度
***
*OK*

    


