### 第一部分：环境搭建
#### 一、使用 pnpm创建一个vite程序
```javascript
 $ pnpm create vite
```
#### 二、安装vite插件：@vitejs/plugin-vue-jsx
```javascript
$ pnpm i @vitejs/plugin-vue-jsx
```
#### 三、在vite.config.js加入jsx配置
```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vueJsx from '@vitejs/plugin-vue-jsx'
import vue from '@vitejs/plugin-vue'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(), vueJsx()],
})
```
至此，我们新建的这个vite项目已经全面支持jsx语法了。
### 第二部分、改造App.vue为App.jsx
jsx语法的组件命名需采用大驼峰式命名规则。

jsx文件内容的写法跟使用模板语法时，script标签内容中的写法一模一样，可以直接导出一个对象，也可以从vue中导入一个defineComponent函数，然后默认导出defineComponent函数，并传入一个对象。

注：在模板语法中setup函数选项需return出一个对象，而在jsx语法中setup需return出一个箭头函数，所有原先在template标签中书写的内容需写在该函数体内，并且函数体只能有一个根标签。

```javascript
// App.js
import { defineComponent } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import Logo from "./assets/logo.png";
export default defineComponent({
   name: "App",
   components: { HelloWorld },
   setup() {
     return () => (
       <>
         <img alt="Vue logo" src="/{Logo}" />
         <HelloWorld msg="Hello Vue 3 + Vite" />
       </>
     );
   },
});
```

### 第三部分：组件定义：
#### 一、定义函数式组件
```javascript
const App = () => <div>Vue 3.0</div>;
```
#### 二、在函数式组件使用render函数
```javascript
const App = {
   render() {
     return <div>Vue 3.0</div>;
   },
 };
```

#### 三、推荐写法（除了指令、样式其余跟模板语法无异）
```javascript
import { defineComponent } from "vue"
export default defineComponent({
     setup() {
         return () => {

             <div className="nameInfo">欢迎</div>

         }
     }
})
```

### 第四部分、语法介绍
#### 一、样式相关语法介绍
1、如直接导入css文件，则该文件中的class类名可在项目的任何组件中使用，相当于全局类名（属性名class需要写成className）。

```javascript
// App.css
.nameInfo{
     color:red;
}
// App.jsx
import { defineComponent } from "vue"
import "./App.css"
export default defineComponent({
     setup() {
         return () => {
             <div className="nameInfo">欢迎~</div>
         }
     }
})
```
2、vite天然支持CSS modules，任何以 .module.css 为后缀名的 CSS 文件都被认为是一个 CSS modules文件。导入这样的文件会返回一个相应的模块对象：

```javascript
/* example.module.css */
.red {
   color: red;
}
import { defineComponent } from "vue"
import style from "./example.module.css"
export default defineComponent({
     setup() {
         return () => {
             <div className={style.red}>欢迎~</div>
         }
     }
})
```

3、内联样式写法。

```javascript
import { defineComponent } from "vue"
export default defineComponent({
     setup() {
         return () => {
             <div style={{ color: "red" }}>欢迎~</div>
         }
     }
})
```

#### 二、vue常用指令语法
1、v-text

```javascript
 import { defineComponent, ref } from "vue";
 export default defineComponent({
   name: "HelloWorld",
   setup() {
     const text = ref("欢迎");
     return () => (
       <>
         <h1 v-text={text.value}></h1>
       </>
     );
   },
 });
```
2、v-html

```javascript
import { defineComponent, ref } from "vue";
export default defineComponent({
   name: "HelloWorld",
   setup() {
     const text = ref("欢迎");
     return () => (
       <>
         <h1 v-html={text.value}></h1>
       </>
     );
   },
});
```
3、v-show

```javascript
import { defineComponent, ref } from "vue";
export default defineComponent({
   name: "HelloWorld",
   setup() {
     const visible = ref(true);
     setTimeout(() => {
       visible.value = false;
     }, 2000);
     return () => (
       <>
         <div v-show={visible.value} style={{ color: "red" }}>
           欢迎
         </div>
       </>
     );
   },
 });
```
4、v-if、v-else-if、v-else无法直接使用，需使用逻辑与、逻辑或、三元表达式实现条件渲染。

```javascript
import { defineComponent, ref } from "vue";
export default defineComponent({
   name: "HelloWorld",
   setup() {
     const h1Show = ref(true);
     const h2Hide = ref(false);
     const h3Show = ref(true);
     return () => (
       <>
         {h1Show.value && <h1>这个显示</h1>}
         {h2Hide.value && <h2>这个不显示</h2>}
         {h3Show.value ? <h3>h3Show.value为true显示</h3> : <h3>h3Show.value为false显示</h3>}
       </>
     );
   },
 });
```

