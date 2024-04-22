## watch 和 watchEffect 的区别

> **watch需要手动指定监听的数据，仅在数据变化时触发；watchEffect自动监听无需指定，初始化时默认执行一次绑定函数。**

> ### `watch`
>
> watch 是一个选项API，在组件的选项中使用，可监听指定的数据变化，并执行回调函数。
>
> - • `watch` 是一个侦听器函数，可以监视特定的数据变化，并在数据变化时执行回调函数。
> - • 你可以通过传入要监视的数据以及回调函数来创建一个 `watch`。
> - • 你可以选择性地传入选项对象来定制 `watch` 的行为，比如设置监听深度、立即执行等。
> - • 适合处理需要根据新值和旧值执行不同逻辑的情况，比如计算属性或者复杂的数据侦听。
>
> ### `watchEffect`
>
> watchEffect是一个函数API，在组件的setup函数或生命周期函数中使用。它会自动追踪依赖的响应式数据，并在数据变化时执行回调函数。
>
> - • `watchEffect` 是一个立即执行的侦听器函数，会自动追踪其内部所使用的响应式数据，并在这些数据变化时重新运行。
> - • 不需要显式地传入要监视的数据，`watchEffect` 会自动捕获其内部使用的响应式数据，并在这些数据变化时触发重新运行。
> - • 适合处理那些不需要关心具体数据变化情况，而只需在数据变化时执行某些逻辑的情况。
>
> `watch` 更适合处理需要对数据变化做精细控制的情况，而 `watchEffect` 则更适合处理简单的数据变化触发逻辑。

```js
# 示例1：使用watch监视特定数据的变化

import { watch, reactive } from 'vue';

const state = reactive({
  count: 0,
  doubleCount: 0
});

watch(() => state.count, (newCount, oldCount) => {
  state.doubleCount = newCount * 2;
});

// 现在，每当state.count发生变化时，回调函数会被执行，更新state.doubleCount的值
在这个例子中，我们使用watch来监视state.count的变化。当state.count发生变化时，回调函数会被触发，然后更新state.doubleCount的值。
```

```js
# 示例2：使用watchEffect自动追踪内部依赖

import { watchEffect, reactive } from 'vue';

const state = reactive({
  count: 0
});

watchEffect(() => {
  console.log(`Count的值为: ${state.count}`);
});

// 现在，每当state.count发生变化时，watchEffect会自动重新运行，输出最新的count值
在这个例子中，我们使用watchEffect来自动追踪console.log语句内部所使用的响应式数据state.count，当state.count发生变化时，watchEffect会自动重新运行console.log语句，输出最新的count值。
```

> - • `watch`更适合处理需要对数据变化做精细控制的情况，
> - • `watchEffect`则更适合处理简单的数据变化触发逻辑，因为它会自动追踪内部依赖并触发相应的副作用。