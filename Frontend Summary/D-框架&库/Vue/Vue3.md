## Vue3

##### 与vue2的区别

```
(1)vue2与vue3 创建实例
vue2 以组件为单位 ，组件模块化，组件独立化 
vue3 以逻辑为单位 ，逻辑模块化，逻辑独立化 setup 
vue3 向下兼容,支持 vue2 的写法
vue2 创建实例化 new Vue() 
```

##### vue3 

```js
Vue.createApp(参数1,参数1-1).mount(参数2) 
	参数1：就是vue实例的配置项options ：data methods... 
	参数2：就是需要挂载的元素 String 容器（vue2中的el） 
// 例子： 
<div id="app"> 
    {{mes}} 
    <button @click="fn">button</button> 
</div> 
let options={ 
    data(){ 
        return { 
        	mes:"hello" 
        } 
    }, 
    methods:{ 
        fn(){ 
        	Salert('111') 
        } 
    } 
} 
Vue.createApp(options).mount('#app') 

// 参数1-1：可选， 我们可以将根 prop 传递给应用程序： 
const app = createApp( 
    { props: ['username'] }, 
    { username: 'Evan' } 
) 

<div id="app"> 
    <!-- 会显示 'Evan' --> 
    {{ username }} 
</div>
```

##### 组合式API

```js
import { defineComponent,...} from ‘vue’

export default defineComponent({
  inheritAttrs: false,
  props: {
      height: { type: undefined, default: 'auto' },
      value: { type: String, default: '' },
  },
  emits: ['change', 'get', 'update:value', 'closeLoading'],
  setup(props, { attrs, emit }) {
      return {}
  }
}
```

###### (1)defineComponent中的setup(props,context)

```js
setup 使用组合式 API 的地方 
setup 有两个参数 props 、context（上下文对象） 
setup 执行的时间点在 beforeCreate 之前 
setup 必须返回一个对象或函数 
	1.对象：这个对象的属性就是 data methods.. 
	2.函数：这个函数必须返回 h 渲染函数 Vue.h("h3",a) 'h3'-》标签 a -》变量 
setup 中不要使用this 

<div id="app"> 
    {{mes}} 
    <button @click="fn">button</button> 
</div> 
```

**①　setup 返回 object** 

```js

let options={ 
    setup(props,context){ 
        console.log(props,context) 
        
        //data 
        let mes = 'hello1' 
        
        //methods 
        function fn(){ 
        	alert('1') 
        } 
        return { 
            mes, 
            fn 
        } 
    } 
} 
Vue.createApp(options).mount('#app') 
在 fn 中修改数据，dom不会更新，想要成为响应式变量，就用ref 
Vue.ref('hello') 返回一个对象 
```

**②　setup 返回 函数function** 

```js
let options = { 
    setup(){ 
        let a = "hee" 
        return () => Vue.h("h3",a) 
    } 
} 
// 注意：vue2追求的开发理念是约定大于配置，vue3是配置大于约定
```

**③　setup参数1：props**

```js
export default { 
    props: { 
    	title: String 
    },//必须在props中定义，setup中才能使用 
    setup(props) { 
    	console.log(props.title) 
    } 
} 
// setup 函数中的 props 是响应式的，当传入新的 prop 时，它将被更新 . 
// 因为 props 是响应式的，你不能使用 ES6 解构，它会消除 prop 的响应性。

// 1.如果需要解构 prop，可以在 setup 函数中使用 toRefs 函数来完成此操作： 
import { toRefs } from 'vue' 
setup(props) { 
    const { title } = toRefs(props) 
    console.log(title.value) 
} 
// 2. 如果 title 是可选的 prop，则传入的 props 中可能没有 title 。在这种情况下，toRefs 将不会为 title 创建一个 ref 。你需要使用 toRef 替代它： 
import { toRef } from 'vue' 
setup(props) { 
    const title = toRef(props, 'title') 
    console.log(title.value) 
}
```

**④　setup参数2: context**

```js
context 是一个普通 JavaScript 对象，暴露了其它可能在 setup 中有用的值： 
export default { 
    setup(props, context) { 
       
        // Attribute (非响应式对象，等同于 $attrs) 
        console.log(context.attrs) 

        // 插槽 (非响应式对象，等同于 $slots) 
        console.log(context.slots) 

        // 触发事件 (方法，等同于 $emit) 
        console.log(context.emit) 

        // 暴露公共 property (函数) 
        console.log(context.expose) 
    } 
} 
不是响应式的，这意味着你可以安全地对 context 使用 ES6 解构。 
export default { 
    setup(props, { attrs, slots, emit, expose }) { 
    	... 
    } 
} 
// 根据 attrs 或 slots 的更改应用副作用，那么应该在 onBeforeUpdate 生命周期钩子中执行此操作。
```

