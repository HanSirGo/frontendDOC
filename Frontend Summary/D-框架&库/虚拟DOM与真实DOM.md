# 虚拟DOM 一定比真实DOM快吗

虚拟DOM（VNode） 和 真实DOM 的渲染效率问题一直是面试中**高频、高难度**话题。

昨天有个同学在面试中就被问到了这个问题。面试官提问说：“虚拟DOM（VNode）渲染效率一定比 真实DOM 快吗？你是如何得到的这个结论？你的依赖数据是什么？”

好吧... 大厂的面试就是不一样，光靠背八股文肯定是混不过去了。那么今天，就让咱们带着这三个问题，一起来看看 **虚拟DOM（VNode） 和 真实DOM 的真实渲染效率，以及测试、依赖数据！**

## **01：测试逻辑**

在去讲解测试逻辑之前，我们先来明确一下什么叫做 虚拟DOM 和 真实DOM。

### **虚拟DOM**

所谓虚拟DOM指的是：**通过 JS 对象来模拟 DOM 结构**。

所以我们可以**简单的**把 虚拟DOM 理解为 JS 对象。只不过这个 JS 对象比较复杂一些而已。

### **真实DOM**

所谓真实DOM就是：**通过 JS 获取到的 html 标签元素**。

比如，我们通过 `document.querySelect('#app')` 拿到的这个元素就被成为 DOM 元素，也就是 真实DOM。

### **测试逻辑**

那么明确好了两个基本概念之后，接下来咱们就说下测试方案。整个测试方案分成 4 个部分：

1. **DOM 操作 和 JS 操作** 对比：来看下 DOM 操作 和 JS 操作的性能耗时差异
2. **DOM 操作 和 VNode 操作** 对比：来看下 涉及到 VNode 操作时，性能差异
3. **DOM 操作 和 复杂的VNode 操作** 对比：来看下 当 VNode 变得复杂时，性能消耗增加了多少
4. **复杂DOM 操作 与 复杂VNode 操作** 对比：来看下当 DOM 操作都变得复杂时，性能消耗差异

明确好了以上内容之后，就让咱们开始吧~~

## **02：DOM 操作与数据操作对比**

```vue
<body>
  <div id="app"></div>

  <script>
    const COUNT = 10000

    console.time('dom 耗时')
    for (let i = 0; i < COUNT; i++) {
      const ele = document.createElement('div')
      ele.innerHTML = 'hello Sunday'
      document.querySelector('#app').appendChild(ele)
    }
    console.timeEnd('dom 耗时')


    console.time('js 耗时')
    const vNodes = []
    for (let i = 0; i < COUNT; i++) {
      const vNode = {
        type: 'div'
      }
      vNodes.push(vNode)
    }
    console.timeEnd('js 耗时')
  </script>
</body>
```

在以上代码中，我们首先标记了一个数量值 `COUNT` 它表示循环执行的次数。然后我们利用 `console.time` 和  `console.timeEnd` 来记录执行时间，这里打印出来的时间就是我们的耗时。

此时，第一次的打印为 **真实DOM 的耗时**，第二次的打印为 **JS 数组操作的耗时**

最终打印结果如下：

![1712993362746](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712993362746.png)

通过耗时对比我们可以很清楚的看到：**在简单的 DOM 操作与 简单的数据操作** 对比下，DOM 操作的耗时大约是数据操作的 **23** 倍左右。

## **03：DOM 操作与 VNode 操作对比**

```vue
<body>
  <div id="app"></div>

  <script>
    const COUNT = 10000

    console.time('dom 耗时')
    for (let i = 0; i < COUNT; i++) {
      const ele = document.createElement('div')
      ele.innerHTML = 'hello Sunday'
      document.querySelector('#app').appendChild(ele)
    }
    console.timeEnd('dom 耗时')


    const { h } = Vue
    console.time('js 耗时')
    const vNodes = []
    for (let i = 0; i < COUNT; i++) {
      const vNode = h('div', 'hello Sunday')
      vNodes.push(vNode)
    }
    console.timeEnd('js 耗时')
  </script>
</body>
```

在这个代码中，我们使用了 Vue 中的 h 函数来构建了 Vnode，这样构建出的 VNode 对象结构上会更加复杂。从而可以验证出 **当对象的结构变得更加复杂时，对 JS 耗时的影响会有多大**

执行以上代码，得出的结果如下：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712993385902.png)

根据执行结果我们可以看出：当使用到了 h 函数时，JS 耗时明显增加。增加耗时的原因有两个：

1. h 函数生成对象的耗时
2. vNode 变得更加复杂时，push 的耗时

那么，假如 **VNode 变得超级复杂的时候**，JS 的耗时会超过 真实DOM 操作吗？

