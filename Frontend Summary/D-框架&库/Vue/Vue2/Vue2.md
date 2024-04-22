## Vue2

#### 1. **网址**

```
https://cn.vuejs.org/index.html 

源码网站 https://github.com/vuejs/vue 
```

#### 2. **Vue**

```
1、实现了数据和视图的分离 

2、options 

​    a. el 选择应用的出口容器 
​    b. data 存储应用的临时数据 data 中的字段跟后台的字段保持一致   

模板语法： 

​    vue的模板使用 {{}} 渲染数据  mustache语法（胡子） 
​    这里面的数据与页面是响应式的 （一改都改） 
​    **实现的原理 用了观察者模式和发布订阅模式** 
​    声明式渲染是响应式的 
```

##### (1) **渐进式框架** 

```
渐进式：随着开发应用增多，vue上的逻辑越来越多，vue这个框架会越来越笨重。 

在vue项目中vue项目中，随着项目的业务增加，我们会在vue身上注入更多的逻辑，如：vue-router vuex等，都是项目中添加在vue身上，使项目体积越来越大。
```

##### (2) **MVVM**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps1.jpg) 

##### (3) **双向数据绑定原理**

```
双向数据绑定的原理：采用“数据劫持”结合“发布者-订阅者”模式的方式，通过“Object.defineProperty()”方法来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。

所谓要实现双向数据绑定，vue中内部采用了发布-订阅模式。内部结合了Object.defineProperty这个ES5的新特性（ie8浏览器可不支持哦...），对vue传入的数据进行了相应的数据拦截，为其动态添加get与set方法。当数据变化的时候，就会触发对应的set方法，当set方法触发完成的时候，内部会进一步触发watcher,当数据改变了，接着进行虚拟dom对比，执行render,后续视图更新操作完毕。
```

##### (4) **虚拟DOM**

```
**虚拟 dom** 是相对于浏览器所渲染出来的真实 dom 的

Vnode的本质就是**用树型结构的JS对象来描述真实的DOM结构的信息**，这个树结构的JS对象包含了整个DOM结构的信息.
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps2.jpg) 

###### a. 虚拟 DOM 实现原理

```
虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要 的 dom 操作，从而提高性能。

**具体实现步骤如下：**

用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树， 插到文档当中；

当状态变更的时候，重新构造一棵新的对象树。通过diff 算法，比较新旧虚拟 DOM 树的差异。

根据差异，对真正的 DOM 树进行增、删、改。
```

###### b. 虚拟 DOM 的优缺点

```
优点：
	降低浏览器性能消耗
	diff算法,减少回流和重绘
	跨平台

缺点：
	首次显示要慢些:
​		首次渲染大量DOM时，由于多了一层虚拟DOM的计算, 会比innerHTML插入慢
	无法进行极致优化：
​		虽然虚拟 DOM + 合理的优化，足以应对绝大部分应用的性能需求，但在一些性能要求极高的应用中 无法进行针对性的极致优化
```

##### (5) Key的作用：唯一标识符

```
1.虚拟DOM中key的作用：
	key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：

2.对比规则
	（1）旧虚拟DOM中找到了与新虚拟DOM相同的key：
		①若虚拟DOM中的内容没变，直接使用之前的真实DOM。
		②若虚拟DOM中的内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM。
	（2）旧虚拟DOM中未找到与新虚拟DOM中相同的key
		创建新的真实DOM，随后渲染到页面
3.用index作为key可能会引发的问题：
	（1）若对数据进行：逆序添加、逆序删除等破坏顺序操作：
		会产生没有必要的真实DOM更新= = >界面效果没问题，但效率低。
	（2）如果结构中还包含输入类的DOM：
		会产生错误DOM更新 = = >界面有问题。
```

![1709175307932](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175307932.png)

![1709175315630](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175315630.png)

```
4.开发中如何选择key？
	（1）最好使用每条数据的唯一标识，如id、手机号、身份证号、学号等唯一值。
	（2）如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。
5.v-if中的key：管理可复用的元素
	Vue会尽可能的高效的渲染元素，通常会复用已有元素，而不是从头渲染；
	所以vue提供了一种方式来声明“这个元素是完全独立的，请不要复用它”，只需要添加一个唯一值的key属性
		
		<input placeholder="user" />
		
	vue中用到input 就会复用上边的dom，只是替换了一下placeholder的值
	想要不复用就添加一个key
	
		<input placeholder="user" key="user-input" />
```

##### 多页面应用 MPA

```
整个项目有多个html，页面都是以html展示 
```

##### 单页面应用 SPA

```
 **single page application** 

每个项目中自使用一个html 作为项目的载体，其他都是以组件的形式展示。 


**单页面应用优势**：
	减少服务器交互次数，用户体验感更强一些（除了第一次页面是从服务器请求，后面的页面都在客户端操作），更适用于移动端，后台管理项目 

**缺点**：单页面导致需要seo 的项目不支持，对大型的电商项目不友好  解决：查看文档 nuxt.js 

**服务端渲染**：把前期需要渲染的工作都在服务端完成了，直接把渲染后的数据返回给客户端（不需要用户等待） 

**客户端渲染**：用户打开终端，访问客户端渲染的网站，用户需要等待这个客户端渲染的过程（需要O解决客户端首页白屏的方案）
```

#### 1. **Vue 指令** 

**v-if、v-else-if、v-else 条件指令**

```
根据指令的值判断当前的元素移除不移除 对DOM节点  

v-else 与v-if 一起使用时，中间不能被打断 
```

**v-bind 动态绑定元素的属性  简写  :**

```
class 和style属于标签的属性，对属性的动态绑定用 v-bind 

<div v-bind:class=""></div>

绑定语法 :
1.值可以是变量 v-bind:class="aa" aa就是data中的数据 
2.值是数组 v-bind:class="[aa]" aa就是data中的数据  
3.值是对象 v-bind:class="{ss:false}"  ss就是类名，值可以是布尔值或者代表布尔值的data中的数据 

```

**v-show 条件指令**

```
根据指令的值判断当前的元素显示隐藏 对display的判断 none或block等
```

**v-show 与 v-if 的区别**

```
v-show 是display的block 与none 的设置 （元素频繁切换时，建议使用v-show）

v-if是dom节点的删除和添加  
```

**v-for 循环指令**

```
v-for="(值/元素，键/下标) in obj/arr"不论数组还是对象 都是先值后键 
```

**v-once**

```
v-once所在的节点在初次动态渲染后，就视为静态内容了，以后数据的改变捕获引起v-once所在结构的更新，用于优化性能  
```

**v-model**

```
<input type="text" > 

v-model 收集的是 value值，用户输入的就是value值 

<input type="radio">  

v-model收集的是value值，且要给标签配置value值 

<input type="checkbox">  

1.没有配置input的value值，那么收集的就是checked 布尔值 

2.配置input的value值： 

	a.v-model 的初始值是非数组，那么收集的就是checked  

	b.v-model 的初始值是数组，那么收集的就是value组成的数组 
```

> 单选框 name 属性值必须相同 
>
> input 与 label ：input 的id 与label 的for一致 (点击label中的字，input就会获取焦点)
>
> v-model：实际时v-bind与v-on的语法糖

**v-text**

```
向所在节点中渲染文本内容;会替换掉节点中的内容；不支持结构解析(有标签也会以文本展示) 
```

**v-html**

```
向所在节点中渲染包含html结构的内容;会替换掉节点中的内容；支持结构解析； 会导致**xss攻击** 
```

**v-on**

```
事件绑定：v-on:eventname  v-on:click  

注意：onclick 事件名是click，on是事件修饰符 

事件修饰符：	
	@click.stop="" 阻止事件冒泡 
	@click.prevent="" 阻止事件默认行为 
	@click.capture="" 阻止事件捕获 
			.native - 监听组件根元素的原生事件。  

事件修饰符可以串联 @click.stop.prevent 

事件指令后面绑定的是:
	1.事件绑定 
	2.表达式 （赋值运算 函数运行 逻辑运算 比较运算 四则运算 三目运算，不能是语句if for） 
	3.Vue中的methods 里的事件函数不能使用箭头函数，因为没有this指向 

**原生js 事件绑定的方式** 
	1.dom 0级 div.onclick 
	2.dom 2级 div.addEventListener('click',fn,boolean) 
	上面的绑定方式 都属于dom操作 ，Vue是不允许dom操作（vue内部进行diff算法，这个算法主要进行虚拟dom viltual Dom ,虚拟dom获取不到dom节点，使用指令绑定，v-on:click=""） 
```

**v-pre**

```
跳过其所在的节点的编译过程，直接显示在页面上，一般用在没有用到 插值语法 指令的节点上，会加快编译 
```

**v-clock（没有值）**

```
vue实例创建完毕并接管容器后，会删除v-clock属性；使用css配合c-clock可以解决网速慢时，页面展示出 {{name}} 的问题  [v-clock]:{display:none}; 
```

#### 1-1. 修饰符

```
.lazy 失去焦点在收集数据 

.number

