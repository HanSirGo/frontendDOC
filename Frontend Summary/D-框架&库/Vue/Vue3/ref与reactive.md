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

## Vue3Reactive的懒响应性

### **Reactive 的实现原理**

Reactive 是基于 `Proxy` 进行实现的，这个概念很多同学都是知道的。我们可以通过 vue 的源码来看下这个实现：

![1712988676440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712988676440.png)

通过上图红框中的代码，我们可以很清晰的看到在 Reactive 里面，Vue 最终通过 `new Proxy` 的方式生成了一个 Proxy 的实例对象。 这个 Proxy 的实例就是 `reactive()` 方法调用时的返回值。

我们可以通过如下伪代码表示：

```js
function reactive (target) {
  return new Proxy(target, {....})
}

const test = reactive({name: '张三'})
```

在这种情况下，`{name: '张三'}` 就会被包装成一个 **响应式对象**，`proxy` 通过监听它的 `getter、setter` 行为来触发响应性。

### **Proxy 的问题**

Proxy 可以代理对象，这个是没有问题的。**但是：Proxy 只能代理一层对象，而不能代理多层**。

什么意思呢？假如当 代理对象 具备层级嵌套的时候，proxy 是不可以代理子层级的。我们可以通过以下代码来进行测试：

```js
const target = {
    name: '张三',
    child: {
      name: '小张三'
    }
  }

  const p = new Proxy(target, {
    set(target, property, value, receiver) {
      console.log('触发了 set');
      target[property] = value
      return true
    },
    get(target, property, receiver) {
      console.log('触发了 get');
      return target[property]
    }
  })

  p.name = '李四'  // 打印：触发了 set
  p.child.name = '小李四' // 打印：触发了 get。注意：并没有触发 child 的 set
```

在上述代码中，我们利用 proxy 代理了 target 对象。**注意：此时的 target 是存在嵌套的复杂数据类型 child**。

当我们执行 `p.name = '李四'` 时，触发 `set` 行为，这是一个很正常的逻辑。但是，当我们执行 `p.child.name = '小李四'` 时，我们会发现 **仅触发了一个 get（p.child），而没有触发 set** 行为。

这就证明：对于 Proxy 而言， **它只能代理 一层 的复杂数据类型，而不可以代理多层**。

但是，在 Vue 中 **多层的 Reactive 对象却可以实现响应性，那么这是如何做到的呢？** 具体的方式就是 **Reactive的懒响应性**。

### **Reactive的懒响应性**

我们观察 Vue 的源码，在源码监听 proxy getter 行为的地方，存在这样一段代码（我把代码稍微做了一些调整，让大家看的可以更加清晰）：

![1712988765645](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712988765645.png)

核心的代码就在红框的位置，其中的 res 大家可以理解为 `target[property]` 。

而根据 **01：Reactive 的实现原理** 我们知道 `reactive` 方法本质上就是生成了一个 Proxy 实例。所以说，这里的代码核心其实就是 **一旦获取到的属性是对象，则会为该对象再次执行 reactive 方法**

如果用伪代码表示，就是下面这样：

![1712988786451](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712988786451.png)

即：**再次通过 Proxy 完成代理，并返回**

所以说，所谓的 Reactive的懒响应性 指的就是：**Reactive 最初只会为复杂数据类型执行第一层的响应性。如果存在多层的复杂数据类型嵌套时，则会在使用到（getter 行为）该子集时，才会再次为该子集包装 proxy（执行 reactive 方法）**

## 给一个reactive包裹变量重新赋值,上一个数据源失去响应性

>  如果给一个reactive包裹变量重新赋值,上一个数据源失去响应性。这是因为，vue.js的数据响应式是通过对象的属性变化来实现的。当给一个变量重新赋值时，实际上是新建了一个新的对象，而上一个对象被丢弃了，因此，vue.js无法检测到变化，也就无法触发更新。
>
> ```js
> const status = reactive({
>     arr:[],
>     form:{}
> })
> status.arr = [1,2]
> status.form = {...{a:1}}
> ```
>
> 

