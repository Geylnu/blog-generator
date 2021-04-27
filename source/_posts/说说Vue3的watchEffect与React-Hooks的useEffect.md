---
title: 说说Vue3的watchEffect与React Hooks的useEffect
date: 2020-10-10 11:19:59
tags:
  - Vue3
  - React
  - Hooks
  - 副作用
categories: Vue
---

最近有空阅读了一下 Vue3 的文档，发现 Vue 也新增了类似 React Hooks 中的 useEffect 的 watchEffect，两者基本很相似，这里就来比较比较。

### `useEffect`

React Hooks 带有一种函数式的设计理念，期望 `UI = f(x)`，UI 是纯函数的渲染结果，useEffect 则用来处理副作用，也就是对外部环境的影响，在 Hooks 中基本取代了生命周期的概念,类似于原本的`componentDidMount`和`componentWillUnmount`。

常见的在 useEffect 中请求数据

```jsx
const example = (props) => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    let hasCancle = false;
    query("/xxxx").then((data) => {
      // 避免副作用被取消后仍然调用
      if (!hasCancle) {
        const { count } = data;
        setCount(data);
      }
    });
    return () => {
      // 清除副作用
      hasCancle = true;
    };
  }, []);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
};
```

### `watchEffect`

watchEffect 作用基本与 useEffect 作用一致，上面的代码可以很方便的改写为 Vue 版本

```js
setup(props){
    const count = ref(0);
    watchEffect( onInvalidate => {
      let hasCancle = false;
      query("/xxxx").then((data) => {
        if (!hasCancle) {
          const { count } = data;
          setCount(data);
        }
      });

      onInvalidate(()=>{
          hasCancle = false
      })
    })
}
```

尽管功能上 watchEffect 与 useEffect 十分相似，但由于实现不一，实质还是有很多不同

#### 调用时机

在 React 中，`useEffect`被用来处理与界面无关的副作用，会在 React 更新 DOM 后调用，这样可以尽快的展现渲染结果，如果涉及到对 DOM 的副作用操作，则可以使用`useLayoutEffect`在重渲染期间调用，以避免浏览器重渲染。

而在 Vue 中，watchEffect 的调用时机则由`watchEffect`函数的第二个参数控制。

```js
watchEffect(
  (onInvalidate) => {
    // do xxx
  },
  {
    flush: "pre", // 'pre' | 'post' | 'sync'
  }
);
```

watchEffect 的默认调用时机`pre`，按照官方文档，它有两次调用时机

- 在初始化时被同步调用
- 在组件被更新前

watchEffect 类似于`beforeCreate`与`beforeUpdate`，但是实际还是有不小区别

```js
{
  beforeCreate() {
    console.log("beforeCreate");
  },
  created() {
    console.log("created");
  },
  setup() {
    const count = ref(0);

    watchEffect(
      () => {
        console.log("watchEffect sync");
        console.log(count.value);
      },
      {
        flush: "sync",
      }
    );

    watchEffect(() => {
      console.log("watchEffect pre");
      console.log(count.value);
    });

    watchEffect(
      () => {
        console.log("watchEffect post");
        console.log(count.value);
      },
      {
        flush: "post",
      }
    );

    onBeforeMount(() => console.log("beforeMount"));
    onMounted(() => console.log("onMounted"));
    onBeforeUpdate(() => {
      console.log("onBeforeUpdate");
    });

    onUpdated(() => {
      console.log("onUpdated");
      console.log(count.value);
    });

    console.log("sync setup");

    return {
      count,
    };
  },
};
```

以上代码调用顺序会是这样的结果：

```
watchEffect sync
watchEffect pre
sync setup
beforeCreate
created
beforeMount
watchEffect post
onMounted
```

我们可以看到 setup 函数里所有的内容都会被首先被调用，调用时机为 `pre` 和 `sync` 的 watchEffect 函数也会被同步调用，调用顺序取决于声明顺序，而调用时机为`post`的 watchEffect 函数 则会在 DOM 元素挂载后调用。

对于三种不同刷新方式，其实可以很简单的理解：

- `pre` 初始化会在 setup 函数中被同步调用，此后总在组件更新前调用
- `post` 初始化会在元素挂载后，此后总在组件更新后调用，总能获取实际的渲染结果
- `sync` 初始化会在 setup 函数中被同步调用，此后总在状态更改时同步调用

