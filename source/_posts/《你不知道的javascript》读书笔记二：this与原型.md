---
title: 《你不知道的javascript》读书笔记二：this与原型
date: 2020-09-14 20:36:28
tags:
  - 你不知道的Javascript
  - 读书笔记
  - this
  - 原型
categories: Javascript
---

这部分其实应该叫做吐槽合集，作者在这部分大量吐槽了把 js 中原型理解成类的做法。现在的前端社区中，如 React，Vue 也是更少利用类的一些特性，而是使用了混入，类似于该书中讲的行为委托模式；而在 React Hooks 和 Vue composition api 中，则更激进的几乎完全去掉了 Class 风格的代码。

读了这部分，就能了解社区为什么不太喜欢 Class 风格的代码。

## 从 this 讲起

### this

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
4. 硬绑定 bind Foo.speak.bind(Boo) this = Boo
5. new 绑定 new Foo.speak this = {}（es5）

当然，其实也可以用符合直觉的方式来理解，使用函数是手动传入 this，而在其它情况下，则是调用方，也就是指执行上下文，如果没有，则认为是 window。
可以看下一些经典的例子

```js
const foo = {
  name: "foo",
  logThis: function () {
    console.log(this);
  },
};

foo.logThis(); // { name: 'foo', logThis: Function  }

const anthorLogThis = foo.logThis;

anthorLogThis(); // Window

function returnLogFunction() {
  return function () {
    console.log(this);
  };
}

returnLogFunction()(); // Window

(false || foo.logThis)(); // Window
```

这里可能最让人疑惑的可能是最后一个例子，他们看起来是具有执行上下文的

```js
(false || foo.logThis)(); // Window
```

这里要注意在使用了括号运算符后，返回的是函数的值，而返回的并不是执行上下文和函数的整体，也就自然变为了默认的 Window。

如果想具体如何判断 this，可以参考[这篇博客](https://github.com/mqyqingfeng/Blog/issues/7 "JavaScript深入之从ECMAScript规范解读this")
这篇博客从规范实现上面阐述如果判断 this，当然，绝大多数情况都可以以符合直觉的方式，判断它的调用方是谁就好了。

### this 与属性描述符

属性描述符是 ES5 时出现的，可以给对象设置数据描述符和存取描述符

```js
const o = {}; // 创建一个新对象

// 在对象中添加一个属性与数据描述符的示例
Object.defineProperty(o, "a", {
  value: 37,
  writable: true,
  enumerable: true,
  configurable: true,
});

o.a; // 37

const name = "";
Object.defineProperty(o, "name", {
  get: function () {
    console.log("get!");
    return name;
  },
  set: function (value) {
    console.log("set!");
    name = value;
  },
});
```

在对象属性访问中，也有类似的[[GET]]与[[PUT]]。
默认的内置[[GET]]操作会首先在对象中查找是否有名称相同的属性，如果找到就会返回这个属性的值，如果没有找到，则会遍历原型链上的属性，如果没有则会返回 undefined，
而默认的[[PUT]]方法则由于原型链的存在复杂很多

在对象自身存在属性时，[[PUT]]方法很简单，如果有定义的 setter 就调用 setter，如果没有，就检查属性描述中定义是否可写，如果可写就写入。

而如果对象属性不存在，则会涉及到复杂的原型上属性的处理了。

1. 如果原型上存在对应属性，并且是数据描述符，并且 writable 为 true，那么会直接在当前对象上添加对应的属性和值。
2. 如果原型上存在对应属性，并且是数据描述符，如果 writable 为 flase 那么这次赋值默认会失败，严格模式下则会报错。
3. 如果原型上存在对应属性，并且是存取描述符，则会调用对应的 setter

第二点类似于 java 中的 final 标识符，表示不可重写，然而可能大多数场景开发者会认为这是一个 BUG，因为结果出人意料。

## 类与原型

在很多基于类的语言中，类的继承行为都是复制的，对类的重新定义和运行时改写是不允许的，非常安全。

```java
class Person {
    private String name;
    private int age;

    public String getName() {...}
    public void setName(String name) {...}
    public int getAge() {...}
    public void setAge(int age) {...}
}

class Student extends Person {
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

而在 js 中，类似的概念是以原型为基础的，在使用 new 关键字时，产生的对象会自动指向函数的[[prototype]]属性

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.sayName = function () {
  console.log(this.name);
};

const person = new Person("st", 34);
person.sayName; // st
```

指的注意的是，由于 js 的动态性，原型也是可以更改的，并且所有属性是共享的，基于原型的方法会很脆弱

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.sayName = function () {
  console.log(this.name);
};

const person = new Person("st", 34);
person.sayName; // st

Person.prototype.sayName = function () {
  console.log(this.name + "new");
};
person.sayName; // stnew
```

由于在 es6 之前，js 没有提供相关类的写法，就有各种对继承的实现，使用最多的就是寄身组合式继承

```js
function Parent(name) {
  this.name = name;
  this.colors = ["red", "blue", "green"];
}

Parent.prototype.getName = function () {
  console.log(this.name);
};

function Child(name, age) {
  Parent.call(this, name);
  this.age = age;
}

// 关键的三步
var F = function () {};

F.prototype = Parent.prototype;

Child.prototype = new F();

var child1 = new Child("kevin", "18");

console.log(child1);
```

而在 ES6 中，继承只需要简单的 extnds 关键字

```js
class Parent {
  constructor(name) {
    this.name = name;
    this.colors = ["red", "blue", "green"];
  }

  getName() {
    console.log(this.name);
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name);
    this.age = age;
  }

  getName() {
    console.log(`${this.name} ${this.age}`);
  }
}

const child = new Child("kk", 18);
child.getName(); // kk 1
```

指的注意的是，super 是静态绑定的，super 在声明时就已经确定了。

```js
class P {
  foo() {
    console.log("P.foo");
  }
}

class C extends P {
  foo() {
    super.foo();
  }
}

const c1 = new C();
c1.foo(); // P.foo

const D = {
  foo() {
    console.log("D.foo");
  },
};

const E = {
  foo: C.prototype.foo,
};

Object.setPrototypeOf(E, D);
E.foo(); // P.foo
```

实际上，在 Vue 和 React 的代码中，继承的代码很少被使用，更多倾向于使用混入来复用代码，比如 Vue 中的 混入代码

```js
/**
 * Mix properties into target object.
 */
export function extend(to: Object, _from: ?Object): Object {
  for (const key in _from) {
    to[key] = _from[key];
  }
  return to;
}
```

混入可以更方便的组合多种对象，而不必实现复杂的继承关系。

## 社区

实际上社区也有很多基于类关系的框架和代码，比如 angular、nest,实际上它们工作、维护的相当好，并没有太过不堪，选择基于类、原型、函数式更多是一种编程风格的选择，都有或多或少的优点缺点。