.trim
```

#### 2. **生命周期**

<img src="file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps3.jpg" alt="img" style="zoom: 67%;" />![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps4.jpg)<img src="file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps3.jpg" alt="img" style="zoom: 67%;" />![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps4.jpg) 

**Created beforemount mounted 执行异步**

```
created中的同步任务–mounted中的同步任务–created中的异步任务–mounted中的同步任务
如果考虑用户体验方面的话，在created中调用异步请求最佳，用户就越早感知页面的已加载，毕竟越早获取数据，在mounted实例挂载的时候就越及时。
```

#### 3. Options

##### **el** 

```
选择应用的出口容器 
```

##### **template**

```
模板
```

##### **name &** **name属性的作用**

**a. 全局注册组件时使用**

```js
封装组件

import ContractList from './src/index.vue'

ContractList.install = function(Vue) {
    Vue.component(ContractList.name, ContractList)
}

export default ContractList
```

 

**b. 项目中有用到keep-alive时**

```js
name可以用作include和exclude的值

在keep-alive中加入exclude属性，exclude="Table"这样就不会对Table组件进行缓存，

第二次进入该页面时就会得到最新数据。

<keep-alive exclude="Table">
    <router-view/>
</keep-alive>

当组件在 <keep-alive> 内被切换

它的 activated 和 deactivated 这两个生命周期钩子函数将会被对应执行。

activated:被 keep-alive 缓存的组件激活时调用。

deactivated被 keep-alive 缓存的组件停用时调用。这里把this.getData();放到activated中
```

**c. 递归组件**

```js
当你使用递归组件时，对子组件进行递归
子组件的 name 属性
父组件中 注册的子组件的component 一致才可以使用递归

```

```vue
// 父组件：
<template>
  <div>
    <ul>
      <tree :model="treeData"></tree>
   </ul>
  </div>
</template>
<script>
  import tree from '@/components/tree'
  export default {
    data () {
      return {
        treeData: {
          title: '1',
          children: [
            {title: '1-1'},
            {
              title: '1-2',
              children: [
                {title: '1-2-1'},
                {title: '1-2-2'},
                {title: '1-2-3'},
              ]
            },
            {
              title: '1-3',
              children: [
                {
                  title: '1-3-1',
                  children: [
                    {
                      title: '1-3-1-1'
                    },
                    {
                      title: '1-3-1-2'
                    }
                  ]
                },
                {title: '1-3-2'},
                {title: '1-3-3'},
              ]
            },
          ]
        }
      }
    },
    created(){   },
    methods:{   },
  }

</script>

<style scoped lang="scss"></style>
```

```vue
// 子组件：

<template>
  <div>
    <div v-for="(item, index) in model" :key="index">
      {{ item.title }}
      <div
        v-if="item.children && item.children.length"
        style="margin-left:20px;"
      \>
        <tree :model="item.children"></tree>
      </div>
    </div>
  </div>

</template>

<script>
export default {
  // 这里要写name:tree才可以递归
  name: "tree",
  props: {
    model: Array
  },
  data() {
    return {};
  }

};
</script>

<style scoped lang="scss"></style>
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps6.jpg) 

**d. vue-tools插件 调试时**

```
vue组件中的name决定了vue-tools的显示。
```

![1709175353306](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175353306.png)

##### **data** 

**a. data中的数据为对象或数组时，怎样才能使数据成为响应式**

```
1) 监测Object中的数据
	通过setter实现监视，且要在new Vue时就传入要监视的数据：
		A. 对象中后追加的属性，vue默认不做响应式处理
		B. 如需给后添加的属性做响应式处理，请使用如下api

Vue.set(target,’属性/index’,value) 或 vm.$set((target,’属性/index’,value)

2) 监测Array中的数据
	通过包裹数组更新元素的方法实现：本质做了两件事：
		A. 调用原生对应的方法对数组进行更新
		B. 重新解析模板，进而更新页面

push()、pop、shift、unshift、splite、sort、reverse或者 Vue.set() vm.$set()

Vue.set()、vm.$set() 不能给vm或者vm的**根数据对象**添加属性
```

**b. 组件中的data为什么必须是一个函数形式，并且返回一个对象**

```
根实例对象data可以是对象也可以是函数（根实例是单例），不会产生数据污染情况
组件实例对象data必须为函数，目的是为了防止组件复用多个组件实例对象之间共用一个data（浅拷贝），产生数据污染。采用函数的形式，initData时会将其作为工厂函数都会返回全新data对象
```

##### methods

```
**事件函数存放在 options 中的 methods 字段中**
```

##### computed：计算属性

```
计算属性本质还是属性，在结构上像个方法，这个属性的属性值是这个方法的返回值 
```

```js
计算属性的书写方式：

new Vue({ 
    el:'#app', 
    data:{ 
      ss:'hello', 
    }, 
	// 1. 函数形式
    computed:{ 
      mess(){ //写逻辑的空间 
        return this.ss.split('').reverse().join('') 
      } 
    } 
    // 2. 对象形式
    computed:{ 
        mess:{ 
            get(){ return }, 
            set(value){ } 
        } 
    } 
  }) 

<div id="app">{{mess}}</div>  
```

###### computed与methods的区别

```
\1. 在使用时， computed可以当做属性使用，而methods则可以当做方法调用。
\2. computed可以具有getter和setter方法，因此可以赋值，而methods是不行的。
\3. computed无法接收多个参数，而methods可以
\4. computed是有缓存的，而methods没有

methods 与 computed 都可以达到相同的效果
methods 没法缓存，函数执行完毕后都销毁了， computed可以缓存
```

###### Watch与computed区别

```
1、computed支持缓存，只有依赖数据发生改变，才会重新进行计算；而watch不支持缓存，数据变，直接会触发相应的操作。
2、computed不支持异步，当computed内有异步操作时无效，无法监听数据的变化；而**watch支持异步**。
```

##### watch 侦听属性

```js
侦听器 用于监听数据状态的变化的 
watch 的字段和 data 中的字段保持一致

const vm = new Vue({ 
    el:'#app', 
    data:{ 
      aa:'hello', 
      as:'world' 
    }, 
	// 1. 函数形式
    watch:{ // 侦听器的方法名跟data中字段一致 
      aa(){  console.log('=====');  }, 
      as(){  console.log('=====');  } 
    } 

  }) 

 <div id="app">{{aa}}{{as}}</div>

 在控制台 改变vm中data的数据 vm.aa = 1 
```

```
vm.$watch 返回一个取消观察函数，用来停止触发回调：
var unwatch = vm.$watch('a', cb)
// 之后取消观察
unwatch()
```

```js
// 2. 对象形式
watch: {
	'docData.doc_id': { //可以监听docData 也可以监听单个key，如：’docData.doc_id'
		handler(newVal, oldVal) {   },
		deep: true,
		immediate: true
	}  
}

**immediate**: 当值第一次绑定时，不会执行监听函数，只有值发生改变时才会执行。如果我们需要在最初绑定值的时候也执行函数，则就需要用到immediate属性。

**deep**: 当需要监听一个对象的改变时，普通的watch方法无法监听到对象内部属性的改变，此时就需要deep属性对对象进行深度监听。
```

```js
// 3. watch 与 methods 结合
watch: {
  data： 'changeData' // **值可以为methods的方法名**
}，
methods: {
   changeData(curVal,oldVal){
  　　　 conosle.log(curVal,oldVal)
　　}
}
```

###### vue中watch和created哪个先执行

```
官网的生命周期图中，init reactivity是晚于beforeCreate但是早于created的。
watch加了immediate: true，应当同init reactivity周期一同执行，会早于created执行。
而正常的watch，则是mounted周期后触发data changes的周期执行，晚于created。
并且首次不执行watch，watch加了immediate: true，首次也会执行
```

###### watch监听:新旧值是一样

```
使用watch监听数组或对象时, 发现返回的新旧值是一样的, 官方文档给出了答案 :

这需要使用到 this.$set方法 
它接受三个参数 : (object / array , properyName / index , value) ,
```

**问题：**

```js
在 data中命名一个对象 a , 一个数组 b 

data () {
	retutn {
		a : {name : 'lily', age : 18} ,
		b : [
			{name : '小明' , age : 14} ,
			{name : '小红' , age : 15}
		]
	}
} , 

在methods中对 a , b 进行操作 : 

methods : {
	set() {
		this.$set(a , 'age' , 20);
		this.$set(b, 1 , {name : '小红' , age : 16} ;
	}

}
```

**解决方法：**

```js
**在computed监听新变量 , 一定要用JSON.parse 和 JSON.stringify 转化一下, 不然watch监听时返回的新旧值还是一样的 .**

computed : {
	newA : function () {
		return JSON.parse(JSON.stringify(this.a))
	} , 
	newB : function () {
		return JSON.parse(JSON.stringify(this.b))
	}
}

watch监听 , handler函数的两种写法都是可以的, 但是不能使用箭头函数 . 

watch : {
	newA : {
		handler : function (newVal, oldVal) {
			console.log(newVal,oldVal)
		},
		immediate : true //初始化页面后立即监听
	} , 
	newB : {
		handler (newVal , oldVal) {
			console.log(newVal , oldVal)
		} ,
		immediate : true
	}
```

触发set方法后 , 最后打印的结果 :

![1709175375348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175375348.png)

##### directives 自定义指令