5、v-for无法直接使用，需使用map去实现循环遍历渲染。

```javascript
import { defineComponent, reactive } from "vue";
export default defineComponent({
   name: "HelloWorld",
   setup() {
     const list = reactive([1, 2, 3]);
     return () => (
       <>
         {list.map(item => (
           <h1>值为：{item}</h1>
         ))}
       </>
     );
   },
 });
```
6、v-on无法直接使用，需使用原生绑定事件方式去实现，因此无法使用事件修饰符。

```javascript
// 不需要传值 
 import { defineComponent, ref } from "vue";
 export default defineComponent({
   name: "HelloWorld",
   setup() {
     const myName = ref("~WEB前端~");
     const changName = () => {
       myName.value = "欢迎";
     };
     return () => (
       <>
         <button onClick={changName}>{myName.value}</button>
       </>
     );
   },
 });
  // 传值的写法（高阶函数）
 import { defineComponent, ref } from "vue";
 export default defineComponent({
   name: "HelloWorld",
   props: ["msg"],
   setup() {
     const myName = ref("~WEB前端~");
     const changName = value => {
       console.log("value", value);
       myName.value = "欢迎";
     };
     return () => (
       <>
         <button onClick={() => changName("欢迎")}>{myName.value}</button>
       </>
     );
   },
 });
 // 获取事件对象
 import { defineComponent, ref } from "vue";
 export default defineComponent({
   name: "HelloWorld",
   props: ["msg"],
   setup() {
     const myName = ref("~WEB前端~");
     const changName = (event, value) => {
       console.log("event", event);
       console.log("value", value);
       myName.value = "欢迎";
     };
     return () => (
       <>
         <button onClick={event => changName(event, "欢迎~")}>{myName.value}</button>
       </>
     );
   },
 });
```
7、 v-bind（需注意以下三点）

1、标签属性值如果需要一个变量，需要按照 属性名={变量名} 的形式书写，并且属性名前不能带 “:” ；

2、如果是ref包装之后的响应式变量，需要按照 属性名={变量名.value} 的形式书写；
3、图片资源需先导入后使用，如下示例中的logo图片资源，
```javascript
 import { defineComponent, ref } from "vue"
 import Logo from "./assets/logo.png"
 export default defineComponent({
     setup() {
         const altText = ref("Vue logo")
         return () => {
             <img alt={ altText.value } src={ Logo } />
         }
     }
 })
```
8、v-model

```javascript
import { defineComponent, ref } from "vue";
export default defineComponent({
   name: "HelloWorld",
   setup() {
     const text = ref("WEB前端~");
     return () => (
       <>
         <h1>{text.value}</h1>
         <input v-model={text.value} placeholder="~WEB前端~" />
       </>
     );
   },
 });
```
9、v-slot，在jsx中v-slot，需要写成v-slots

```javascript
 // HelloWorld.jsx
 import { defineComponent, ref } from "vue";
 export default defineComponent({
   name: "HelloWorld",
   props: ["msg"],
   setup(props, { slots }) {
     return () => (
       <>
         <h1>{slots.default ? slots.default() : "WEB前端"}</h1>
         <h2>{slots.bar?.()}</h2>
       </>
     );
   },
 });
```
第一种：
```javascript
 // App.jsx
import { defineComponent } from "vue";
import HelloWorld from "./components/HelloWorld.jsx";
export default defineComponent({
  name: "App",
  components: { HelloWorld },
  setup() {
    const slots = {
      bar: () => <span>这个会渲染到h2中</span>,
    };
    return () => (
      <>
        <HelloWorld v-slots={slots}>
          <div>这个会渲染到H1中</div>
        </HelloWorld>
      </>
    );
  },
});
```

第二种：
```javascript
 // App.jsx

 import { defineComponent } from "vue";
 import HelloWorld from "./components/HelloWorld.jsx";
 export default defineComponent({
   name: "App",
   components: { HelloWorld },
   setup() {
     const slots = {
       default: () => <div>这个会渲染到H1中</div>,
       bar: () => <span>这个会渲染到h2中</span>,
     };
     return () => (
       <>
         <HelloWorld v-slots={slots} />
       </>
     );
   },
 });
```
第三种：
```javascript
 // App.jsx

 // h1当中渲染子组件默认的
 // h2当中则渲染父组件传入的bar组件内容
 import { defineComponent } from "vue";
 import HelloWorld from "./components/HelloWorld.jsx";

 export default defineComponent({
   name: "App",
   components: { HelloWorld },
   setup() {
     const slots = {
       bar: () => <span>这个会渲染到h2中</span>,
     };
     return () => (
       <>
         <HelloWorld v-slots={slots} />
       </>
     );
   },
 });
```
10、v-pre、v-cloak、v-once、v-memo 四个指令暂时未研究出来如何在jsx中去实现，欢迎补充。

