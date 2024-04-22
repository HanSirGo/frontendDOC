## ref和 reactive 区别

> 在Vue 3中，`ref` 和 `reactive` 都是用来创建响应式数据的工具，但它们有一些区别。

> #### `ref`：
>
> - • `ref` 是用来创建一个包裹基本数据类型（如数字、字符串等）的响应式对象。
> - • 当你需要包装一个基本类型的值，并且希望该值是响应式的，你可以使用 `ref`。
> - • 通过访问 `.value` 属性来获取包装的值，或者通过修改 `.value` 属性来更新该值。

```js
 import { ref } from 'vue';

    const count = ref(0);
    console.log(count.value); // 输出: 0

    count.value++; // 更新值
```

> #### `reactive`：
>
> - • `reactive` 用于创建一个包裹对象的响应式代理。
> - • 当你需要包装一个对象，并且希望对象内部的属性是响应式的，你可以使用 `reactive`。
> - • 通过直接访问和修改对象的属性来实现对对象的响应式跟踪。

```js
  import { reactive } from 'vue';

    const state = reactive({
    count: 0
    });

    console.log(state.count); // 输出: 0

    state.count++; // 更新对象属性值
```

> `ref` 和 `reactive` 的实现原理涉及到 Vue 3 的响应式系统和 ES6 的 Proxy 对象。
>
> #### `ref` 实现原理：
>
> - • 在内部，`ref` 实际上是使用了一个名为 `Ref` 的类来封装基本类型的数据。
> - • 当你使用 `ref` 创建一个响应式的变量时，Vue 3 实际上会创建一个 `Ref` 类的实例，该实例包装了你提供的基本类型的值。
> - • `Ref` 类内部利用了一个名为 `value` 的属性来存储和追踪值的变化。同时，它还利用了 Vue 3 的响应式系统来确保当 `value` 发生变化时，相关的视图能够及时更新。
>
> #### `reactive` 实现原理：
>
> - • `reactive` 的实现原理涉及到 ES6 的 Proxy 对象，它允许你创建一个代理对象，拦截对目标对象属性的访问和修改。
> - • 当你使用 `reactive` 包装一个对象时，Vue 3 实际上会使用 Proxy 来创建这个对象的一个代理，从而实现对对象内部属性的访问和修改进行拦截。
> - • 当代理对象的属性被访问或修改时，Vue 3 的响应式系统会负责通知相关的视图进行更新，以确保视图与数据的同步。

> ref 和 reactive 是 Vue3 数据响应式中非常重要的两个概念。
>
> - • reactive 用于处理对象类型的数据响应式。底层采用的是 new Proxy()
> - • ref 通常用于处理单值的响应式，ref 主要解决原始值的响应式问题。底层采用的是Object.defineProperty()实现的