```js
directives: {
      **focus**: {
        // 指令的定义
        inserted: function (el) {
          el.focus()
        }
      }
    }

<input v-focus>
```

 

```js
directives: {
      'big-font': {
        // 指令的定义
        inserted: function (el) {
          el.focus()
        }
      }
    }

<input v-big-font>
```

**钩子函数**

```js
一个指令定义对象可以提供如下几个钩子函数 (均为可选)：

bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
update：所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。
componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
unbind：只调用一次，指令与元素解绑时调用。
接下来我们来看一下钩子函数的参数 (即 el、binding、vnode 和 oldVnode)。
```

**钩子函数参数**

钩子函数会被传入以下参数：

```js
el：指令所绑定的元素，可以用来直接操作 DOM。
binding：一个对象，包含以下 property：
	name：指令名，不包括 v- 前缀。
	value：指令的绑定值，例如：v-my-directive="1 + 1" 中，绑定值为 2。
	oldValue：指令绑定的前一个值,仅在 update 和 componentUpdated 钩子中可用.无论值是否改变都可用.
	expression：字符串形式的指令表达式。例如 v-my-directive="1 + 1" 中，表达式为 "1 + 1"。
	arg：传给指令的参数，可选。例如 v-my-directive:foo 中，参数为 "foo"。
	modifiers:一个包含修饰符的对象。例如：v-my-directive.foo.bar 中,修饰符对象为 { foo: true, bar: true }。
	
vnode：Vue 编译生成的虚拟节点。移步vode api来了解更多详情。
oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。

除了 el 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 dataset来进行。
```

##### filters 过滤

```js
filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
```

**使用：**

```js
	<!-- 在双花括号中 -->
	{{ message | capitalize }}
        
	<!-- 在 `v-bind` 中 -->
	<div v-bind:id="rawId | formatId"></div>
```

```js
过滤器可以串联：
	{{ message | filterA | filterB }}
filterA 被定义为接收单个参数的过滤器函数,表达式 message 的值将作为参数传入到函数中。然后继续调用同样被定义为接收单个参数的过滤器函数 filterB，将 filterA 的结果传递到 filterB 中。

过滤器是 JavaScript 函数，因此可以接收参数：
	{{ message | filterA('arg1', arg2) }}
filterA 被定义为接收三个参数的过滤器函数。其中 message 的值作为第一个参数，普通字符串 'arg1' 作为第二个参数，表达式 arg2 的值作为第三个参数。
```

##### mixins 混入

```js
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
// 定义一个使用混入对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})
var component = new Component() 
// => "hello from mixin!"
```

###### 选项合并：

```
当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。
	数据对象在内部会进行递归合并，并在发生冲突时以组件数据优先。
	同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子之前调用。
	值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

注意：**Vue.extend()** 也使用同样的策略进行合并。

怎么实现既有mixin的功能又不会有合并问题？？
```

##### inheritAttrs

```js
	当一个组件设置了inheritAttrs: false后（默认为true），那么该组件的非props属性(即未被props接收的属性)将不会在组件根节点上生成html属性。与$attrs、$listeners结合使用。
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps13.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps14.jpg) 

##### provide/inject依赖注入

```
子组件获取父组件方法的配置
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps15.jpg) 

```
rovide / inject API 主要解决了跨级组件间的通信问题，不过它的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提供与依赖注入的关系。
```

```js
// 使用
// 传递属性
// A.vue
export default {
  provide: { name: '浪里行舟' }
}
// B.vue
export default {
  inject: ['name'],
  mounted () { console.log(this.name);  // 浪里行舟 }
}

// 传递方法
父组件：provide提供父组件的方法ff   provide:() => {ff:this.ff} （与data同级）
provide 必须是一个对象或者返回对象的函数

子组件：子组件中使用inject接受 inject:['ff'] / inject:{ ft:ff }
inject 必须是一个 字符串数组 或 对象

vm使用 provide 那么所有的vc都可以使用 inject 接收数据

```

#### 1. 组件

##### (1) 组件的命名规则

```
1.驼峰命名 myName 

2.烤串命名 my-name 

3.贪吃蛇 my_name 
```

##### (2) 全局定义组件（不推荐使用，不管你用不用，组件都会挂在Vue上）

```
 Vue.component(组件名，{ 配置项 } ) 
    组件名：1.组件名的首字母一定要大写 

    配置项：1.组件的配置项options与new Vue() el 换成 template（只能有一个顶级标签） 
		   2. data是一个函数，返回值一定是对象 
```



**全局定义组件**

```js
// 简单模板
Vue.component('Myhead',{
    template:'<h3>hello{{mess}}<h3>',//组件的模板只能有一个顶级标签
    data(){
      return {
        mess:'seec3'
      }
    }
  })

// 复杂模板
// 注意：当template中的字符串 太长时，我们可以这样

<!-- 定义模板 -->
<script type="template/text" id="myhead">
  <div>
    <h3>hello {{mess}}</h3>
  </div>
</script>
Vue.component('Myhead',{
    template:'#myhead',//组件的模板只能有一个顶级标签
    data(){
      return {
        mess:'seec3'
      }
    }
})
```

##### (3) 局部定义组件 （用时挂载，挂载在谁身上，就在谁的模板中使用）

```
1.定义组件，
	.vue文件定义组件，使用import引入
2.挂载组件
 	在components中注册，在模板中使用
```

**局部定义组件** 

```js
let Myson={
    template:'<h3>hello{{mess}}</h3>',//当template 太多时，进行分离，用上面的方法
    data(){
      return {
        mess:'seec3' //自己的数据只能用在自己的模板中
      }
    }
  }
  const app = new Vue({
    el:'#app',
    components:{//注入组件
      Myson
    }
  })
```

**区别：** 

```
1.全局定义的组件，在Vue中的任意地方使用，而，局部组件是在那个组件注入，就在那个组件的模板中使用  

2.从内存上，不管你使不使用全局都占内存，局部可以按需使用 

Ps:	
	1.组件模板中，只能有一个顶级标签 
	2.组件中的data必须是一个函数，这个函数必须返回对象 
```

##### (4) 异步组件

**全局定义** 

```js
Vue.component('async-webpack-example', function (resolve,reject) {
    resolve({
         template: '<div>I am async!</div>',
         data(){ return {} }
    })
})
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
Vue.component(
  'async-webpack-example',
  // 这个动态导入会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

**局部定义** 

```js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})

```



##### (5) **处理加载状态组件**

```js
const AsyncComponent = () => ({
  // 需要加载的组件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 如果提供了超时时间且组件加载也超时了，
  // 则使用加载失败时使用的组件。默认值是：`Infinity`
  timeout: 3000
})

// 注入
components:{ AsyncComponent }

// 使用
<AsyncComponent />
```

##### (6) 递归组件

```js
递归：函数内部调用函数本身。

递归组件：在组件的模板中调用自身。
    不能直接使用组件名，需要使用name属性定义的名称调用（name的值与组件名一致）
    必须有条件终止组件调用，否者报  Maximum call stack size exceeded 栈溢出


<div id="app">
    <Son :list="arrs"></Son>
</div>

```

```js
//模板
<script id="son" type="template/text">
  <div>
    <ul>
      <li v-for="(item,index) in list">
        {{item.name}}--{{item.text}}
        <div v-if="item.children">
          <Son :list="item.children"></Son>
        </div>
      </li>      
    </ul>
  </div>
</script>
new Vue({
    el:'#app',
    data:{
      arrs:[
        {
          name:'zs',
          text:'哈哈哈',
          children:[
            {name:'zxs',text:'heihei'},
            {name:'zxw',text:'heihei'}
          ]
        },
        {
          name:'ls',
          text:'呵呵呵',
          children:[
            {name:'lxs',text:'heihei'},
            {name:'lxw',text:'heihei'}
          ]
        }
      ]
    },
    components:{
      Son
    }
  })
  let Son = {
    name:'Son',
    template:'#son',
    props:['list'],
    data(){
      return{}
    }
  }
```



##### (7) 内置组件(系统组件)

###### ①component动态组件 

```js
父组件模板中  使用component 系统组件 利用is 属性动态绑定组件值。

component 系统组件本身不具备缓存功能，如果需要缓存动态组件的状态，利用keep-alive  保持这些组件的状态，以避免反复重新渲染导致的性能问题 

<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component :is="currentTabComponent"></component>
</keep-alive>

```

```js
<div id="app">
    <component :is="com"></component>  <!-- com的值变换，这显示的组件就会变换-->
  </div>

  new Vue({
    el:'#app',
    data:{
      com:'Bcom'
    },
    components:{//组件
      Acom, Bcom,Ccom
    }
  })
```

```js
<div id="app">
    <ul class="list">
      <li @click="fn(0)">a组件</li>
      <li @click="fn(1)">b组件</li>
      <li @click="fn(2)">c组件</li>
    </ul>
    <component :is="com"></component>
  </div>

  new Vue({
    el:'#app',
    data:{
      com:'Bcom'
    },
    components:{Acom, Bcom,Ccom},
    methods:{
      fn(v){
        if(v===0)return this.com = 'Acom';
        if(v===1)return this.com = 'Bcom';
        if(v===2)return this.com = 'Ccom';
      }
    }
  })