### 第五部分、安装组件库（以Naive UI为例）
#### 一、使用 pnpm安装naive-ui依赖
```javascript
$ pnpm install naive-ui
```
#### 二、建议按需引入，在使用组件库组件时，标签名应采用大驼峰式书写格式
```javascript
 import { defineComponent, ref } from "vue";
 import { NButton } from "naive-ui";
 export default defineComponent({
   name: "HelloWorld",
   props: ["msg"],
   components: { NButton },
   setup() {
     const myName = ref("~WEB前端~");
     const changName = (event, value) => {
       console.log("event", event);
       console.log("value", value);
       myName.value = "~WEB前端~";
     };
     return () => (
       <>
         <NButton type="primary" onClick={event => changName(event, "~WEB前端~")}>
           {myName.value}
         </NButton>
       </>
     );
   },
 });
```

至此如何在vue中使用jsx语法内容全部讲完！！！
### 基本语法对照 SFC
#### defineComponent 和 setup
SFC方式结构固定：template、script、style
![1713774292102](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774292102.png)
TSX方式就完全是一个ts文件的写法，没有模板template和样式style
![1713774284091](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774284091.png)
setup中函数的返回值有多种方式，可以直接返回html，这个适合结构简单的页面，如果返回比较多，可以使用如下方式：
![1713774273408](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774273408.png)
如果是多节点，可以使用空符号包裹
![1713774264279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774264279.png)
在以上的方式中我们把除了布局以外的逻辑都写在//Todo部分，但是有时候我们需要做一些按条件渲染的逻辑，那么也可以在return里加处理逻辑，例如：
![1713774254950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774254950.png)
这种方式类似v-if，但是和v-if还是有点区别，v-if可以作用在更小的范围，而这种方式只适合整个组件的条件渲染，这个可能不好理解，在下面v-if的使用中我们会看到区别。

v-if
使用条件判断语句来实现v-if的功能，一般是三目运算符
![1713774241248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774241248.png)
在这里你可以看到v-if的使用和我们上面的条件返回不一样，区别就是整体渲染没有大的变化，只是其中部分地方要按条件显示。

v-bind
绑定变量，也就是简写的:冒号，修改方式就是将冒号去掉，把双引号改为大括号
![1713774227777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774227777.png)
v-for
采用map循环的方式，返回一个数组
![1713774215535](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774215535.png)
自定义指令
自定义指令和普通指令v-model一样
![1713774199522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774199522.png)
插槽
插槽有两种实现方式，一种是用v-slots绑定对象，一种是直接在元素中使用对象。
![1713774186311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774186311.png)
![1713774175926](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774175926.png)
props
父组件向子组件传值
![1713774163211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774163211.png)
需要注意的是，prop传递过来的值如果没有默认值，需要判断是否为空，可以使用计算属性或者条件渲染处理。

emit
子组件向父组件传值
![1713774151126](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774151126.png)
事件监听
事件监听就是v-on或者@，在TSX中事件以on开头，即使我们的自定义事件没有on，也要在监听的时候加上，一般都采用的是小驼峰的方式。
![1713774139295](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774139295.png)
自定义事件只需要在事件名前面加上on即可，参数传递与上面一致
![1713774126234](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774126234.png)
在TSX中处理事件不能使用事件修饰符，因此需要在事件函数中自行处理，例如冒泡、阻止默认行为等。

属性/事件继承
对于这个我也不知道怎么描述，当我们给一个组件传递属性和事件时，一般子组件在props中接收属性值，emits中接收事件，但是我们也可以传一些额外的属性和事件，即不在props和emits中的属性和事件，虽然这是不推荐的做法，但是有时候当我们封装第三方库的时候，这种用法就非常的方便。具体看如下代码：
![1713774116877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774116877.png)
SFC中，在template中我们可以通过$attrs获取到额外的属性和方法，script中可以通过getCurrentInstance方法获取组件对象，然后通过.attrs拿到属性和方法。

TSX中，直接通过attrs获取属性和方法，通过{...attrs}把属性和方法传递给子元素。

其他命令
v-show和v-model与SFC中使用一样，这里不做示例

组件引用
通过ref获取组件dom信息
![1713774104298](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774104298.png)
对外暴露属性和方法
在父组件中直接调用子组件的属性和方法
![1713774095139](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713774095139.png)