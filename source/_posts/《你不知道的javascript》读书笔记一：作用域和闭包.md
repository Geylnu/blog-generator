---
title: 《你不知道的javascript》读书笔记 一：作用域和闭包
date: 2020-08-16 19:36:28
tags:
  - 你不知道的Javascript
  - 读书笔记
  - 作用域
  - 闭包
categories: Javascript
---

其实之前已经把《你不知道的 javascript》上卷读完了，但是其实只是囫囵吞枣，看的很初略，没有自己的思考，再到今天其实有点忘得差不多，这次重新精读一次，记录下自己的思考。

## JS 是如何处理变量声明的？

js 入门会遇见很多很神奇的点，很著名的就是变量提升

- var 变量提升
  ```js
  b = 3;
  var b;
  console.log(b); // 3
  ```
- 函数声明提升
  ```js
  hosting(); // 'function hosting'
  function hosting() {
    console.log("function hosting");
  }
  var hosting = 0;
  ```
  在这里使用`var`声明的变量标志符被提升了，甚至可以在声明前调用它，函数也是同理，并且函数声明的值也被一同提升了。

上面的代码可以翻译成类似的同理代码:

- var 变量提升
  ```js
  var b = undefined;
  b = 3;
  console.log(b); // 3
  ```
- 函数声明提升
  ```js
  var hosting = function hosting() {
    console.log("function hosting");
  };
  hosting(); // 'function hosting'
  hosting = 0;
  ```
  了解了之后，其实我们还会冒出更多的疑问，为什么会这样？为什么这么设计？
  《你不知道的 javascript》正好解答了这一部分。

### 编译原理

在了解 JS 如何变量声明前，我们还需要一些预先的知识，了解代码是如何编译的。编译器前端大致有下面几个流程：

- #### 词法分析

词法分析就是将字符串分解成适合理解的词法单元，例如 `23 + 10 * 2` 就应该是 5 个词法单元`23` `+` `10` `*` `2`，词法分析有两种方式，一种是基于状态机的，另一种是基于正则的，两者是等同的。

- #### 语法分析

语法分析实际上做的就是将连续的词法单元分析成一个个语意块，就像将一串连续的汉字理解一个个词语一样，最终输出成为一颗抽象语法树，上面词法单元分析的结果应该是这样

```JSON
{
  "type": "ExpressionStatement",
  "expression": {
    "type": "BinaryExpression",
    "left": {
       "type": "Literal",
      "value": 23,
    },
    "operator": "+",
    "right": {
      "type": "BinaryExpression",
      "left": {
        "type": "Literal",
        "value": 10,
      },
      "operator": "*",
      "right": {
        "type": "Literal",
        "value": 2,
      }
    }
  }
}
```

整个语法结构是这样的

```
ExpressionStatement =
 BinaryExpression  =
 Literal(23) + BinaryExpression =
 Literal(23) + (Literal(10) * Literal(2))
```

- #### 语义分析

在通过语法分析后，事实上代码已经可以转换成机器代码跑起来了， 但是为了避免一些语义错误和代码优化，就可以在生成抽象语法树后再进行一些处理。
比如前端打包中`Tree shaking`实际上就是在这个阶段对代码进行静态分析，移除不必要的代码。

了解了代码的编译过程，我们就可以从编译原理角度来理解编译器做的这些小动作 了。

### 作用域

作用域的定义在书中定义的很明白

> 作用域负责收集并维护由所有声明的标识符（变量）组成的一系列查询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。
> 它决定了变量如何被查找及访问。

#### 为什么要变量提升？

这是一个常见的声明

```js
var a = 10;
```

声明语义应该是这个样子的：

> 为一个变量分配内存，将其命名为 a，然后将值 2 保存进这个变量。

如果重复声明，在 C、Java 中，会直接报错，也是符合常理的做法。

然而在 js 中，却不是这样。首先编译器会预先扫描所有的变量声明，在首次声明时会将变量添加进作用域，并将变量默认值置为`undefined`,如果遇到再次声明，则忽略它。函数声明与变量基本一致，但是在编译时就会确定函数的值。

