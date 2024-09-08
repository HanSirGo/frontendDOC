

# v-for和v-if为什么不共存

# **v-for 和 v-if 可以共存吗**

在Vue.js中，v-for和v-if可以一起使用，但需要注意一些潜在的问题。在Vue 2和Vue 3中，它们的行为略有不同。

### **Vue 2：**

在Vue.js 2中，`v-for`和`v-if`可以在同一元素上共存，但需要注意一些细节。在Vue.js的文档中有关于这个问题的说明。

当`v-for`和`v-if`同时存在于同一个元素上时，Vue.js会先处理`v-for`，然后再处理`v-if`。这意味着`v-if`将在每次迭代中都进行条件判断。如果你希望只对特定的项应用`v-if`条件，可以将`v-if`放在包裹元素上。

下面是一个简单的示例，演示了`v-for`和`v-if`的共存：

```
<div id="app">
    <div v-for="item in [1, 2]" v-if="item !== 2"></div>
</div>
```

在这个例子中，只有在`item !== 2`为真时，才会渲染`<div>`元素。

需要注意的是，这种情况下会创建多个具有相同条件的元素。如果`v-if`条件在整个`v-for`循环中保持不变，这可能会导致渲染多余的元素。在这种情况下，你可能需要考虑在数据处理层面进行过滤，以减少不必要的迭代和渲染。

### **Vue 3：**

在Vue 3中，Vue引入了一种新的v-for语法，使得在同一元素上使用v-for和v-if更加直观和灵活，并且有更好的性能优化。。你可以使用新的v-for语法来同时处理v-if的条件。

```
<div v-for="item in items" :key="item.id" v-if="item.visible">
  {{ item.text }}
</div>
```

在上面的例子中，:key用于为v-for提供唯一的key值，以确保正确地重新渲染。这种写法更清晰，并且能够避免Vue 2中可能出现的一些问题。

# **Vue2和Vue3中v-for 和 v-if优先级是怎么样的**

在Vue.js 2中，`v-for`的优先级高于`v-if`。这意味着在同一元素上使用`v-for`和`v-if`时，`v-for`会在`v-if`之前执行。这样在每次迭代时都会执行`v-if`的条件判断。

在Vue.js 3中，情况有所改进。Vue.js 3引入了一种称为"block tree"的概念，它允许`v-if`直接影响`v-for`，从而更灵活地进行优化。当`v-if`位于`v-for`之上时，Vue.js 3会尽力合并它们，以减少不必要的渲染和迭代。

总的来说，在Vue.js 3中，`v-if`和`v-for`的优先级关系更灵活，可以更好地优化性能。

# **vue2**

```
<div id="app">
    <div v-for="item in [1, 2]" v-if="item !== 2"></div>
</div>
```

剖析原理必然先把Vu2解析成：

https://v2.template-explorer.vuejs.org/

这是一个解析`Vue2`代码的网站，能把`Vue`代码解析成最终的产物代码

![1725786449129](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786449129.png)

其实重要的部分在这里![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786467113.png)

可以看到在`Vue2`中，**会先循环，然后在循环中去判断**，判断为真则正常渲染，判断为假则执行`_e`函数，`_e`函数就是注释的意思，就是把判断为假的节点注释掉，也就是：

1. 先走`v-for`循环 3 次
2. 在循环体中走`v-if`判断
3. 判断`item !== 2`则正常渲染，`item === 2`则把这个节点注释掉

所以最终选出来 1、3 两个节点大家能理解为什么`Vue2`会警告你了吗？

**因为其实我们只需要渲染2个节点，但是最终还是循环了3次，造成了性能浪费，也就是 v-for 优先级高于 v-if，共存时会造成性能浪费**

# **vue3**

但是在`Vue3`中，`v-for`和`v-if`却是可以共存的，为什么呢？我们还是拿最简单的代码来分析

```
<template>
    <div v-for="item in [1, 2, 3]" v-if="item 1==2"></div>
</template>
```

我们可以到这个网站：

https://play.vuejs.org/

看到解析后的产物

![1725786496125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786496125.png)

重要部分在这里

![1725786514894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786514894.png)

可以看到，跟`Vue2`不同的是，`Vue3`中是先走判断，然后再走循环的，也就是：

1. 先走`v-if`判断
2. 如果`item !== 2`，就去走循环也就是`v-for`
3. 如果`item == 2`，就执行`createCommandVNode`，创建一个注释节点

**也就是在Vue3中，v-if优先级是高于v-for的，真正循环的只有1、3这两个节点，这提高了性能**

但是我们会看到，代码会报错：**item 找不到** ？

![1725786529100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786529100.png)

这是因为在`v-for`和`v-if`共存的时候，`Vue3`会在底层帮我们转换成

```
<template>
    <template v-if="item !==2">
        <div v-for="item in [1, 2, 3]"></div>
    </template>
</template>
```

不信我们可以看看转换后的产物，跟刚刚是一模一样的！！！ !

![1725786554988](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725786554988.png)

`item`是在外层的，所以报错说`item`找不到，大家都知道为啥了吧？在外层怎么能获取到`item`呢？

### **总结**

总结就是不要让`v-if`和`v-for`共存在同一个标签中，遇到这种情况需要使用`computed`去计算，然后再去渲染

```
<div v-for="item in list" :key="item"></div>

<script setup lang="ts">
import { computed } from 'vue'
const list = computed(() => [1, 2, 3].filter(item => item !== 2))
</script>
```