```

###### ②keep-alive

```
include : 缓存的组件,include=”New” include=”[‘New’,’person’]”
exclude : 值为 不进行缓存的组件，其他组件都缓存

生命周期：
activated 类型：Function
    详细：
     被 keep-alive 缓存的组件激活时调用。
     该钩子在服务器端渲染期间不被调用。

deactivated 类型：Function
    详细：
     被 keep-alive 缓存的组件失活时调用。
     该钩子在服务器端渲染期间不被调用。
```

**缓存非路由组件**

```js
<keep-alive>
	<component is=""></component>
</keep-alive>
```

**缓存路由组件**

```js
<keep-alive>
	<router-view>缓存所有路由组件</router-view>
</keep-alive>

<keep-alive include="" exclude="">
	<router-view>缓存特定路由组件</router-view>
</keep-alive>
```

###### ③transition动画

```
动画分为前半场和后半场。设置动画需要添加类名

v-enter表示进入时的动画（初始样式），
v-leave-to表示结束时的动画（结束样式类名）。

v-enter-to表示前半场动画的结束状态，
v-leave表示后半场动画起始样式

v-enter-active
v-leave-active表示动画的时间段

enter -> enter-active ->  enter-to 前半场
leave -> -> leave-active -> leave-to 后半场

```

![1709175418718](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175418718.png)

**1)** **设置动画**

```js
<button @click="show = !show">
    Toggle render
</button>

<transition>  
    <p v-if="show">hello</p>
</transition>

transition 有 name 属性 与动画 类名 有关：
	1.name使用默认 值  动画类名  就以 v- 开头 如：v-enter v-enter-to ...
	2.name使用我们定义的值xx动画类名就以 xx- 开头 如:fade-enter fade-leave... name="fade"

transition 有 tag 属性 解析之后 把标签转为 tag的值
例如：tag="ul" 转为 ul 元素

```

**2)** **自定义过渡的类名**

```
我们可以通过以下 attribute 来自定义过渡类名：

enter-class
enter-active-class
enter-to-class (2.1.8+)
leave-class
leave-active-class
leave-to-class (2.1.8+)
```

**3)** **JavaScript钩子**

```js
在过渡钩子函数中使用 JavaScript 直接操作 DOM

可以配合使用第三方 JavaScript 动画库，如 Velocity.js

<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
  <!-- ... -->
</transition>
```

```
这些钩子函数 的参数 有 el 和 done

el: transition元素
done：回调函数

 当只用 JavaScript 过渡的时候，在 enter 和 leave 中必须使用 done 进行回调。否则，它们将被同步调用，过渡会立即完成 

  推荐对于仅使用 JavaScript 过渡的元素添加 v-bind:css="false"，Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响。 
```

**4)** **过渡模式**

```js
同时生效的进入和离开的过渡不能满足所有要求，所以 Vue 提供了过渡模式

in-out：新元素先进行过渡，完成之后当前元素过渡离开。
out-in：当前元素先进行过渡，完成之后新元素过渡进入。

用 out-in 重写之前的开关按钮过渡：
<transition name="fade" mode="out-in">
  <!-- ... the buttons ... -->
</transition>
```

**5)** **过渡持续时间**

```js
<transition :duration="1000">...</transition>

 定制进入和移出的持续时间： 
 <transition :duration="{ enter: 500, leave: 800 }">...</transition>
```

**6)** **半场动画**

```
我们可能不需要完整的动画，只需要半场动画，这时，则需要通过动画的钩子函数实现半场动画。

其中:css=false表示Vue 会跳过 CSS 的检测。这也可以避免过渡过程中 CSS 的影响
```

###### ①transition-group动画列表

```
如果使用v-for列表渲染的内容想要添加动画，则需要使用动画列表**transition-group**组件 
```

![1709175439486](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175439486.png)

```
**设置点击子项删除当前项时，如果不特别设置，则无动画效果**。可以通过v-move类实现移动时的动画过渡效果。 

设置移动时的动画过渡时，需要给v-leave-active添加定位属性。
```

![1709175454429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175454429.png)

###### ①slot  插槽

```
	原本组件标签内的内容是不被识别的，但是在组件模板中添加 slot 那么 组件标签内的内容就会展示在页面中
    有了slot 组件标签中的 文本，标签 以及其他组件标签 都可以被识别
    后备内容：我们可以在 slot 中预先添加内容，作为它的默认内容，当组件标签的内容变化时，就会覆盖原先的内容
```

**1)** **普通（默认）插槽**

```js
// name默认为 default

// 子组件children：
<template>
<div class="about">
<h1>This is an Children page</h1>
<!-- 定义一个默认插槽 -->
<slot></slot>
</div>
</template>

// 父组件：
<template>
  <div class="about">
    <h1>This is an Parent page</h1>
    <children>
      <!-- 一个p标签的dom结构 -->
      <p>子组件标签之间</p>
    </children>
  </div>
</template>
```

**2)** **具名插槽**

```
	具名插槽： <slot> 元素有一个特殊的 attribute：name。这个 attribute 可以用来定义额外的插槽 ， 一个不带 name 的 <slot> 出口会带有隐含的名字“default”。 

    在向具名插槽提供内容的时候，我们可以在一个 <template> 元素上使用 v-slot 指令，并以 v-slot 的参数的形式提供其名称
```

```js
// 语法： <slot name="名称"></slot>
// 子组件 children：
<template>
  <div class="about">
    <h1>This is an Children page</h1>
    <!-- 给插槽加了个name属性，就是所谓的具名插槽了 -->
    <slot name="one"></slot>
    <slot name="two"></slot>
  </div>
</template>

// 父组件
<template>
  <div class="about">
    <h1>This is an Parent page</h1>
    <children>
      <template v-slot:one>
        <p>one插槽</p>
      </template>
      <template v-slot:two>
        two插槽
      </template>
    </children>
  </div>
</template>
```

```js
 // 子组件
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot>默认插槽</slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>

// 父组件模板
// 任何没有被包裹在带有 v-slot 的 <template> 中的内容都会被视为默认插槽的内容。 

<base-layout>
   	<template v-slot:header>
     <h1>Here might be a page title</h1>
	</template>   

	<p>A for the main content.</p>
   	<p>And another one.</p>
   	<template v-slot:footer>
     <p>Here’s some contact info</p>
   	</template>
 </base-layout>    

```

```js
跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #。
例如 v-slot:header 可以被重写为 #header

 普通插槽 #default   必须始终以明确的插槽名
<children>
        <template #default="slotProps">
          <h2>{{slotProps.obj.name}}</h2>
        </template>
</children>
```

**3)** **作用域插槽**

```js
<slot name="b" :data="user"></slot>

<template v-slot:b="data">
    <h4>{{ data.user.a }}</h4>
</template>

v-slot:b ==> #b
------------------------------------------

<slot :data="user"></slot>

<template v-slot:default="data">
    <h4>{{ data.user.a }}</h4>
</template>

```

**解构插槽** 

```js
<slot name="b" :data="user"></slot>

<template v-slot:b="{user}">
    <h4>{{ user.a }}</h4>
</template>
```

#### 1. 组件通信

##### (1) 父子间通信

###### ①　**父传子**

```
在父组件的模板中，把数据绑定在子组件标签上
子组件使用 props 字段来接受传递的值 （注意 props 的写法上）
在子组件模板中可以直接使用
        有两种情况：
        1. 父组件的模板 中<B  :属性="值"></B>  这个"值"是父组件的data的数据
         子组件的配置项  props:['属性'] 
         
        2.父组件的模板 中 <B 属性="值"></B>   这个"值" 只是字符串
        子组件的配置项  props:['属性']  
```

###### **props接收字段**

​     

```js
 props值可以是数组，也可以是对象
         a.数组：props:['属性']
         b.对象：props:{name:String,age:Number}  接收的同时对数据进行类型限制  
                props:{ 接收的同时对数据进行：类型限制、默认值指定、必要性的限制
                    name:{ 
                    	type:String,
                    	required:true,
                    	default:()=>{return [] }
                    },//required 是不是必要的
                    age:{ 
                    	type:Number,
                    	default:18 
                    }//default 默认值
                }
          注意：props是只读属性，想修改，复制props到data中，修改data的数据

              data与props 有同名属性是，props优先显示，并报错
```

> **不能直接修改 通过props 传递过来的值  会  报错的** 

![1709175484988](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175484988.png)

```js
<v-tabber v-model=”activeName” @change=”onChange”></v-tabber>
new Vue({
    props:[‘activename’],
    data(){
        return {
        	activeName:this.activename
        }
    }
})
```

###### ①　**子传父** 

```
自定义事件 this.$emit(eventname,args)   将自定事件推到事件池

        eventname 自定义事件的事件名 
        args是给执行 自定义事件的事件函数的参数

        那个组件产生的自定义事件，那就把自定义事件绑定到那个组件上（父组件的子组件标签上）
        1.在子组件中使用 this.$emit() 自定义事件 
        2.在父组件中通过 v-on 绑定这个自定义事件（谁定义的自定义事件绑定在谁身上）
        3.这个绑定的事件的事件函数就是关于处理这些数据的业务