###### (2)script setup 语法糖

**①　基本用法**

```js
<script setup> 
	console.log('hello script setup') 
</script> 
里面的代码会被编译成组件 setup() 函数的内容。这意味着与普通的 <script> 只在组件被首次引入的时候执行一次不同，<script setup> 中的代码会在每次组件实例被创建的时候执行
```

**②　响应式**

```vue
<script setup> 
    import { ref } from 'vue' 
    const count = ref(0) 
</script> 

<template> 
	<button @click="count++">{{ count }}</button> 
</template>
```

**③　使用组件**

```vue
// 引入 
<script setup> 
	import MyComponent from './MyComponent.vue' 
</script> 
// 不在components中注册 
// 使用 
<template> 
	<MyComponent /> 
</template>
```

**④　props 和 emits**

```js
// 在 <script setup>中必须使用 defineProps 和 defineEmits API 来声明 props 和 emits 
<script setup> 
    const props = defineProps({ 
    	foo: String 
    }) 
    const emit = defineEmits(['change', 'delete']) 
// setup code 
</script> 
    
1.defineProps 和 defineEmits 都是只在 <script setup> 中才能使用的编译器宏。他们不需要导入且会随着 <script setup> 处理过程一同被编译掉。 
2.defineProps 接收与 props 选项相同的值，defineEmits 也接收 emits 选项相同的值。 
3.defineProps 和 defineEmits 在选项传入后，会提供恰当的类型推断。 
4.传入到 defineProps 和 defineEmits 的选项会从 setup 中提升到模块的范围。因此，传入的选项不能引用在 setup 范围中声明的局部变量。这样做会引起编译错误。但是，它可以引用导入的绑定，因为它们也在模块范围内。
```

**⑤　defineExpose**

```js
// 使用 <script setup> 的组件是默认关闭的，也即通过模板 ref 或者 $parent 链获取到的组件的公开实例，不会暴露任何在 <script setup> 中声明的绑定。 (父获子，子获父) 
// 为了在 <script setup> 组件中明确要暴露出去的属性，使用 defineExpose 编译器宏： 
<script setup> 
    import { ref } from 'vue' 
    const a = 1 
    const b = ref(2) 
    defineExpose({ 
    	a, 
    	b 
    }) 
</script> 
// 当父组件通过模板 ref 的方式获取到当前组件的实例，获取到的实例会像这样 { a: number, b: number } (ref 会和在普通实例中一样被自动解包) 
```

**⑥　useSlots 和 useAttrs**

```js
// 在 <script setup> 使用 slots 和 attrs 的情况应该是很罕见的，因为可以在模板中通过 $slots 和 $attrs 来访问它们。在你的确需要使用它们的罕见场景中，可以分别用 useSlots 和 useAttrs 两个辅助函数： 
<script setup> 
import { useSlots, useAttrs } from 'vue' 
const slots = useSlots() 
const attrs = useAttrs() 
</script> 
// useSlots 和 useAttrs 是真实的运行时函数，它会返回与 setupContext.slots 和 setupContext.attrs 等价的值，同样也能在普通的组合式 API 中使用。 
```

**⑦　限制：没有 Src 导入**

```js
<script setup> 不能和 src attribute 一起使用。 
如： <script src=’./xx/xx.js’></script>
```

###### (3)watch侦听器

**①　vue2-watch**