很对人都很奇怪为什么 js 会如此设计，为什么这样进行编译？这样做有什么好处吗？
实际上 js 语言的创造者 BrendanEich 其实在[twitter](https://twitter.com/BrendanEich/status/33403701100154880)说过变量提升的原因。Quora 上[一篇文章](https://www.quora.com/Why-does-JavaScript-hoist-variables)也说的很清楚。

> yes, function declaration hoisting is for mutual recursion & generally to avoid painful bottom-up ML-like order

> A bit more history: \`var\` hoisting was an implementation artifact. \`function\` hoisting was better motivated

大意就是 BrendanEich 在设计 js 语法时，十分讨厌 LISP 命令式的、自上而下的函数书写风格，如果没有函数声明提升，我们的代码将是这样：

```js
function doSomethingB(){
  doXXX...
}

function doSomethingA(){
  doSomethingB()
}

function init(){
  doSomethingA()
}
```

我们阅读代码时，通常是自上而下的，然而如果没有变量提升，就不得不自上而下声明函数，自上阅读代码。因此 js 编译器会预先做变量提升，类似于类似于 C\C++代码中头文件的作用。而与此同时，作为函数声明提升的副作用，变量声明也被提升了，也就成为了现在的局面。

#### 函数作用域与词法作用域

在 ES6 之前，仅有全局作用域和函数作用域，这在某些情况下就会造成一些问题，这也在一些经典面试题中出现过

```js
function log123() {
  for (var i = 0; i < 3; i++) {
    window.setTimeout(() => {
      console.log(i + 1);
    }, i * 1000);
  }
}

log123(); // ??
```

这里期望输出行为应该在 1s、2s、3s 时分别输出 1、2、3
而实际输出是在 1s、2s、3s 时都输出 4。

在 ES6 中，解决这个办法很简单，将`var`变成`let`即可，
在 ES6 前有两种解决办法：

```js
// 第一种 参数传递
function log123() {
  for (var i = 0; i < 3; i++) {
    window.setTimeout(
      (count) => {
        console.log(count + 1);
      },
      i * 1000,
      i
    ); //  注意这里
  }
}

// 第二种 立即执行函数
function log123() {
  for (var i = 0; i < 3; i++) {
    (function () {
      var j = i;
      window.setTimeout(() => {
        console.log(j + 1);
      }, i * 1000);
    })();
  }
}
```

如果初学者的话，可能看到这一大串代码有点手足无措，因为这里确实设计到了 JS 大量的核心特性，我们可以从头开始一点一点看下去

在第一个未改进的版本，我们首先在全局作用域中声明了函数`log123`函数，编译器预扫描了函数声明，并和宿主环境提供的 api 一起构建了全局作用域。

```
global:[log123, ...otherGlobalApi]
```

在构建完全局作用域后，代码就开始执行了，值得注意的是，js 不会预先编译所有的代码，函数会在执行时编译并缓存编译结果，以达到更快的执行速度。
在代码执行到`log123()`时，编译器会拿到函数的内容，开始编译并构建执行环境。
编译器会预先扫描函数体内所有的声明，其中包括形参。

```
script:[i]
global:[log123, ...otherGlobalApi]
```

整个过程中，函数作用域被押入栈中，在查询变量时，会按照入栈顺序从最近到最晚开始查询，理所当然的，全局作用域是最后被查询的作用域。

接下来函数继续执行

```js
function log123() {
  //  i被置为0
  for (var i = 0; i < 3; i++) {
    //  创建函数表达式 将函数表达式传入setTimeout函数
    window.setTimeout(() => {
      console.log(i + 1);
    }, i * 1000);
  }
}
```

在这里又创建了一个新的函数表达式，这个函数将会在定时器结束后被调用
在这里，词法作用域的特殊性就显现出来了，词法作用域在书中是这样定义的

> 词法作用域就是定义在词法阶段的作用域。换句话说，词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变

在这里，由于函数表达式是在`log123`函数内声明的，按照词法作用域的解析规则，函数表达式的内部作用域是这样的

```
script:[ ]
script:[i]
global:[log123, ...otherGlobalApi]
```

函数表达式内部并没有变量 i，按照作用域链查找规则，`console.log(i)`中使用的就是`log123()`中的 i
这里其实就是所说的闭包

> 当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行。

闭包几乎在 js 中随处可见，也并没有什么特殊，早期的 js 利用闭包来避免一些私有方法暴露，但在模块化的今天已经越来愈少。更多的时候，我们在写代码的时候无意就创见了闭包，比如一个事件监听函数，使用 React Hooks。

到了此时，我们终于可以总结一下输出和预期不一致的原因了。

1. 我们创建了一个函数作用域
2. 在函数作用域中声明了`i`
3. 我们创建了一个函数表达式，用于为定时器回调，函数表达式继承了父级函数作用域
4. 循环结束，`i`被置为 4
5. 定时器执行回调，执行`console.log(i)`时，在父级作用域中查找到了`i`，i 的值为 4

在 ES6 之前，解决办法就是上述两种，第一种通过函数传参数的方式，传递了`i`当时的字面量`1`,`2`,`3`，`i`指向的是函数表达式作用域的实参，输出是预期的结果。
第二种方式，是在每一次循环时，都为函数表达式创建了新的父级函数作用域。
作用域链会像这样

```
script:[ ]
script:[j]
script:[i]
global:[log123, ...otherGlobalApi]
```

函数表达式访问的是临时的父级作用域`j`的值，也如预期结果

#### 块级作用域

所幸我们很快在 ES6 中拥有了块级作用域，用 let 实现上面的效果很简单，只需要把 var 更换成 let

```js
function log123() {
  for (let i = 0; i < 3; i++) {
    window.setTimeout(() => {
      console.log(i + 1); // 1 2 3
    }, i * 1000);
  }
}
```

如果只把 let 理解成仅创建块级作用域的话，实际上无法解释输出的结果，按照作用域链查找规则，log 输出的还是父级块级作用域的 i

实际上，`let`在循环语句块中，有特别的处理，let 会在循环语句中的每一次循环创建单独的块级作用域，上面的代码类似于这样

```js
function log123() {
  for (let _i = 0; i < 3; i++) {
    let i = _i; // 创建单独的作用域
    window.setTimeout(() => {
      console.log(i + 1); // 1 2 3
    }, i * 1000);
  }
}
```

实际上，这里的处理方式基本类似于上面第二种办法。

### 结语

以上大概就是读完《你不知道的 javascript》 作用域和闭包 部分的一些想法和笔记，通过解答 js 的经典问题 变量提升来做一些回顾，如果需要再深入的话，可能还是需要深入 ECMAScript 标准，但是作为开发的话，目前的知识已经足够了。
