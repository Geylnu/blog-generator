---
title: 从callback到async函数-js异步编程的演变
date: 2019-10-06 11:30:33
tags:
  - 异步
  - async
  - 函数
  - Promise
categories: Javascript
---

## 前言

其实之前只知道 Promise 很方便，大家也都是这么用的，generate 函数也一知半解，直到最近看了阮一峰的[ES6 入门](http://es6.ruanyifeng.com/#docs/generator-async)和在知乎看的一篇工业聚的[文章](https://zhuanlan.zhihu.com/p/83965949 "100 行代码实现 Promises/A+ 规范"),才了解整个的发展历程，我也推荐你去看看他们的文章。

## js 异步编程模型

js 作为一门浏览器的脚本语言，出生的时候就希望它尽可能的简单，易用，即使是非专业人员也能迅速掌握，而且 js 最初的应用场景并不复杂，所以单线程的 js 就确定了下来。
虽然 js 是单线程的，但浏览器却不是，对于很多耗时过长的任务，比如发起 ajax 请求，设置定时器，会由浏览器来做，js 只需要注册回调函数，声明浏览器完成异步后需要要做什么就可以了，js 只要有异步的地方，都有回调函数。

```js
let xhr = new XMLHttpRequest();

xhr.open("GET", "/cccc");
xhr.onreadystatechange = function () {
  if (xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
    console.log(xhr.responseText);
  }
};
xhr.send();
```

## 原始回调函数

最早的回调函数方式就是直接在参数中传递一个回调函数

```js
setTimeout(() => {
  console.log("1000 delay");
}, 1000);
```

执行 setTimeout 后，这个计时任务会交由其它线程处理以避免阻塞 js 主线程,例如在浏览器会交由一个单独的计时线程处理；在时间超时后，浏览器会向 js 任务队列中插入新的任务，而 js 主线程在在同步任务完成后会不断检测队列中是否有新任务，并执行，此时这个回调函数也就被执行了。

回调函数的一大缺点就是在大量异步任务时，会嵌套下去，代码会逐渐横向发展，也即回调地狱。

```js
function stepBystep() {
  setTimeout(() => {
    console.log("1000 delay");
    setTimeout(() => {
      console.log("2000 delay");
      setTimeout(() => {
        console.log("3000 delay");
        setTimeout(() => {
          console.log("4000 delay");
        }, 4000);
      }, 3000);
    }, 2000);
  }, 1000);
}
```

为了解决这个问题，Promise 就诞生了

## Promise

Promise 语法可以将异步回调写成链式调用的模样，看着更清晰更直观，错误捕获等也有更好的体验

```js
function setTimeoutPromise(time) {
  return new Promise((res) => {
    setTimeout(() => {
      return res(time);
    }, time);
  });
}

function stepBystepPromise() {
  setTimeoutPromise(1000)
    .then(() => {
      console.log("1000 delay");
      return setTimeoutPromise(2000);
    })
    .then(() => {
      console.log("2000 delay");
      return setTimeoutPromise(3000);
    })
    .then(() => {
      console.log("3000 delay");
      return setTimeoutPromise(4000);
    })
    .then(() => {
      console.log("4000 delay");
    });
}
```

这时候代码就是竖向展开的，错误捕获也很方便，`.catch()`就好了。
虽然 Promise 封装的回调函数已经很方便了，但是如果还想进一步呢？

## generate 函数

generate 函数提供了`yield`关键字，可以在函数中断执行，直到调用`.next()`方法

```js
function* generate() {
  let a = yield 4 + 1;
  console.log("log:" + a);
  let b = yield 5 + 1;
  console.log("log:" + b);
}

let gen = generate();
gen.next(); // {value: 5,done: flase}
gen.next(8); // log:8 {value: 6,done: flase}
gen.next(9); // log:9 {value: undefined,done: true}
```

用这个方法，我们就可以尝试者用 generate 函数执行异步操作,在此之前我们首先需要把带有 callback 函数写成 Thunk 函数或者 Promise 的风格

### 什么是 Thunk 函数

Thunk 函数就是将带有回调函数的多参数函数转变成只有一个回调函数的单参数函数，就像下面这样

```js
function setTimeOutThunk(time) {
  return (callback) => setTimeout(callback, time);
}
```

这样做的目的是为了方便使用 generate 函数来控制异步流程，下面就是一个例子

```js
function setTimeOutThunk(time) {
  return (callback) => setTimeout(callback, time, time);
}

function* generate() {
  let val = yield setTimeOutThunk(1000);
  console.log(val);
  val = yield setTimeOutThunk(2000);
  console.log(val);
  val = yield setTimeOutThunk(3000);
  console.log(val);
  val = yield setTimeOutThunk(4000);
  console.log(val);
}

let gen = generate();
let { value } = gen.next(); // {value: [[callbackFunction]] , done: flase}
value(() => gen.next(123)); //123 {value: [[callbackFunction]] , done: flase}
```

这里`setTimeOutThunk()`已经变成单参数版本的函数,我们在调用`.next()`后，返回只具有回调函数参数的函数，这里其实主要是为了方便传参，如果另有约定不使用 Thunk 函数转化也可以。

在回调函数中,我们让回调函数调用下一个`.next()`,就可以让这个异步函数像同步函数一样执行了完整版本见下。

```js
function setTimeOutThunk(time) {
  return (callback) => setTimeout(callback, time, time);
}

function* gen() {
  let val = yield setTimeOutThunk(1000);
  console.log(val);
  val = yield setTimeOutThunk(2000);
  console.log(val);
  val = yield setTimeOutThunk(3000);
  console.log(val);
  val = yield setTimeOutThunk(4000);
  console.log(val);
}

function run(gen, ...args) {
  let { value, done } = gen.next(...args);
  if (!done) {
    debugger;
    if (value instanceof Promise) {
      value.then((...args) => run(gen, ...args));
    } else {
      value((...args) => run(gen, ...args));
    }
  }
}

run(gen());
```

这里也支持 Promise
实质上这里已经完全将异步回调写成了同步代码，除了需要手动调用一下`run()`方法。

## async 函数

async 函数其实就是 generate 函数的语法糖版本，会自动运行，让我们不需要再额外调用`run()`方法，使用就很方便了

```js
function setTimeoutPromise(time) {
  return new Promise((res) => {
    setTimeout(() => {
      res(time);
    }, time);
  });
}

async function stepBystepAwait() {
  let time1 = await setTimeoutPromise(1000);
  console.log(`${time1} delay`);
  time1 = await setTimeoutPromise(2000);
  console.log(`${time1} delay`);
  time1 = await setTimeoutPromise(3000);
  console.log(`${time1} delay`);
  time1 = await setTimeoutPromise(4000);
  console.log(`${time1} delay`);
}
```

写法上也与 generate 相似。

## 一切皆回调

无论是 Promise，还是 async 函数，其实仍然是利用回调函数处理异步，只是对如何处理异步函数做了一层封装，做了一些语法糖，让我们更方便的使用而已。