事件池 ：程序中的事件都存储在事件池中，当事件触发时，会到事件池中查找
```

 

```js
// 子组件  
<div  @click='fnSon' ></div>
 
methods：{
	fnSon(){
		this.$emit('zdy',数据)
	}
}

// 父组件   
<div>
 <子组件  @zdy='fnP'></子组件>
</div>

methods:{
	fnp(value){
 		//value 子组件的数据 
 	}
 }
```

###### ②　父获子

```
1.this.$childrens[0]  不推荐 

2.this.$refs   
            a.给子组件标签上添加一个 ref 属性，且设置唯一的值id (在父组件的模板中)
            b.在父组件中使用this.$refs.id 就可以拿到数据了
```

###### ③　子获父

```
this.$parent
```

##### (1) **Vuex**

```
Vuex中有介绍
```

##### (2) provide/inject

```
前边的options中有介绍
```

##### (3) $attrs/$listeners

```
多级组件嵌套需要传递数据时，通常使用的方法是通过vuex。但如果仅仅是传递数据，而不做中间处理，使用 vuex 处理，未免有点大材小用。为此Vue2.4 版本提供了另一种方法----$attrs/$listeners
```

```
	$attrs：包含了父作用域中不被 prop 所识别 (且获取) 的特性绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件。通常配合 inheritAttrs 选项一起使用。
	$listeners：包含了父作用域中的 (不含 .native 修饰器的) v-on 事件监听器。它可以通过 v-on="$listeners" 传入内部组件。
```

**示例：**

```vue
// A组件（App.vue）
<template>
  <div id="app">
    <!-- 此处监听了两个事件，可以在B组件或者C组件中直接触发 -->
    <child1 :pchild1="child1" :pchild2="child2" :pchild3="child3" @method1="onMethod1" @method2="onMethod2"></child1>
  </div>
</template>
<script>
import Child1 from "./Child1.vue";
export default {
  data() {
    return {
      child1:'1',
      child2: 2,
      child3:{ name:'child3' }
    };
  },
  components: { Child1 },
  methods: {
    onMethod1(msg1) {
      console.log(`${msg1} running`);
    },
    onMethod2(msg2) {
      console.log(`${msg2} running`);
    },
  },
};
</script>
```

```vue
// B组件（Child1.vue）
<template>
  <div class="child-1">
    <h2>in child1</h2>
    <p>props: {{ pchild1 }}</p>
    <p>$attrs: {{ $attrs }}</p>
    <hr/>
    <!-- 通过 v-bind 绑定$attrs属性，C组件可以直接获取到A组件中传递下来的props（除了B组件中props声明的） -->
    <!-- C组件中能直接触发test的原因在于B组件调用C组件时 使用v-on 绑定了$listeners 属性 -->
    <child2 v-bind="$attrs" v-on="$listeners"></child2>
  </div>
</template>

<script>
import Child2 from "./Child2.vue";
export default {
  data() {
    return {
      child1:'child1'  
    };
  },
  components: { Child2 },
  props: {
    pchild1:{
      type:String
    }
  },  
  inheritAttrs: false,
  mounted() {
    this.$emit("method1",this.child1);
  },
};
</script>
```

```vue
// C 组件 (Child2.vue)
<template>
  <div class="child-2">
    <h2>in child2:</h2>
    <p>props: {{ pChild2 }}</p>
    <p>$attrs: {{ $attrs }}</p>
    <p>pchild3Name: {{ $attrs.pchild3.name }}</p>
    <hr/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      child2:'child2'
    };
  },
  props: {
    pChild2:{
      type:String,
    }
  },
  inheritAttrs: false,
  mounted() {
    this.$emit("method2",this.child2);
  },
};
</script>
```

###### ①　**$attrs的作用和使用方法**

```
$attrs是vue版本2.40以上新增的属性；

使用场景：
　vue项目里面，大家肯定遇到过组件之间的传值问题，父传子、子传父、非父子之间传值等等；
　假如说我们碰到一个多层组件之间的传值，可以用$emit、$on可以解决这个问题，但是操作太过于繁琐了，或者使用vuex也可以去实现，但是有点大材小用杀鸡用了宰牛刀。
　然后 $attrs 就是用来解决这个问题的!
　
官方解释：
$attr：包含了父作用域中不作为 prop 被识别 (且获取) 的 attribute 绑定 (class 和 style 除外)。当一个组件没有声明任何 prop 时，
这里会包含所有父作用域的绑定 (class 和 style 除外)，并且可以通过 v-bind="$attrs" 传入内部组件——在创建高级别的组件时非常有用。

如果给组件传递的数据，组件不使用props接收，那么这些数据将作为组件的HTML元素的特性，这些特性绑定在组件的HTML根元素上
inheritAttrs: false的含义是不希望本组件的根元素继承父组件的attribute，同时父组件传过来的属性（没有被子组件的props接收的属性），也不会显示在子组件的dom元素上，但是在组件里可以通过其$attrs可以获取到没有使用的注册属性, ``inheritAttrs: false`是不会影响 style 和 class 的绑定
```

```
下面看实际用例（父组件的列表行数据传递给孙子组件展示）：

总结：当我们涉及到多层传参的时候，父组件传出去的值，中间的组件不用通过props接收，但是要在子组件（中间组件）身上通过v-bind="$attrs",这样最下层的组件才能拿到最外层组件传过
父组件（Father.vue），给子组件关联数据，子组件如果不用props接收，那么这些数据就作为普通的HTML特性应用在子组件的根元素上
```

```vue
<template>
  <div>
    <el-table :data='list'>
      <el-table-column
        prop="name"
        label="姓名"
      ></el-table-column>
      <el-table-column
        prop="study"
        label="学习科目"
      ></el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button @click='transmitClick(scope.row)'>传递</el-button>
        </template>
      </el-table-column>
    </el-table>
    
    <!-- 儿子组件 -->
    <ChildView
      :is-show="isOpen"
      :row="row"
    >
    </ChildView>
  </div>
</template>

<script>
import ChildView from './Child.vue'
export default {
  components: { ChildView },
  data() {
    return {
      isOpen: false,
      row: {},
      list: [
        { name: '王丽', study: 'Java' },
        { name: '李克', study: 'Python' }
      ]
    }
  },
  methods: {
    // 传递事件
    transmitClick(row) {
      this.isOpen = true;
      this.row = row
    }
  }
}
</script>
```

```vue
// 儿子组件（Child.vue），中间层，作为父组件和孙子组件的传递中介，在儿子组件中给孙子组件添加v-bind="$attrs"，这样孙子组件才能接收到数据
<template>
  <div class='child-view'>
    <p>儿子组件</p>
    <GrandChild v-bind="$attrs"></GrandChild>
  </div>
</template>

<script>
import GrandChild from './GrandChild.vue'
export default {
  // 继承所有父组件的内容
  inheritAttrs: true,
  components: { GrandChild },
  data() {
    return {
    }
  }
}
</script>

<style lang="stylus">
.child-view {
  margin: 20px
  border: 2px solid red
  padding: 20px
}
</style>
```

```vue
// 孙子组件（GrandChild.vue），在孙子组件中一定要使用props接收从父组件传递过来的数据

<template>
  <div class='grand-child-view'>
    <p>孙子组件</p>
    <p>传给孙子组件的数据：{{row.name}} {{row.name !== undefined? '学习' : ''}} {{row.study}}</p>
  </div>
</template>

<script>
export default {
  // 不想继承所有父组件的内容,同时也不在组件根元素dom上显示属性
  inheritAttrs: false,
  // 在本组件中需要接收从父组件传递过来的数据，注意props里的参数名称不能改变，必须和父组件传递过来的是一样的
  props: {
    isShow: {
      type: Boolean,
      dedault: false
    },
    row: {
      type: Object,
      dedault: () => { }
    }
  }
}
</script>
```

##### (4) **作用域插槽**

```
前边的插槽中有介绍
```

##### (5) **全局事件总线**

```js
main.js
	beforeCreate(){ Vue.prototype.$bus = this }

提供数据的组件
	fn(){ this.$bus.$emit('eventname',data) }

接收数据的组件
	this.$bus.$on('eventname',value=> console.log(value))

销毁事件
	beforeDestroy(){ this.$bus.$off('eventname') }
```

#### 1. Vue Router路由

##### (1) **安装路由**

```
vue中不允许使用超链接 a  使用 路由 
 将我们的组件映射到路由上  
npm i vue-router@4 -S  用在vue3.x 

npm i vue-router@3 代表安装vue-router 3版本的 用在vue 2.x 

npm i vue-router@3.5.1 安装3.5.1这个版本
```

##### (2) **手动配置路由**

```js
1.定 ：定义组件

    import MyHome from './components/MyHome.vue'

2.配 ：配置路由规则 映射

    let routes = [{
        path:'/home',
        component:MyHome
    }]

3.实 ：实例化路由

    import VueRouter from 'vue-router'
    Vue.use(VueRouter)
    let router = new VueRouter({
        routes
    })

4.挂 ：挂载路由

new Vue({
 	router,
	render: (h) => h(App),
}).$mount("#app");

```

##### (3) **单独路由文件**

