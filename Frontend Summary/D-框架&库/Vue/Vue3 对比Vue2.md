# Vue3 对比Vue2

### 生命周期的变化

> 整体来看，变化不大，只是名字大部分需要 + `on`，功能上类似。使用上 Vue3 组合式 API 需要先引入；Vue2 选项 API 则可直接调用，如下所示。

```
// vue3
<script setup>     
	import { onMounted } from 'vue'
	onMounted(() => {  ...  })
	// 可将不同的逻辑拆开成多个onMounted，依然按顺序执行，不被覆盖onMounted(() => {  ...})
</script>
// vue2
<script>        
export default {               
	mounted() {  ...               }, 
}
</script>
```

常用生命周期表格如下所示。

| Vue2.x        | Vue3            |
| :------------ | :-------------- |
| beforeCreate  | Not needed*     |
| created       | Not needed*     |
| beforeMount   | onBeforeMount   |
| mounted       | onMounted       |
| beforeUpdate  | onBeforeUpdate  |
| updated       | onUpdated       |
| beforeDestroy | onBeforeUnmount |
| destroyed     | onUnmounted     |

> **Tips：** `setup`是围绕`beforeCreate`和`created`生命周期钩子运行的，所以不需要显式地去定义。

### 多根节点

> Vue3 支持了多根节点组件，也就是`fragment`。
>
> Vue2中，编写页面的时候，我们需要去将组件包裹在`<div>`中，否则报错警告。

```vue
<template>
    <div>    
        <header>...</header>    
        <main>...</main>    
        <footer>...</footer>  
    </div>
</template>
```

> Vue3，我们可以组件包含多个根节点，可以少写一层，niceeee ！

```vue
<template>
    <header>...</header>
    <main>...</main>
    <footer>...</footer>
</template>
```

### 异步组件

> Vue3 提供 `Suspense`组件，允许程序在等待异步组件时渲染兜底的内容，如 loading ，使用户体验更平滑。使用它，需在模板中声明，并包括两个命名插槽：`default`和`fallback`。`Suspense`确保加载完异步内容时显示默认插槽，并将`fallback`插槽用作加载状态。

```vue
<tempalte>
    <suspense>
        <template #default>
        	<todo-list />
        </template>
        
        <template #fallback>
            <div>
            	Loading...
            </div>
        </template>
    </suspense>
</template>
```

> 真实的项目中踩过坑，若想在 setup 中调用异步请求，需在 setup 前加`async`关键字。这时，会受到警告`async setup() is used without a suspense boundary`。
>
> 解决方案：在父页面调用当前组件外包裹一层`Suspense`组件。

### Teleport

> Vue3 提供`Teleport`组件可将部分DOM移动到 Vue app之外的位置。比如项目中常见的`Dialog`组件。

```vue
<button @click="dialogVisible = true">点击</button>

<teleport to="body">
	<div class="dialog" v-if="dialogVisible">  	</div>
</teleport>
```

### 组合式API

> Vue2 是 `选项式API（Option API）`，一个逻辑会散乱在文件不同位置（data、props、computed、watch、生命周期函数等），导致代码的可读性变差，需要上下来回跳转文件位置。Vue3 `组合式API（Composition API）`则很好地解决了这个问题，可将同一逻辑的内容写到一起。
>
> 除了增强了代码的可读性、内聚性，组合式API 还提供了较为完美的逻辑复用性方案，举个🌰，如下所示公用鼠标坐标案例。

```vue
// main.vue
<template>
	<span>mouse position {{x}} {{y}}</span>
</template>

<script setup>
    import { ref } from 'vue'
    import useMousePosition from './useMousePosition'
    const {x, y} = useMousePosition()}
</script>
```

```vue
// useMousePosition.js
<script setup>
    import { ref, onMounted, onUnmounted } from 'vue'
    function useMousePosition() {
        let x = ref(0)
        let y = ref(0)
        
        function update(e) {
            x.value = e.pageX
            y.value = e.pageY
        }
        onMounted(() => {    
            window.addEventListener('mousemove', update)
        })
        onUnmounted(() => {    			
            window.removeEventListener('mousemove', update)
        })
        return {
            x,
            y
        }
    }
</script>
```

> 解决了 Vue2 `Mixin`的存在的命名冲突隐患，依赖关系不明确，不同组件间配置化使用不够灵活。

### 响应式原理

> Vue2 响应式原理基础是`Object.defineProperty`；Vue3 响应式原理基础是 `Proxy`。

#### Object.defineProperty

> 基本用法：直接在一个对象上定义新的属性或修改现有的属性，并返回对象。**Tips：** `writable` 和 `value` 与 `getter` 和 `setter` 不共存。

```js
let obj = {}
let name = '瑾行'
Object.defineProperty(obj, 'name', {
    enumerable: true, // 可枚举（是否可通过for...in 或 Object.keys()进行访问）
    configurable: true,// 可配置（是否可使用delete删除，是否可再次设置属性）  
    value: '', // 任意类型的值，默认undefined 
    writable: true, // 可重写
    get: function() {    return name  },
    set: function(value) {    name = value  }
})
```