```js
data(){ 
    return { 
    	count:{num:1} 
    } 
} 
// 1.函数形式 
watch:{ 
	count(newvalue,oldvalue){//do some ..} 
} 
// 2.对象形式 
watch:{ 
    count:{ 
        flush:'',//flush 选项可以更好地控制回调的时间。它可以设置为 'pre'、'post' 或 'sync'。 默认值是 'pre'，指定的回调应该在渲染前被调用 
        immediate:true, //boolean 为了发现对象内部值的变化，可以在选项参数中指定 deep: true。这个选项同样适用于监听数组变更。 
        deep:true, //boolean 为了发现对象内部值的变化，可以在选项参数中指定 deep: true。这个选项同样适用于监听数组变更。 
        handler(newvalue,oldvalue){ //do some ..} 
    } 
} 
// 3.停止侦听 
// $watch 返回一个取消侦听函数，用来停止触发回调： 
const app = createApp({ 
    data() { 
        return { 
        	a: 1 
        } 
    } 
}) 
const vm = app.mount('#app') 
const unwatch = vm.$watch('a', cb) 
// later, teardown the watcher 
unwatch() 
// 4.deep:true 导致newvalue与oldvalue 值相同了： 
// 解决：computed；需要不监听的属性通过计算属性 计算一下，然后 监听属性监听这个计算属性 
computed(){ 
    newcount(){ 
    	return JSON.parse(JSON.stringify(this.count)) //生成新的地址 
    } 
} 
```

```js
// 例子： 
data(){ 
    return { 
        count:{ 
        	num:1 
        } 
    } 
}, 
watch: { 
    newcount :{ 
        deep:true, 
        immediate:true, 
        handler(n,o){ 
        console.log(n,o); 
        } 
    } 
}, 
methods:{ 
    fn(){ 
    	this.count.num++ 
    } 
}, 
computed:{ 
    newcount(){ 
    	return JSON.parse(JSON.stringify(this.count)) 
    } 
} 
// vm.$watch 返回一个取消观察函数，用来停止触发回调： 
var unwatch = vm.$watch('a', cb) 
// 之后取消观察 
unwatch()
```

**②　vue3--watch(重大问题)**

```js
//默认深度监听 
setup(){ 
    let mes = ref('hello') 
    let obj = reactive({name:'张',person:{age:12}}) 

    //1.监听 ref 响应数据 
    //ref基本数据类型监听,监听值 
    watch(mes,(newvalue,oldvalue)=>{ 
    console.log('mes改变了',newvalue,oldvalue); 
    },{deep:true,immediate:true}) 

    //2.监听 reactive 响应数据 
    //reactive引用数据类型监听,监听的是地址, 
    watch(obj,(newvalue,oldvalue)=>{ 
    console.log('obj改变了',newvalue,oldvalue); 
    },{deep:false})//默认深度监听 

    //监听引用数据类型的某个字段(类型 引用数据类型) 
    watch(obj.person,(newvalue,oldvalue)=>{ 
    console.log('obj.person',newvalue,oldvalue); 
    },{}) 

    //监听引用数据类型的某个字段 （类型 基本数据类型） 
    watch(()=>obj.person.age,(newvalue,oldvalue)=>{ 
    console.log('obj.person',newvalue,oldvalue); 
    },{}) 

    //注意： newvalue,oldvalue相同是因为监听的是地址 
    //解决办法：使用计算属性computed ，监听计算属性 

    //3.监听多个数据 
    //监听多个数据 
    watch([mes,obj],(newvalue,oldvalue)=>{ 
    console.log('[mes,obj]',newvalue,oldvalue); 
    },{}) 
    
    return { 
        mes, 
        obj 
    } 
}
```

**③　vue3--watchEffect**

```js
在watchEffect 回调中，使用了那个数据，那个数据就会被监听 
watchEffect(()=>{ 
    mes.value 
    console.log("watchEffect"); 
})
```

###### (4)computed计算属性

```js

import {ref,computed} from 'vue' 
setup(){ 
    //1.函数形式 
    let as = computed(()=>{ 
    	return '1111' 
    }) 
    //2.对象形式 
    let as = computed({ 
        get(){return c.value}, 
        set(value){ 
        	c.value = value 
        } 
    }) 
    let c = ref('lisi') 
    function ff(){ 
    	as.value = 'zhangsan' 
    } 
    return { 
        as, 
        c , 
        ff 
    } 
}
```

###### (5)生命周期钩子函数--vue2/vue3

```
vue3 的hook函数有哪些 
beforeCreate 与 created 被 setup替换 
onBeforeMount onMounted onBeforeUpdate onUpdated 
onBeforeUnmount、onUnmounted替换了 beforeDestroy destroyed 销毁 
```

###### (6)组件通信