```js
// 当路由组件太多的话，就不适合放在 入口文件main.js中
// 在src/router/router.js 

import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)//安装一个依赖vue的插件
import MyHome from '../components/MyHome.vue'
import MyAbout from '../components/MyAbout.vue'

let routes = [
  {
    path:'/home',//路径 以当前项目为目录 / 代表根目录 home是path值，随便写
    component:MyHome
  },
  {
    path:'/about',
    component:MyAbout
  }
]

let router = new VueRouter({
  routes
})

export default router

// 在出口文件中 main.js

import router from "./router/router.js";

//挂载路由

new Vue({
  router,
  render: (h) => h(App),
}).$mount("#app");

```

##### (4) **路由独有的组件**

###### ①　**router-link**

```js
<router-link to="/home">home</router-link> 编译后会变成 a 标签，

属性 to 相当于 href，值是path等
属性 tag 的值 是 标签 如：tag='li' router-link编译后 成为 li标签

active-class：被激活的类名（注意权重）

<router-link to='/home' active-class='a'>home</router-link>

<router-link to='/about' active-class='a'/>about</router-link>

点击那个就会激活那个，样式就会被使用

```

###### ②　**router-view**

```js
<!-- 路由出口组件 --> 想将路由组件 放到那个组件中，那个这个组件就必须要有 router-view

<router-view><router-view>
```

##### (5) **路由Options**

###### ①　**Path：**

```js
{
    path: "/",
    name: "home",
    component: HomeView,
},
```

###### ②　**Name：**

```js
{
    path: "/",
    name: "home",
    component: HomeView,
},
```

###### ③　**Children：子路由**

```js
children:[
    {
    	path:"/home/son", 或者 path:”son” 会拼接父级path
    	//也可以写成 son 会和父路由拼接，但是绝对不能 写成/son 这表示从根目录下的 son但是son配置在home路由下；
    	也可以写 / 这样点击它就会跳转到根目录
    	...
    }
] 
```

###### ④　**Alias：别名**

```js
{
    path:'/about',
    alias:'/xiaotao',
    name:'about'
}

给 about 起了别名 xiaotao

当我访问 xiaotao 是 出现的都是  about
页面
```

###### ⑤　**Components：命名视图**

```js
页面要 多视图（多个 router-view 系统组件） 就使用 命名视图

如果使用了命名视图，在路由配置项中就不能使用 component 了，使用components

在路由配置项中 component 与components不能同时出现

    <router-view name="wang"></router-view>
    <router-view name="han"></router-view>
    <!-- router-view 都有name，如果没写，默认default，如果写了就会失去原有的功能，不会展示组件 -->

 解决办法：在路由配置中：

components:{ //name值作为键，组件名作为值 
    wang:MyHome,
    han:MyAbout
}

```

###### ⑥　**Component：与components不能共存**

```js
import MyHome form '@/view/MyHome.vue'  //@ 代表 /src

component:MyHome,// component、components 不能共存 

路由懒加载
component:()=>import(/*webpackChunkName:"ImportFuncDemo"*/,'../compontents/Login')}
```

###### ⑦　**Redirect：重定向**

```js
redirect:
   进入目标页面时，重定向到其他页面
值：
	字符串 "/user/zhangsan"
	对象 {name:"user",params:{name:'lisi'}}
	redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
```

###### ⑧　**Props：解构参数**

```js
有了 props:true  this.$routes.params.id就可以不用了

// 没有props:true
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const routes = [{ path: '/user/:id', component: User }]

// 使用props:true
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'

}

const routes = [{ path: '/user/:id', component: User, props: true }]

接收父组件传递的数据
解耦路由和组件的耦合度，（接收路由配置里的路由参数）
```

###### ⑨　**Mode：路由的工作模式**

```js
// 默认hash、history
const router = new VueRouter({
	mode: 'history',
	routes: [  { path:’*’, component: NotFoundComponent }  ]
})
```

**Hash模式**

```js
即地址栏 URL 中的 # 符号,这个#就是hash符号，中文名哈希符或锚点
比如这个 URL：http://www.baidu.com/#/home，hash 的值为 #/home
它的特点在于：hash 虽然出现在 URL 中，但不会被包括在 HTTP 请求中，对后端完全没有影响，因此改变 hash 不会重新加载页面。
路由的哈希模式其实是利用了window.onhashchange事件，也就是说你的url中的哈希值（#后面的值）如果有变化，就会自动调用hashchange的监听事件，在hashchange的监听事件内可以得到改变后的url，这样能够找到对应页面进行加载
hashchange事件有两个属性：
newURL: string类型，当前页面新的url
oldURL: string类型，当前页面旧的url

window.addEventListener('hashchange', function (e) {
  console.log(e.newURL,e.oldURL)
  var str = e.newURL.split('#')[1]
  document.getElementById('num').innerHTML = str.split('=')[1]
})
```

**history模式**

```
HTML5 History Interface 中新增的两个神器 pushState() 和 replaceState() 方法（需要特定浏览器支持），用来完成 URL 跳转而无须重新加载页面，不过这种模式还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，就需要前端自己配置404页面。
```

![1709175529957](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175529957.png)

```
pushState() 和 replaceState() 这两个神器的作用就是可以将url替换并且不刷新页面，好比挂羊头卖狗肉，http并没有去请求服务器该路径下的资源，一旦刷新就会暴露这个实际不存在的“羊头”，显示404（因为浏览器一旦刷新，就是去真正请求服务器资源）
那么如何去解决history模式下刷新报404的弊端呢，这就需要服务器端做点手脚，将不存在的路径请求重定向到入口文件（index.html），前后端联手，齐心协力做好“挂羊头卖狗肉”的完美特效
```

**解决mode为history时出现的404问题**

```js
// 1)方法一
// 在nginx的配置文件中修改
location /{
    root   /data/nginx/html;
    index  index.html index.htm;
    if (!-e $request_filename) {
        rewrite ^/(.*) /index.html last;
        break;
    }
}
// 2)方法二
	Node中间件：connect-history-api-fallback
// 3)方法三
// vue.js官方教程里提到的https://router.vuejs.org/zh/g...
server {
      listen       8081;#默认端口是80，如果端口没被占用可以不用修改
      server_name  myapp.com;
      root        D:/vue/my_app/dist;#vue项目的打包后的dist
      location / {
          try_files $uri $uri/ @router;#需要指向下面的@router否则会出现vue的路由在nginx中刷新出现404
          index  index.html index.htm;
      }
      #对应上面的@router，主要原因是路由的路径资源并不是一个真实的路径，所以无法找到具体的文件
      #因此需要rewrite到index.html中，然后交给路由在处理请求资源
      location @router {
          rewrite ^.*$ /index.html last;
      }
      #.......其他部分省略
}
```

###### ①　**路由守卫/导航守卫**

**全局守卫：** 

```js
// 任何一个路由跳转都会触发

router.beforeEach((to,from,next)=>{
    //禁止在里边写for循环
    to:当前的路由
    from：上一个路由
    next: 中间件函数，代表下一步
})

afterEach() 全局后置钩子 没有next
```

**单个路由守卫:**

```js
某个路由跳转会触发，其他不会

    在配置路由中添加 ：
    beforeEnter:(to,from,next)=>{}
```

**组件内部守卫：** 

```js
beforeRouteEnter 进入前，不能获取组件this，在守卫执行前，组件实例还没被创建
beforeRouteUpdate 路由变化时
beforeRouteLeave 离开后，通常用来禁止用户在还未保存修改前，突然离开，该导航可以用next(false)来取消

// 参数：

to:进入的目标
from：离开的路由
next：
    next()是放行，next()中有参数，则表示中断当前导航，执行新的导航
    next(false) 会重置到from 路由对应的地址
    next('/') next({path:'/'}) 允许设置 replace:true name:'home'之类的选项和router.push中的选项

next(error) 参数是 Error 实例，则导航被终止，且该错误会被传递给router.onError()注册过的回调
```

##### (1) **路由懒加载**

```js
1.Vue异步组件(异步加载)
	component:resolve => require(['@/components/List'],resolve)}
2.推荐方式-ES6的import()
	component:()=>import(/*webpackChunkName:"ImportFuncDemo"*/,'../compontents/Login')}
3.webpack提供的require.ensure()实现懒加载
	component:r=>require.ensure([],()=>r(require('@/components/list')),'listDemo')},
注：r就是resolve
第一个参数是数组，表明第二个参数里需要依赖的模块，这些会提前加载。
第二个是回调函数,在这个回调函数里面require的文件会被单独打包成一个chunk,不会和主文件打包在一起，这样就生成了两个chunk,第一次加载时只加载主文件。
第三个参数是错误回调。 
第四个参数是单独打包的chunk的文件名 
```

##### (2) **动态路由**

```js
在路由配置项中 {path:'/user/:name'} 设置形参

  	<router-link to="/user/zhangsan">user/zhangsan</router-link> 设置实参

  	组件中通过 this.$route.params.name取值  这个name是你设形参时定义好的
```

##### (3) **嵌套路由**

```js
{
    path: "/about",
    component: MyAbout,
    children:[
      {path:'/about/child',component:AboutChild},
      {path:'/about/son',component:AboutSon},
    ]
  },
```

##### (4) **路由跳转**

![1709175556002](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709175556002.png)

###### ①　**声明式跳转**