```js
// 搬运 Vue2 核心源码，略删减。
function defineReactive(obj, key, val) {
    // 一 key 一个 dep  const dep = new Dep()    // 获取 key 的属性描述符，发现它是不可配置对象的话直接 return  
    const property = Object.getOwnPropertyDescriptor(obj, key)
    if (property && property.configurable === false) {
    return
    }
    // 获取 getter 和 setter，并获取 val 值
    const getter = property && property.get
    const setter = property && property.set  
    if((!getter || setter) && arguments.length === 2) { 
    val = obj[key]
    } 
    // 递归处理，保证对象中所有 key 被观察
    let childOb = observe(val)    
    Object.defineProperty(obj, key, {
    enumerable: true,
    configurable: true,
    // get 劫持 obj[key] 的 进行依赖收集
    get: function reactiveGetter() {
        const value = getter ? getter.call(obj) : val
        if(Dep.target) {
            // 依赖收集
            dep.depend()
            if(childOb) { 
                // 针对嵌套对象，依赖收集
                childOb.dep.depend()
                // 触发数组响应式
                if(Array.isArray(value)) {
                    dependArray(value)
                }
            }
        }
    }
    return value
    })
    // set 派发更新 obj[key]
    set: function reactiveSetter(newVal) {
        ...
        if(setter) {
        	setter.call(obj, newVal)
        } else {
        	val = newVal
        }
        // 新值设置响应式
        childOb = observe(val)
        // 依赖通知更新
        dep.notify()
	}
}
```



> 那 Vue3 为何会抛弃它呢？那肯定是有一些缺陷的。
>
> 主要原因：无法监听对象或数组新增、删除的元素。Vue2 方案：针对常用数组原型方法`push`、`pop`、`shift`、`unshift`、`splice`、`sort`、`reverse`进行了hack处理；提供`Vue.set`监听对象/数组新增属性。对象的新增/删除响应，还可以`new`个新对象，新增则合并新属性和旧对象；删除则将删除属性后的对象深拷贝给新对象。

> **Tips：** `Object.defineOProperty`是可以监听数组已有元素，但 Vue2 没有提供的原因是`性能`问题，具体可看见**参考**第二篇 ~。

#### Proxy

> `Proxy`是ES6新特性，通过第2个参数`handler`拦截目标对象的行为。相较于`Object.defineProperty`提供语言全范围的响应能力，消除了局限性。但在兼容性上放弃了（IE11以下）
>
> **局限性**
>
> 1. 对象/数组的新增、删除。
> 2. 监测.length修改。
> 3. Map、Set、WeakMap、WeakSet的支持。

```js
// 基本用法：创建对象的代理，从而实现基本操作的拦截和自定义操作。
const handler = {
    get: function(obj, prop) {
    	return prop in obj ? obj[prop] : ''
    },
    set: function() {},
    ...
}
```

```js
// 搬运 Vue3 的源码 reactive.ts 文件
function createReactiveObject(target, isReadOnly, baseHandlers, collectionHandlers, proxyMap) {
    ...
    // collectionHandlers: 处理Map、Set、WeakMap、WeakSet
    // baseHandlers: 处理数组、对象
    const proxy = new Proxy(target,targetType === TargetType.COLLECTION ? collectionHandlers : baseHandlers)  
	proxyMap.set(target, proxy)
    return proxy
}
```

> 以 baseHandlers.ts 为例，使用Reflect.get而不是`target[key]`的原因是receiver参数可以把this指向getter调用时，而非Proxy构造时的对象。

```js
// 依赖收集
function createGetter(isReadonly = false, shallow = false) {
return function get(target: Target, key: string | symbol, receiver: object) {
...
// 数组类型
const targetIsArray = isArray(target)
if (!isReadonly && targetIsArray && hasOwn(arrayInstrumentations, key)) {
return Reflect.get(arrayInstrumentations, key, receiver)
}
// 非数组类型
const res = Reflect.get(target, key, receiver);
// 对象递归调用
if (isObject(res)) {
return isReadonly ? readonly(res) : reactive(res)
}
return res
}}
// 派发更新
function createSetter() {
    return function set(target: Target, key: string | symbol, value: unknown, receiver: Object) {
        value = toRaw(value)
        oldValue = target[key]
        // 因 ref 数据在 set value 时就已 trigger 依赖了，所以直接赋值 return 即可
        if (!isArray(target) && isRef(oldValue) && !isRef(value)) {
            oldValue.value = value
            return true
        }
        // 对象是否有 key 有 key set，无 key add
        const hadKey = hasOwn(target, key)
        const result = Reflect.set(target, key, value, receiver)
        if (target === toRaw(receiver)) {
            if (!hadKey) {
                trigger(target, TriggerOpTypes.ADD, key, value)
            } else if (
                hasChanged(value, oldValue)) { 
                    trigger(target, TriggerOpTypes.SET, key, value, oldValue)
                }
            }
        return result
    }
}
```

