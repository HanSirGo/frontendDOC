# 可视化拖拽库

提到拖拽功能的实现，大家首先想到的几乎都是大名鼎鼎的`Sortablejs`。

和大家一样，`Sortablejs`也是我开发时的首选， 由于项目使用的是 Vue3，选择了`Sortablejs`官方封装的 **vue.draggable.next**

![1714282186996](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282186996.png)

但是在使用过程中发现很多问题，开发过程并不顺利，其中一个问题和这个网友遇到的一样，使用socket.io更新列表时，总是报这个错误：

![1714282198217](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282198217.png)

找解决方案过程中，发现这个库最近一次更新已经是3年前了：

![1714282212161](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282212161.png)

看到issue回复中，都在推荐`VueDraggablePlus`这个库， 纠结再三还是放弃了`Sortablejs`官方封装支持vue的库。

![1714282234167](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282234167.png)

## **关于 VueDraggablePlus**

`VueDraggablePlus` 是一个专为 Vue 打造的拖拽排序模块，基于 Sortablejs 封装，支持 Vue3 或 Vue 2.7+。之前，Vue 作者尤雨溪还在社交媒体上推荐了这款组件。

### **解决痛点**

在 `Sortablejs` 官方以往的 `Vue` 组件中，都是通过使用组件作为列表的直接子元素来实现拖拽列表，当我们使用一些组件库时，如果组件库中没有提供列表根元素的插槽，我们很难实现拖拽列表，`vue-draggable-plus` 完美解决了这个问题，它可以让你在任何元素上使用拖拽列表，我们可以使用指定元素的选择器，来获取到列表根元素，然后将列表根元素作为 `Sortablejs` 的 `container`，

### **技术特性**

- **功能强大**：全面继承 `Sortable.js` 拖拽排序库的所有功能；
- **Vue 生态支持好**：兼容 Vue3 和 Vue2；
- **实用灵活**：支持组件、指令、函数式调用，我们喜欢那种编程方式都没问题；
- **TS 支持**：这个库本身就是用 `TypeScript`编写，有完整的 TS 文档；
- **数据绑定**：支持 `v-model` 双向绑定，不需要单独维护排序数据；
- **支持自定义容器**：可以自定某个容器作为拖拽容器，比 `Sortable.js` 更灵活。

上面提到了，`vue-draggable-plus`提供三种方式：组件使用方式、hooks使用方式和指令使用方式。下面都给大家介绍一下具体如何使用。

## **使用**

### **安装**

首先安装依赖

```js
npm install vue-draggable-plus
// 或者
yarn add vue-draggable-plus
```

首先导入`vue-draggable-plus`组件：

```js
import { VueDraggable } from 'vue-draggable-plus'
```

### **组件使用方式**

```vue
<template>
    <div class="flex w-full">
      <VueDraggable
        ref="el"
        v-model="list"
        :animation="150"
        ghostClass="ghost"
        class="flex w-full flex-col gap-2 p-4 h-300px"
      >
        <div
          v-for="item in list"
          :key="item.id"
          class="cursor-move flex justify-between h-30"
        >
        <span>{{ item.id }}</span>
        <span>{{ item.name }}</span>
        </div>
      </VueDraggable>
    </div>
  </template>
  
  <script setup lang="ts">
  import { ref } from 'vue'
  import { type UseDraggableReturn, VueDraggable } from 'vue-draggable-plus'
  const list = ref([
    {
      name: '风险编号',
      id: "riskNumber"
    },
    {
      name: '风险点等级',
      id:  "rishLevel"
    },
    {
      name: '控制目标',
      id: "control target"
    },
    {
      name: '影响程度',
      id:  "degree of influnce"
    }
  ])
  const el = ref<UseDraggableReturn>()
  </script>
  
  <style scoped>
  .ghost {
    opacity: 0.5;
    background: #c8ebfb;
  }
  </style>
```

![1714282274144](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282274144.png)

### **函数使用**

多列表之间拖拽排序,可以通过设置相同的`group`属性来实现：

```vue
<template>
  <div class="flex gap-2 p-4 w-300px h-300px m-auto bg-gray-500/5 rounded">
    <section ref="el1" class="w-[50%]">
      <div
        v-for="item in list1"
        :key="item.id"
        class="flex justify-between h-30 bg-gray-500/5 rounded p-3 cursor-move"
      >
        <span>{{ item.name }}</span>
      </div>
    </section>
    <section ref="el2" class="w-[50%]">
      <div
        v-for="item in list2"
        :key="item.id"
        class="flex justify-between h-30 bg-gray-500/5 rounded p-3 cursor-move"
      >
        <span>{{ item.name }}</span>
      </div>
    </section>
  </div>
</template>
  
<script setup lang="ts">
import { ref } from "vue";
import { type UseDraggableReturn, VueDraggable } from "vue-draggable-plus";
import { useDraggable } from "vue-draggable-plus";

const list1 = ref([
  {
    name: "风险编号",
    id: "riskNumber",
  },
  {
    name: "风险点等级",
    id: "rishLevel",
  },
  {
    name: "控制目标",
    id: "control target",
  },
  {
    name: "影响程度",
    id: "degree of influnce",
  },
]);
const list2 = ref([
  {
    name: "曝光量",
    id: "open",
  },
  {
    name: "展示数",
    id: "show",
  },
  {
    name: "点击数",
    id: "click",
  },
  {
    name: "转化数",
    id: "buy",
  },
]);
const el1 = ref();
const el2 = ref();

useDraggable(el1, list1, {
  animation: 150,
  ghostClass: "ghost",
  group: "number",
});
useDraggable(el2, list2, {
  animation: 150,
  ghostClass: "ghost",
  group: "number",
});
</script>
  
<style scoped>
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
</style>
```

![1714282303216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282303216.png)

### **指令使用方式**

我们可以将表格的 thead 指定为目标容器，实现表格列拖拽

```vue
<template>
  <table class="table table-striped">
    <thead class="thead-dark">
      <tr v-draggable="headers">
        <th class="cursor-move" v-for="header in headers" :key="header.value">
          {{ header.text }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in list" :key="item.name">
        <td v-for="header in headers" :key="header">
          {{ item[header.value] }}
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup lang="ts">
import { ref } from "vue";
import { vDraggable } from "vue-draggable-plus";
const headers = ref([
  {
    text: "序号",
    value: "id",
  },
  {
    text: "日期",
    value: "date",
  },
  {
    text: "展示量",
    value: "show",
  },
]);
const list = ref([
  {
    date: "2023-10-10",
    show: 17800,
    id: 1,
  },
  {
    date: "2023-10-11",
    show: 56231,
    id: 2,
  },
  {
    date: "2023-10-12",
    show: 763230,
    id: 3,
  },
  {
    date: "2023-10-13",
    show: 21232,
    id: 4,
  },
]);
</script>
<style scoped>
tr {
  height: 48px;
  background: #fafafa;
  text-align: center;
}
tr td,
tr th {
  min-width: 60px;
}
tr:nth-child(2n) {
  background-color: #f1f6ff;
}
.table {
  width: 100%;
}
</style>
```

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282344485.png)

还有更多的使用方式，可以查看官方文档：https://alfred-skyblue.github.io/vue-draggable-plus/