我们来看下一次的实验

## **04：DOM 操作与 复杂VNode 操作对比**

```js
<body>
  <div id="app"></div>
  <div id="app2"></div>

  <script>
    const COUNT = 10000

    console.time('dom 耗时')
    for (let i = 0; i < COUNT; i++) {
      const ele = document.createElement('div')
      ele.innerHTML = 'hello Sunday'
      document.querySelector('#app').appendChild(ele)
    }
    console.timeEnd('dom 耗时')


    const { h, render } = Vue
    console.time('js 耗时')
    const vNodes = []
    for (let i = 0; i < COUNT; i++) {
      const vNode = h('div', { id: 'container' }, [
        h('h1', null, ['Hello, World!']),
        h('ul', null, [
          h('li', null, ['Item 1']),
          h('li', null, ['Item 2']),
          h('li', null, ['Item 3'])
        ])
      ]);
      // vNodes.push(vNode)
      render(document.querySelector('#app2'), vNode)
    }
    console.timeEnd('js 耗时')
  </script>
</body>
```

在这个代码中，我们使用 h 函数构建了一个 **嵌套** 的 VNode 结构，这个结构其实就比较复杂了。咱们来看看它的执行结果：

![1712993407721](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712993407721.png)

在这个执行结果中，我们明确的发现 **虚拟 的耗时已经是 真实 DOM 耗时的 40倍 差距了。**

看到这里可能有些同学会说：这是因为虚拟DOM 远比 真实 DOM 复杂的原因导致的。所以接下来咱们就把两边的复杂性调整一直，然后看看最终的测试数据

## **05：复杂DOM 操作与 复杂VNode 操作对比**

```js
<body>
  <div id="app"></div>
  <div id="app2"></div>

  <script>
    const COUNT = 10000

    console.time('dom 耗时')
    for (let i = 0; i < COUNT; i++) {
      // 创建 div 元素
      const container = document.createElement('div');
      container.id = 'container';

      // 创建 h1 元素
      const h1 = document.createElement('h1');
      h1.textContent = 'Hello, World!';

      // 创建 ul 元素
      const ul = document.createElement('ul');

      // 创建三个 li 元素，并添加到 ul 元素中
      const li1 = document.createElement('li');
      li1.textContent = 'Item 1';
      ul.appendChild(li1);

      const li2 = document.createElement('li');
      li2.textContent = 'Item 2';
      ul.appendChild(li2);

      const li3 = document.createElement('li');
      li3.textContent = 'Item 3';
      ul.appendChild(li3);

      // 将 h1 和 ul 元素添加到 container 元素中
      container.appendChild(h1);
      container.appendChild(ul);

      // 将 container 元素添加到 #app2 元素中
      const app = document.querySelector('#app');
      app.appendChild(container);
    }
    console.timeEnd('dom 耗时')


    const { h, render } = Vue
    console.time('js 耗时')
    const vNodes = []
    for (let i = 0; i < COUNT; i++) {
      const vNode = h('div', { id: 'container' }, [
        h('h1', null, ['Hello, World!']),
        h('ul', null, [
          h('li', null, ['Item 1']),
          h('li', null, ['Item 2']),
          h('li', null, ['Item 3'])
        ])
      ]);
      // vNodes.push(vNode)
      render(document.querySelector('#app2'), vNode)
    }
    console.timeEnd('js 耗时')
  </script>
</body>
```

以上代码维持了两边同样的复杂程度，大家可以猜一猜最终的性能消耗会是什么情况呢？

查看最终的打印结果可以发现：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712993433871.png)

在双方都足够复杂的情况下，**虚拟DOM 的性能消耗是远超 真实DOM** 的。

但是，大家一定要注意：

> 以上测试结果只是在 **初始渲染** 的情况下的测试数据。

因为在实际开发中，DOM 的渲染是非常复杂的，涉及到：**渲染、更新、删除** 等各种复杂的操作，所以说我们 **不能够** 仅通过这一次的测试数据得出所有场景下的结论。

以上测试数据仅能够表示：**初始渲染**。

## **总结**

**框架一定是在基础 API 之上的封装。** 如果我们把基础 API 的性能消耗比作为 1，那么框架的性能消耗一定是 **1+N** 的。这也就意味着 **执行同样的操作时，框架的性能消耗一定 > 基础 API 的性能消耗**。

所以说，**虚拟DOM渲染所消耗的性能 > 真实DOM渲染所消耗的性能** 就不足为奇了。

那么为什么还要使用到虚拟 DOM 呢？其实是因为当 **DOM 更新时** ，虚拟DOM 可以帮助我们进行计算，从而减少 DOM 更新的频率，从而提升性能。