### 虚拟DOM

> Vue3 相比于 Vue2 虚拟DOM 上增加`patchFlag`字段。我们借助`Vue3 Template Explorer`来看。

```js
<div id="app">
    <h1>技术摸鱼</h1>
    <p>今天天气真不错</p>
    <div>{{name}}</div>
</div>
```

渲染函数如下。

```js
import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createElementBlock as _createElementBlock, pushScopeId as _pushScopeId, popScopeId as _popScopeId } from "vue"
const _withScopeId = n => (_pushScopeId("scope-id"),n=n(),_popScopeId(),n)
const _hoisted_1 = { id: "app" }
const _hoisted_2 = /*#__PURE__*/ _withScopeId(
    () =>
	/*#__PURE__*/
    _createElementVNode("h1", null, "技术摸鱼", -1 /* HOISTED */)
 )
const _hoisted_3 = /*#__PURE__*/ _withScopeId(() => /*#__PURE__*/_createElementVNode("p", null, "今天天气真不错", -1 /* HOISTED */))
export function render(_ctx, _cache, $props, $setup, $data, $options) {
    return (_openBlock(), _createElementBlock("div", _hoisted_1, [    _hoisted_2,    _hoisted_3,    _createElementVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */) ]))
}
```

> 注意第 3 个`_createElementVNode`的第 4 个参数即`patchFlag`字段类型，字段类型情况如下所示。1 代表节点为动态文本节点，那在 diff 过程中，只需比对文本对容，无需关注 class、style等。除此之外，发现所有的静态节点，都保存为一个变量进行`静态提升`，可在重新渲染时直接引用，无需重新创建。

```js
export const enum PatchFlags {
    TEXT = 1, // 动态文本内容
    CLASS = 1 << 1, // 动态类名
    STYLE = 1 << 2, // 动态样式
    PROPS = 1 << 3, // 动态属性，不包含类名和样式
    FULL_PROPS = 1 << 4, // 具有动态 key 属性，当 key 改变，需要进行完整的 diff 比较
    HYDRATE_EVENTS = 1 << 5, // 带有监听事件的节点
    STABLE_FRAGMENT = 1 << 6, // 不会改变子节点顺序的 fragment
    KEYED_FRAGMENT = 1 << 7, // 带有 key 属性的 fragment 或部分子节点
    UNKEYED_FRAGMENT = 1 << 8,  // 子节点没有 key 的fragment
    NEED_PATCH = 1 << 9, // 只会进行非 props 的比较
    DYNAMIC_SLOTS = 1 << 10, // 动态的插槽
    HOISTED = -1,  // 静态节点，diff阶段忽略其子节点
    BAIL = -2 // 代表 diff 应该结束
}
```

### 事件缓存

> Vue3 的 `cacheHandler`可在第一次渲染后缓存我们的事件。相比于 Vue2 无需每次渲染都传递一个新函数。加一个`click`事件。

```jsx
<div id="app">
    <h1>技术摸鱼</h1>
    <p>今天天气真不错</p>
    <div>{{name}}</div>
    <span onCLick="() => {}"><span>
</div>
```

渲染函数如下

```js
import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createElementBlock as _createElementBlock, pushScopeId as _pushScopeId, popScopeId as _popScopeId } from "vue"const _withScopeId = n => (_pushScopeId("scope-id"),n=n(),_popScopeId(),n)const _hoisted_1 = { id: "app" }const _hoisted_2 = /*#__PURE__*/ _withScopeId(() => /*#__PURE__*/_createElementVNode("h1", null, "技术摸鱼", -1 /* HOISTED */))const _hoisted_3 = /*#__PURE__*/ _withScopeId(() => /*#__PURE__*/_createElementVNode("p", null, "今天天气真不错", -1 /* HOISTED */))const _hoisted_4 = /*#__PURE__*/ _withScopeId(() => /*#__PURE__*/_createElementVNode("span", { onCLick: "() => {}" }, [  /*#__PURE__*/_createElementVNode("span")], -1 /* HOISTED */))export function render(_ctx, _cache, $props, $setup, $data, $options) {  return (_openBlock(), _createElementBlock("div", _hoisted_1, [    _hoisted_2,    _hoisted_3,    _createElementVNode("div", null, _toDisplayString(_ctx.name), 1 /* TEXT */),    _hoisted_4  ]))}
```

### TypeScript 支持

> Vue3 由TS重写，相对于 Vue2 有更好地`TypeScript`支持。
>
> - Vue2 `Option API`中 option 是个简单对象，而TS是一种类型系统，面向对象的语法，不是特别匹配。
> - Vue2 需要`vue-class-component`强化vue原生组件，也需要`vue-property-decorator`增加更多结合Vue特性的装饰器，写法比较繁琐。