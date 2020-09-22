---
title: 《你不知道的javascript读书笔记》二：this与原型
tags:
  - 你不知道的Javascript
  - 读书笔记
  - this
  - 原型
categories: Javascript
---

这部分其实应该叫做吐槽合集，作者在这部分大量吐槽了把 js 中原型理解成类的做法，前端社区中，如 React，Vue 也是更少利用类的一些特性，而是使用了混入，类似于该部分讲的行为委托模式，在 React Hooks 和 Vue composition api 中，则更激进的几乎完全去掉了 Class 风格的代码。

读了这部分，就能了解社区为什么不太喜欢 Class 风格的代码。

## 从 this 讲起

this 也是 js 中新手很头疼的问题了，各种归纳总结 this 的方法随处可见。这里也贴下书中对 this 的定义

> 当一个函数被调用时，会创建一个活动记录（有时候也称为执行上下文）。这个记录会包含函数在哪里被调用（调用栈）、函数的调用方式、传入的参数等信息。
> this 就是这个记录的一个属性，会在函数执行的过程中用到。

借助 this 我们可以很方便的来传递上下文

```js
// 隐式绑定this
const Foo = {
  name: "Foo",
  speak() {
    console.log(this.name);
  },
};

Foo.speak(); // Foo

const Boo = {
  name: "Boo",
};

// 显式绑定this
Foo.speak.call(Boo); // Boo

// 传统传递上下文
function speak(ctx) {
  console.log(ctx.name);
}

speak(Foo); // Foo
speak(Boo); // Boo
```

书里归纳了五种 this 的绑定方式，就以上面的例子为例

1. 默认绑定 speak() this = window
2. 隐式绑定 Foo.speak() this = Foo
3. 显式绑定 Foo.speak.call(Boo) this = Boo
4. 硬绑定 bind Foo.speak.bind(Boo) = Boo
5. new 绑定 new Foo.speak