```js
// 父传子 
<son :mess="mess"></son> 
// 子组件：必须 props:['mess'], 
setup(props,context){ 
	console.log(props); // Proxy {mess: 'hello world'} 
} 
// 子传父 
// 子组件： 
<button @click="fn">子传父</button> 
emits:['fn'], // 强烈建议使用 emits 记录每个组件所触发的所有事件。 
// 不写会出现的问题：该事件现在会被触发两次: 
//一次来自 $emit()。 
//另一次来自应用在根元素上的原生事件监听器。 
setup(props,context){ 
    console.log(context); 
    function fn(){ context.emit(eventname,args) } 
    return { fn } 
} 
父组件 
<son @eventname="ff"></son> 
setup(){ 
    function ff(args){ } 
    return { ff } 
}
```

###### (7)全局事件总线

```js
vue2 
在 beforeCreate 中 Vue.prototype.$bus = this 
触发事件 this.$bus.$emit(eventname,arg) 
监听事件 this.$bus.$on(eventname,arg=> console.log(arg) ) 
执行一次事件 this.$bus.$once(eventname,arg=> console.log(arg) ) 
监听事件 this.$bus.$off(eventname) 
```

```js
vue3 
Vue 3.x 移除了 $on 、$off 和 $once 这⼏个事件 API 
事件总线模式可以被替换为使用外部的、实现了事件触发器接口的库，例如 mitt 或 tiny-emitter。 npm install mitt 

// eventbus.js 
import mitt from 'mitt'; 
const emitter = mitt(); 
export default emitter; 

//组件 
import emitter from './utils/eventbus'; 
setup(){ 

// listen to an event 
emitter.on('foo', e => console.log('foo', e) ) 

// listen to all events 
emitter.on('*', (type, e) => console.log(type, e) ) 

// fire an event 
emitter.emit('foo', { a: 'b' }) 

// clearing all events 
emitter.all.clear() 

// working with handler references: 
function onFoo() {} 
    emitter.on('foo', onFoo) // listen 
    emitter.off('foo', onFoo) // unlisten 
}
```

###### (8)路由

```js
①　配置路由
npm i vue-router@4 
import { createRouter, createWebHashHistory } from "vue-router"; 
const routes = [{ }] 
const router = createRouter({ 
history: createWebHashHistory(), 
routes, 
}); 
createApp(App).use(router).mount('#app')
```

```js
②　路由跳转
// vue2 
// 编程式跳转 （methods中） 跟history api 一样 
this.$router.push() 

// vue3 
// 动态路由 
{path:'/home/:id'} //形参 
import { useRouter,useRoute } from 'vue-router' 
setup(){ 
    let router = useRouter() //路由实例 
    let route = useRoute() //路由的参数 route.params.id 
    router.push('/home/123') //实参 
} 
// 在home中 
import { useRouter,useRoute } from 'vue-router' 
setup(){ 
    let route = useRoute() //路由的参数 route.params.id 
    route.params.id // '123' 
}
```

```
③　路由守卫
全局前置守卫:router.beforeEach(to,from,next可选) 
全局解析守卫:router.beforeResolve(to,from,next可选) 和 router.beforeEach 类似 
全局后置钩子:router.afterEach(to,from) 没有next 
路由独享的守卫:beforeEnter 
组件内的守卫 : 
beforeRouteEnter 
beforeRouteUpdate 
beforeRouteLeave
```

###### (9)Vuex

```js
vue 2 安装 vuex 3 ; npm i vuex@3 
vue 3 安装 vuex 4 npm i vuex@next 
//index.js 
import { createStore } from "vuex"; 
export default createStore({ 
    state: { 
    	collapse:false 
    }, 
    getters: {}, 
    mutations: {}, 
    actions: {}, 
    modules: {}, 
}); 
//组件 
import { useStore } from 'vuex' 
setup(){ 
	let store = useStore() 
	//可以使用 store 的方法 commit dispatch 
} 
//vue2 
import Vue from "vue" 
import Vuex from "vuex" 
Vue.use(Vuex) 
export default new Vuex.Store({ 
    state, 
    getters:{}, 
    mutations, 
    actions, 
    modules:{} 
})
```



```



```

###### 自定义 hook

```js

1.自定义hook 开头以 use 
2.自定义hook必须是一个函数 
3.hook 也是其他api 进行组合的 

import {ref,reactive,watch,computed, onMounted} from 'vue' 
function useMine(idname){ 
    //让元素的背景成红色 
    onMounted(()=>{ 
        let item = document.getElementById(idname) 
        item.style.backgroundColor='red' 
    }) 
} 
export default useMine
```