```
通过 router-link 的to 属性 结合 router-view
```

###### ②　**编程式跳转**

```
Methods中调用this.$router.push()
```

###### ③　**路径参数规则**

```js
由于属性 to 与 router.push 接受的对象种类相同，所以两者的规则完全相同 

// 字符串路径
router.push('/users/eduardo')

// 带有路径的对象  注意：配置路由时 要配置形参  path: '/users/:name'
router.push({ path: '/users/eduardo' })

// 命名的路由，并加上参数，让路由建立 url 注意：params 只能和name 一起使用
router.push({ name: 'user', params: { username: 'eduardo' } })

// 带查询参数，结果是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// 带 hash，结果是 /about#team
router.push({ path: '/about', hash: '#team' })
```

##### (1) **路由替换**

<img src="file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps26.jpg" alt="img" style="zoom: 80%;" />

```
router.replace() 和 router.push() 参数一致
```

##### (2) **横跨历史**

```
向前移动一条记录：router.forward() router.go(1)

返回一条记录：router.back() router.go(-1)
```

##### (3) **addRoutes()** **动态生成路由**

```js
// 动态生成路由：

router.addRoutes([路由配置]) / this.$router.addRoutes([路由配置])

// 例子：

<button @click="addRoutes">addRoutes</button>

addRoutes(){
    this.$router.addRoutes([
        {
            path:'/',
            component:()=>import('./views/...')
        }
    ])
}
```

###### ①　**在addRoutes() 后第一次访问添加的路由会白屏**

```js
// 原因：

    因为刚刚addRoutes 就立即访问 被添加的路由，然而此时addRoutes没有执行结束，因为找不到刚刚添加的路由，导致白屏，因此需要重新访问一次才行

// 解决办法：

    使用 next({...to,replace:true}) 来确保addRoutes时动态添加的路由已经被完全加载上去，next({...to,replace:true}) replace:true 只是一个设置信息，告诉vue本次操作后，不能通过浏览器后退按钮，返回上一个路由

addRoutes(){
    this.$router.addRoutes([
        {
            path:'/',
            component:()=>import('./views/...')
        }
    ])
    next({...to,replace:true})
}
```

#### 1. **Vuex**

##### (1) **Vuex**

```
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库;采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的
```

##### (2) **安装**

```
vuex 安装下载

vue 2 安装 vuex 3 ; npm i vuex@3
vue 3 安装 vuex 4 npm i vuex@next


state，驱动应用的数据源；

view，视图；

actions，用户行为

注意：
1.mutation 是唯一 修改state 方法的地方
2.action 里面可以写 异步代码 mutation 不行

没有异步 任务时， 可以直接 this.$store.commit() 提交 到mutation
```

##### (3) **使用**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps29.jpg) 

###### ①　**State**

```
State存储数据
new Vuex.Store({
    state：{
    	count:[1,2]
    },
    getters:{}, 
    mutations:{}, 
    actions:{}, 
    modules:{}
})
组件中使用：this.$store.state.count
```

###### ②　**Getters**

```js
Getters与computed弄能一致，是vuex的计算属性，对state中的数据进行计算
Getters:{
	filterCount(state){ return state.count.filter( (item,index)=>item>1 ) }
}
组件中使用：this.$store.getters.filterCount
柯里化
Getters:{
	getCount=>state=>index=>state.count[index]
	// getCount(state){ return function(index){return state.count[index } }
}
组件中使用：this.$store.getters.getCount[0]
```

###### ③　**Mutations**

```
mutation 是唯一 修改state 的地方
mutations:{
    setCount(state,payload){
    	State.count.push( payload )
    }
}
触发mutations中的setCount。需要在actions的方法中 使用context.commit( ‘setCount’ , payload ) 
或者 在组件中 this.$store.commit( ‘setCount’ , payload )  
有异步操作必须在actions中完成  
```

###### ④　**Actions**

```js
Actions 可以执行异步操作 commit 触发mutations对应的方法
actions:{  //context 上下文对象，表示store
    getCountIndex(context,payload){
        setTimeout(()=>{
        	context.commit( ‘setCount’ , payload )
        },0)
    }
}
组件中使用 this.$store.dispatch(‘getCountIndex’,payload) 触发actions中的getCountIndex 方法
```

###### ⑤　**Modules**

```js
modules 模块
import modA from ‘./modules/modA.js’
import modB from ‘./modules/modB.js’
new Vuex.Store({
    modules:{
        mB:modB
        modA, //对象的简写形式 modA:modA
        modC:{
            state:{ num:1 },
            mutations:{}
        }
    } 
}
在组件中使用：this.$store.state.modC.num  
this.$store.dispatch(‘setNamexx’,payload) 不需要写成 modC/setNamexx 命名空间中需要
```

##### (4) **vuex模块化+命名空间**

```js
new Vuex.Store({
    modules:{
        modB:{
            Namespaced:true,
            state:{ num:1 },
            mutations:{}
        }
    } 
}
组件中使用：this.$store.commit( ‘modB/setxxx’,payload )  有命名空间，必须写成路径形式
```

##### (5) **辅助函数**

###### ①　**mapState**

###### ②　**mapGetters**

```js
mapState、mapGetters 使用在组件的computed 中
computed:{  
    // 数组形式：
    ...mapState( [ ‘count’ , ’num’ ] ) 
    ...mapGetters( [ ‘docount’ ] )							  
    // 对象形式;相当于在组件中用别名
    ...mapState( { increat : ‘count’ , tegger : ’num’ } )
    ...mapGetters( { don : ‘docount’ } )
}
//  命名空间
computed:{  
    // 数组形式：
    ...mapState( ‘modC’ , [ ‘count’ , ’num’ ] ) 
    ...mapGetters( ‘modC’ , [ ‘docount’ ] )
    // 对象形式;相当于在组件中用别名	        
    ...mapState( ‘modC’ , { increat : ‘count’ , tegger : ’num’ } )		         
    ...mapGetters(‘modC’ , { don : ‘docount’ } )
}
```

###### ③　**mapMutations**

###### ④　**mapActions**

```js
mapMutations、mapActions使用在组件的methods中
computed:{  
    // 数组形式：
    ...mapMutations( [ ‘mut_add’ ] )
    ...mapActions( [ ‘act_add’] )
    // 对象形式;相当于在组件中用别名
    ...mapMutations( { increat : ‘mut_add’} )
    ...mapActions( { don : ‘act_add’ } )
}
//命名空间
computed:{  
    // 数组形式：
    ...mapMutations( ‘modC’ ,  [ ‘mut_add’ ] ) 
    ...mapActions( ‘modC’ , [ ‘act_add’ ] )
    // 对象形式;相当于在组件中用别名
    ...mapMutations( ‘modC’ , { increat : ‘mut_add’ } )
    ...mapActions(‘modC’ , { don : ‘act_add’} )
}
```

##### (6) **缺点：不会保存起来，刷新之后就回到了初始状态**

```
vuex 是 vue 的状态管理器，存储的数据是响应式的。但是并不会保存起来，刷新之后就回到了初始状态，具体做法应该在vuex里数据改变的时候把数据拷贝一份保存到localStorage里面，刷新之后，如果localStorage里有保存的数据，取出来再替换store里的state。
只使用localshorage 那么数据就不是响应式的了。
需要注意的是：由于vuex里，我们保存的状态，都是数组，而localStorage只支持字符串，所以需要用JSON转换。
```

#### 1. **全局方法**

##### (1) Object.defineProperty/proxy

```js
let  obj = { name:'张三' }

 Object.defineProperty(obj,'name',{

        get(){}

        set(value){}

}

let name = obj.name

obj.name = 'lisi'

obj.age = '12'

// 1.Object.defineProperty

        1.可以对读取、修改属性拦截，增加、删除不可以拦截 所以 就有Vue.set() Vue.delete

let o = { name:"lisi" }

let p = new Proxy(o,{
        get(){}
        set(){},
        deleteProperty(target,property){}
})

// 2.proxy

二者都是对对象操作的拦截，Object.defineProperty() 是vue2 实现响应式的原理，vue3是
proxy
二者结合实现的数据响应式，（基础数据类型Object.defineProperty() 引用数据类型proxy）

Object.defineProperty() 只能拦截对象的 读写、proxy满足数据的增删改查

Reflect是Object 的终极规范
```

```
缺点：
	1: 在对一些属性进行操作的时候,  使用这种方法无法进行拦截,  比如通过下表的方式修改数组或者修改数据或者给对象新增属性,  这些都不可以触发组建的重新渲染。 因为 Object.defineProperty 不能拦截到这些操作。更精确的来说，对于数组而言，大部分操作都是拦截不到的，只是 Vue 内部通过重写函数的方式解决了这个问题。
 
	2: 在 Vue3.0 中已经不使用这种方式了，而是通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用Proxy 的好处是它可以完美的监听到任何方式的数据改变，唯一的缺点是兼容性的问题，因为 Proxy 是 ES6 的语法。
	
缺点1：在对对象的属性进行操作的时候,  使用这种方法无法进行拦截；做到响应式。。。如修改value，删除属性等
解决办法：使用Vue.set()或 vm.$set()、Vue.delete()
缺点2：数组，部分操作都是拦截不到的，只是 Vue 内部通过重写函数的方式解决了这个问题
解决办法：Vue.set()或 vm.$set()、Vue.delete()，以及重写函数：splice、shift、unshift、severse...

```

