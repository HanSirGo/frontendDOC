## **vue3 中的 hooks 是什么?**

简单来说如果你的函数中用到了诸如 ref,reactive,onMounted 等 vue 提供的 api 的话,那么它就是一个 hooks 函数,如果没用到它就是一个普通工具函数。至于它为什么叫 hooks,我的理解则是

> 它可以通过特定的函数将逻辑 "钩入" 组件中，使得开发者能够更灵活地构建和管理组件的功能从而提高代码的可读性以及可维护性等

## **在 vue3 中使用**

上面说到 hooks 函数里包含了 vue 提供的 api,下面我们就简单的来举个例子看一下 vue3 中的 hooks 函数。一般来说,如果一个你得函数为 hooks 函数,那么你需要将其以 use 开头命名。在 src 下新建一个 hooks 目录专门存放 hooks 函数,然后写下第一个非常简单的 hooks 函数 useAdd

```js
import { ref } from "vue";
export const useAdd = () => {
  const a = ref(1);
  setInterval(() => {
    a.value++;
  }, 1000);
  return a;
};
```

这是一个非常简单的 hooks 函数,每隔一秒就让 a.value 加 1,最后返回一个响应式的 a,我们在组件中引用一下

```js
<template>
  <div>{{ a }}</div>
</template>

<script lang='ts' setup>
import { useAdd } from "../hooks/useAdd"
const a = useAdd()
</script>
```

此时我们会看到每隔一秒页面上的值就会加 1,所以说 a 还是保持了它的响应式特性

## **mixin 与 hooks**

我们都知道 Vue 3 引入 `Composition API`的写法,当我们引入一个 hooks 函数的时候其实就像在 vue2 中使用一个 mixin 一样,hooks 函数中的`ref`,`reactive`就相当于 mixin 中的`data`,同时 hooks 还可以引入一些生命周期函数,watch 等在 mixin 中都有体现。下面展示一下 mixin 的写法,这里不过多的讲解 mixin

```js
export const mixins = {
  data() {
    return {
      msg: "",
    };
  },
  computed: {},
  created() {
    console.log("我是mixin中的created生命周期函数");
  },
  mounted() {
    console.log("我是mixin中的mounted生命周期函数");
  },
  methods: {
    clickMe() {
      console.log("我是mixin中的点击事件");
    },
  },
};
```

组件中引入

```js
export default {
  name: "App",
  mixins: [mixins],
  components: {},
  created() {
    console.log("组件调用minxi数据", this.msg);
  },
  mounted() {
    console.log("我是组件的mounted生命周期函数");
  },
};
```

用过 vue2 的 mixin 的都知道它虽然可以封装一些逻辑,但是它同时也带来了一些问题.比如你引入多个 mixin 它们的 data,methods 命名可能会冲突,当 mixin 多了可能会出现维护性问题,另外 mixin 不是一个函数,因此不能传递参数来改变它的逻辑,具有一定的局限性等,但这些问题到了 vue3 的 hooks 中则迎刃而解

## **hooks 中生命周期执行顺序**

hooks 中生命周期与组件中的生命周期执行顺序其实很好判断,就看它们谁的同级生命周期函数先创建那就先执行谁的,比如在 useAdd 中加几个生命周期

```js
import { ref, onMounted, onBeforeUnmount, onUnmounted } from "vue";
export const useAdd = () => {
  const a = ref(1);
  setInterval(() => {
    a.value++;
  }, 1000);
  onMounted(() => {
    console.log("hooks---onMounted");
  });
  onBeforeUnmount(() => {
    console.log("hooks---onMounted");
  });
  onUnmounted(() => {
    console.log("hooks---onUnmounted");
  });

  return a;
};
```

然后在组件中也引入这些生命周期

```js
<template>
  <div>{{ a }}</div>
</template>

<script lang='ts' setup>
import { useAdd } from "../hooks/useTest"
import { onMounted, onBeforeUnmount, onUnmounted } from "vue";

onMounted(() => {
  console.log("组件---onMounted");
});
onBeforeUnmount(() => {
  console.log("组件---onMounted");
});
onUnmounted(() => {
  console.log("组件---onUnmounted");
});
const a = useAdd()
</script>

```

如果将 hooks 放到最后那么它们的顺序是这样的

![1712480189889](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480189889.png)

如果放到前面那就是这样的

![1712480202607](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480202607.png)

