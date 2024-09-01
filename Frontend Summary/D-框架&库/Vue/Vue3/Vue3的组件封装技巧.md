# Vue3的组件封装技巧

> 核心知识点`$attrs`,`$slots`

## **1、需求说明**

**需求背景**：日常开发中，我们经常会使用一些UI组件库`诸如and design vue、element plus等`辅助开发，提升效率。有时我们需要进行个性化封装，以满足在项目中大量使用的需求。

**错误示范**：基于`a-modal`封装一个自定义Modal组件：`修改modal样式，按钮样式、每次关闭后销毁、渲染到指定元素上等等`，后续项目的弹窗全部基于该自定义组件。

```
<template>
    <div ref="myModal" class="custom-modal"></div>
    <a-modal
        v-model:visible="visible"
        centered
        destroyOnClose
        :getContainer="() => $refs.myModal"
        @ok="handleOk"
        @cancel="handleCancel"
        :style="{ width: '560px', ...style }"
        :cancelText="cancelText"
        :okText="okText"
    >
        <!-- 以上皆为该组件的默认属性 -->
        <slot></slot>
    </a-modal>
</template>

<script setup>
const props = defineProps({
    title: {
        type: String,
        default: "",
    },
    style: {
        type: Object,
        default: () => ({}),
    },
    cancelText: {
        type: String,
        default: "取消",
    },
    okText: {
        type: String,
        default: "确定",
    },
});
const emits = defineEmits(["handleOk", "handleCancel"]);
const visible = ref(false);

const handleOk = () => {
    emits("handleOk");
};
const handleCancel = () => {
    emits("handleCancel");
};
defineExpose({ visible });
</script>

<style lang="less" scoped>
.custom-modal {
    :deep(.ant-modal) {
        //省略几百行样式代码
    }
}
</style>
```

代码封装完成,于是乎我们便能在项目中应用带有项目风格的弹窗

```
<CustomModal ref="xxxModal" title="xxx" @ok="onXxx" @cancel="onXxx" >content</CustomModal>
```

## **2、$attrs**

**问题来了**：一切看起来都挺正常。直到有一天同事说：`我想要去掉右上角的关闭按钮,能改成自定义的吗`

简单，直接加!

```
 <!-- 省略不相关代码 -->
<a-modal :closable="closable"></a-modal>
<script setup>
const props = defineProps({
    //...
    closable:{
        type: Boolean,
        default: false
    }
});
</script>
```

另一位同事说：`我不想让它是居中的，能改成自定义的吗`，还有一位同事说...

**思考**：这样的情况多了，就有点难顶。每次一有新的需求，我就得改这个组件，导致这个组件代码越来越冗余。那么**是否有一种方式能够将传进来的属性自动绑定给a-modal呢**，有，那儿就是`attrs`

![1725109524971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725109524971.png)



> 注意：
>
> 1.vue提供了`$attrs`这么一个属性用于接收父组件传递下来的属性，`$attrs`不包括已经写入props的值
>
> 2.如果父组件传递了style，class，那么这这些值不仅会存在于`$attrs`，还会默认绑定至根元素上。这一点需要注意

```
<modalTest :footer="null" :centered="false" :zIndex="999" />
//此时的$attrs
{ "footer": null, "centered": false, "zIndex": 999 }
```

有了这个组件实例，结合`v-bind`我们就可以这么写

```
 <a-modal
        v-model:visible="visible"
        centered
        destroyOnClose
        :getContainer="() => $refs.myModal"
        :style="{ width: '560px', ...style }"
        v-bind="$attrs"
    >
     <!-- 略  -->
    </a-modal>
```

这样一来，我们就可以使用`a-modal`提供的任意属性和方法了

## **3、$slots**

**问题来了**：插槽怎么办，例如`a-modal`就提供了许多插槽，是不是要用哪个就先在自定义组件上写好呢

**错误示例:**

```
<a-modal>
    <!-- default -->
    <slot></slot>

    <!-- title -->
    <template #title>
<slot name="title">{{ title }}</slot>
    </template>

    <!-- other -->
</a-modal>
```

弊端就像之前的，如果该原生提供了许多插槽，当有需要时岂不是频繁去修改自定义组件添加相应的插槽

其实利用`$slots`可以解决这个问题

![1725109567729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725109567729.png)

官网的这段话简明扼要的说出的插槽的原理，我们所传递的插槽最终都是变成

```
{
    'slotName'： fn(...args)  //fn返回一个虚拟DOM
    'defautl': fn(...args) //默认插槽
}
```

也就是我们传什么插槽进来，`$slots`就有什么值

那么我们可以遍历`$slots`中的值，有什么插槽我们便动态绑定什么插槽

```
<a-modal>
    <template v-for="(_val, name) in $slots" #[name]="options">
        <slot :name="name" v-bind="options || {}"> </slot>
    </template>
</a-modal>
```

> `#[name]="options"`，我们可以拿到原生`a-modal`在`name`这个插槽中传递来的一些状态`options`，并绑定在`<slot>`上。详情请查看官网：**作用域插槽**[1]。

这样一来，我们原生`a-modal`怎么使用插槽，自定义组件就怎么使用插槽

```
<CustomModal>
    <template #title="{arg1, arg2}">
        content
    </template>
</CustomModal>
```

至此，封装的代码如下

```
<template>
    <div ref="myModal" class="custom-modal"></div>
    <a-modal
        v-model:visible="visible"
        centered
        :getContainer="() => $refs.myModal"
        :style="{ width: '560px'}"
        destroyOnClose
        v-bind="$attrs"
    >
        <template v-for="(_val, name) in $slots" #[name]="ops">
            <slot :name="name" v-bind="ops || {}"> </slot>
        </template>
    </a-modal>
</template>

<script setup>
const visible = ref(false);
defineExpose({ visible });
</script>

<style lang="less" scoped>
.custom-modal {
    //style
}
</style>
```

还有许多优化的空间，例如当前父组件显隐该`Modal`需使用`ref`的方式访问`visible`。在vue3中也可以参考官网的做法这样子写

```
<template>
    <div ref="myModal" class="custom-modal"></div>
    <a-modal
        :visible="visible"
        //....
        v-bind="$attrs"
    >
          <!-- ...  -->
    </a-modal>
</template>

<script setup>
defineProps(['visible'])
const emit = defineEmits(); // 不用写"update:visible"，vue会自动加上
watch(
    () => props.visible,
    (newVal) => emit("update:visible", newVal);
);
</script>
```

那么使用这个控制这个组件的显示隐藏就方便许多了

```
<CustomModal v-model:visible="visible"></CustomModal>
```