##### (2) **Vue.set** 、Vue.delete

```
因为vue2.0 的响应式 采用 Object.defineProperty() 实现的  ，但是，对 对象属性的添加和

删除做不到 响应式。就有了Vue.set Vue.delete

而 vue3 用的是 proxy 
```

##### (3) Vue.extend(options)

```js
Vue.extend(options) ===new Vue()
使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。
data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数
// 创建构造器
var Profile = Vue.extend({
  		template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  		data: function () {
    		return {
      		firstName: 'Walter',
      		lastName: 'White',
      		alias: 'Heisenberg'
    		  }
  	   }
     })
	// 创建 Profile 实例，并挂载到一个元素上。
	new Profile().$mount('#mount-point')
```

##### (4) **Vue.directive** **/** **directives** **自定义指令**

```js
自定义指令
 	用于前端埋点
钩子函数：
    inserted  插入dom
    bind 
    update 
    componentUpdated
    unbind

Vue.directive("focus", {
  inserted(el,binding){
    console.log(binding);
    //元素绑定事件
    el.focus()
  }
})

<input type="text"  v-focus  placeholder="自动获取焦点”>

// 或
 directives:{
    'big-font':{
        inserted(el,binding){}
    }
}

使用：v-big-font

<input type="text"  v-focus  placeholder="自动获取焦点”>

```

##### (5) **Vue.filter** **/** **filters 过滤**

```js
Vue.filter("touppercase",function(value){
        return value.toUpperCase()
})

<div>{{mess | touppercase}}</div>
```

##### (6) **Vue.mixin mixins混入** **以及选项合并的问题**

```js
 来分发 Vue 组件中的可复用功能 

let mymixin = {
	data()=>{ name:’a’ }
    methods:{
        fn(){ console.log('++') },
        ff(){ console.log('==') }
    }
}

export default mymixin

import mymixin from './xxx'

export default {
    name:"componentname",
    mixins:[mymixin],
    methods:{
        fn1(){
            this.fn()
        }
    }
}
// 注意：最好不要放到 new Vue({})中，容易造成 栈溢出；那个组件使用，在引入
```

```
mixin选项合并
情况一：如果是data函数的返回值对象
	返回值对象默认情况下会进行合并（不同key）；
	如果data返回值对象的属性（key）发生了冲突，那么会保留组件自身的数据

情况二：如果是生命周期钩子函数
	生命周期的钩子函数会被合并到数组中，都会被调用（不同名的钩子）；
	mixin中的生命周期钩子函数会比组件中的生命周期钩子函数先执行(全局mixin先于局部mixin,局部mixin先于组件)；

情况三：值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象。
	比如都有methods选项，并且都定义了方法，那么它们都会生效（不同名的key）；
	但是如果对象的key相同，那么会取组件对象的键值对；

Vue.extend() 也使用同样的策略进行合并。
```

##### (7) **Vue.nextTick** / vm.$nextTick

```js
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})

// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
Vue.nextTick()
  .then(function () {
    // DOM 更新了
  })
2.1.0 起新增：如果没有提供回调且在支持 Promise 的环境中，则返回一个 Promise。请注意 Vue 不自带 Promise 的 polyfill，所以如果你的目标浏览器不原生支持 Promise (IE：你们都看我干嘛)，你得自己提供 polyfill。
```

```
Vue的异步更新队列，$nextTick是用来知道什么时候DOM更新完成的。
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。 

解决的问题：
	在获取DOM 节点的时候，状态发生了改变，DOm 还没来得及更新，这些获取 DOM 的行为已经执行了，更新后的DOM是获取不到的

适用场景：
	第一种：有时需要根据数据动态的为页面某些dom元素添加事件，这就要求在dom元素渲染完毕时去设置，但是created与mounted函数执行时一般dom并没有渲染完毕，所以就会出现获取不到，添加不了事件的问题，这回就要用到nextTick处理
	第二种：在使用某个第三方插件时 ，希望在vue生成的某些dom动态发生变化时重新应用该插件，也会用到该方法，这时候就需要在 $nextTick 的回调函数中执行重新应用插件的方法，例如:应用滚动插件better-scroll时
	第三种：数据改变后获取焦点
```

```js
因为 $nextTick() 返回一个 Promise 对象，所以你可以使用新的 ES2017 async/await 语法完成相同的事情：
methods: {
  	  updateMessage: async function () {
        this.message = '已更新'
        console.log(this.$el.textContent) // => '未更新'
        await this.$nextTick()
        console.log(this.$el.textContent) // => '已更新'
     }
   }
```

```js
<div id="app">
    <div id="div" v-if="showDiv">这是一段文本</div>
    <button @click="getText">获取div内容</button>
</div>
<script>
var app = new Vue({
    el : "#app",
    data:{
        showDiv : false
        },
    methods:{
        getText:function(){
            this.showDiv = true;
            var text = document.getElementById('div').innnerHTML;
             console.log(text);
        }
    }
})
</script>

// 运行后在控制台会抛出一个错误：Cannot read property 'innnerHTML of null，意思就是获取不到div元素。这里就涉及Vue一个重要的概念：异步更新队列。
methods:{
    getText:function(){
        this.showDiv = true;
        this.$nextTick(function(){
            var text = document.getElementById('div').innnerHTML; 			
            console.log(text); 
        });
    }
}
```

```
nextTick方法的实现原理 
    vue用异步队列的方式来控制DOM更新和nextTick回调先后执行
    microtask因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
     因为兼容性问题，vue不得不做了microtask向macrotask的降级方案
```

```js
<p ref=”msgp”>{{ msg }}</p>
<p v-if=”msg1”>{{ msg1 }}</p>
<p v-if=”msg2”>{{ msg2 }}</p>
<p v-if=”msg3”>{{ msg3 }}</p>
<button @click="fn">click</button>

data()=>{
	msg:’hello’, msg1:’’, msg2:’’, msg3:’’
}
Method:{
    fn() {
    this.msg = ‘hi’
    this.msg1 = this.$refs.msgp.innerHTML
    this.$nextTick(()=>{
    	this.msg2 = this.$refs.msgp.innerHTML
    })
    this.msg3 = this.$refs.msgp.innerHTML
    }
}     
// 点击后
msg=’hi’ msg1=’hello’ msg2=’hi’ msg3=’hello’
```

```
异步更新队列
	Vue在观察到数据变化时并不是直接更新DOM，而是开启一个队列，并缓冲在同一个事件循环中发生的所以数据改变。在缓冲时会去除重复数据，从而避免不必要的计算和DOM操作。然后，在下一个事件循环tick中，Vue刷新队列并执行实际（已去重的）工作。所以如果你用一个for循环来动态改变数据100次，其实它只会应用最后一次改变，如果没有这种机制，DOM就要重绘100次，这固然是一个很大的开销。
	Vue会根据当前浏览器环境优先使用原生的Promise.then和MutationObserver，如果都不支持，就会采用setTimeout代替。
```

##### (8) **Vue.use**

```
Vue.use() 安装 依赖vue 的插件

针对这个插件的要求

1.如果这个插件是对象，这个插件必须有一个 install方法，同时这个方法的参数是 Vue

2.如果设个插件是函数，直接被当做 install 方法安装

注意：当我们使用 Vue.use()的时候 会自动执行 install方法 或者执行插件方法
```

```js
// 对象形式的插件
let myplugin = {
  install(Vue){
	//把插件都挂在Vue的原型上
    Vue.prototype.cuiplugin = function(){
      console.log('cui');
    }
  }
}

export default myplugin

//main.js

import myplugin from './plugins/myplugin'

Vue.use(myplugin)

const vm = new Vue({
  router,
  render: (h) => h(App),
  mounted () {
 	this.cuiplugin()  
  }
}).$mount("#app");

console.log(vm);

```

```js
// 函数形式的插件
let myplugin = function(Vue){
  Vue.prototype.cuifn = function(){
    console.log('====');
  }
}

export default myplugin

//main.js

import myplugin from './plugins/myplugin'

Vue.use(myplugin)

const vm = new Vue({
  router,
  render: (h) => h(App),
  mounted () {
    this.cuifn()
  }
}).$mount("#app");

console.log(vm);
```

###### ①　**插件--防抖--节流**

```js
let myplugin = {
    install(Vue){
        //防抖
        Vue.prototype.debouncefn = function(){
            let timer = null;
            return function(fn,wait){
                clearTimeout(timer);
                timer = setTimeout( ()=>{ fn() },wait)
            }
        }
    }
}

export default myplugin

//main.js

import myplugin from '../...'

Vue.use(myplugin)

// 调用

created(){ this.newdebouncefn = this.debouncefn(函数,3000) }

let myplugin = {
    install(Vue){
        //节流
        Vue.prototype.throttlefn = function(){
            let flag = true;
            return function(fn,wait){
                if(flag){
                    fn()
                    flag = false
                    setTimeout( ()=>{ falg = true } ,wait)
                }
            }
        }
    }
}

```

##### (9) **Vue.component**