当然。在大多数情况下，我们其实不需要关心时机，仅在涉及到对 DOM 的副作用时做下区分，比如获取 ref

``` Vue
<template>
  <button ref="buttonRef" @click="count++">count is: {{ count }}</button>
</template>

<script>
import { watchEffect, ref } from "vue";

export default {
  setup() {
    const count = ref(0);
    const buttonEL = ref(null);
    watchEffect(() => {
      console.log(buttonRef.value?.innerText); // 初始化: undefined 第一次调用: count is 0
    });

    watchEffect(
      () => {
        console.log(buttonRef.value?.innerText); // 初始化: <button> 第一次调用: count is 1
      },
      {
        flush: "post",
      }
    );

    return {
      count,
    };
  },
};
</script>
```

使用`flush: "post"`,可以保证总是获取到最新的 DOM。

总的来说，Vue 和 React 在处理副作用上走上了不同的道路，React 倾向于尽快的进行重渲染，在渲染结束后执行副作用以避免 UI 线程等待；而 Vue 默认会在渲染更新前调用，更新的结果会直接体现在这次的渲染结果中。

#### 递归调用

在 React 中，`useEffect`经常出现的错误就是进行了递归的重渲染

```js
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1); // 这会导致递归调用,setCount触发了重渲染，副作用重新执行
}, [count]);
```

在 Vue 中，如果这个值仅被单个副作用函数依赖则并不会导致递归调用

```js
const count = ref(0);
watchEffect(() => {
  count.value++;
});
```

值会正确的更新，同时这次的副作用不会被更新。

然而如果被多个副作用函数依赖，或者被异步调用，仍然会导致递归调用

多个依赖

```js
const count = ref(0);
watchEffect(() => {
  count.value++;
});

watchEffect(() => {
  count.value++;
});
```

异步调用

```js
watchEffect(() => {
  nextTick(() => {
    count.value++;
  });
});
```

总而言之，在 watchEffect 应该尽量小心的更新值。

#### 取消副作用

在 React 中，取消副作用的函数通过函数返回值返回

```js
useEffect(() => {
  return () => {
    // do xxxxx
  };
});
```

然而在异步函数中这会存在问题

```js
useEffect(async () => {
  await fetchData(props.id);
  return () => {
    // do xxxxx
  };
});
```

异步函数返回的是一个 promise,我们无法在异步调用完成前拿到取消函数。

因此，为了更好地与 asnyc 函数调用兼容，在Vue中，取消函数以回调的方式注册

```js
const data = ref(null)
watchEffect(async onInvalidate => {
  onInvalidate(() => {...}) // 我们在Promise解析之前注册清除函数
  data.value = await fetchData(props.id)
})
```

### 缺陷

Vue 由于响应式原理，可以自动的进行依赖收集，React 则需要手动填写依赖

```js
const [count, setCount] = useState(0);
useEffect(() => {
  console.log(count);
}, [count]); //手动声明依赖
```

```js
const count = ref(0);
watchEffect(() => {
  console.log(count.value); // 在每次更改count时都会调用。
});
```

Vue 会在第一次运行副作用函数执行时进行自动的依赖收集，类似于目前的`computed`属性。
然而，依赖收集只发生在同步调用时，Vue 无法知道异步调用时使用的依赖，

比如下面这个例子，来自于 github 上的一个[issues](https://github.com/vuejs/vue-next/issues/2223 "watchEffect doesn't work when perform async ")

```js
import { reactive, watchEffect } from "vue";
function watchEffectTest(reactive, watchEffect) {
  const obj = reactive({ age: 1 });

  watchEffect(async () => {
    await new Promise((resolve) => {
      setTimeout(resolve, 100);
    });
    console.log(obj.age);
  });

  const timmer = setInterval(() => {
    obj.age++;
    if (obj.age > 5) {
      clearInterval(timmer);
      console.log("stop", obj.age);
    }
  }, 1000);
}
export default {
  setup() {
    watchEffectTest(reactive, watchEffect);
    return {};
  },
};
```

预期输出：

```
1
2
3
4
5
stop 6
```

实际输出

```
1
stop 6
```

当然，这种缺陷也可以通过手动的读取一次依赖，告诉Vue这是依赖项，仅仅是在编写时会并不优雅。

