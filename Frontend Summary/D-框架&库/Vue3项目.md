### 后台管理系统

1.安装element plus 并配置还开发环境 

2.定义项目主题色 

\3. 

public 站点目录 

https://lin-xin.gitee.io/example/work/#/dashboard 

源码：https://github.com/cuiweijun/vue3-project

#### 1. **修改主题色 element plus**

1.css变量 

<template> 

<div class="home"> 

<h3>hello</h3> 

<el-button type="primary" class="btn">Primary</el-button> 

</div> 

</template> 

.home{ 

--button-bk-color:#f00;//声明变量 

h3{ 

color:red; 

} 

.btn{ 

background: var(--button-bk-color);//使用 

} 

}

#### 2. **本地图片存放，获取(assets\public区别)**

1.获取 本地图片 路径，使用json数据 json-server xxx.json 那么最好放置在public中 

因为 项目 运行后 public是站点目录，路径可以通过 ./imgs(图片在public中的位置) 获取到，在页面中显示。 

\2. 获取 本地图片 路径，使用mock 创建的数据， 

public文件夹中： 

**public是站点目录**，路径可以通过 ./imgs(图片在public中的位置) 获取到，在页面中显示。 

**不会变成base64格式** 

**assets文件夹中：** 

不能使用 相对路径 绝对路径 

require('@/assets/imgs/d06e958d6d024c69a68c03c4b30da8b2.png') 

**会变成base64格式** 

#### 3. **使用element-ui 的图标**

1.安装：npm install @element-plus/icons-vue 

2.注册所有图标 ： 

// main.ts 

// if you're using CDN, please remove this line. 

import * as ElementPlusIconsVue from '@element-plus/icons-vue' 

const app = createApp(App) 

for (const [key, component] of Object.entries(ElementPlusIconsVue)) { 

app.component(key, component) 

} 

3.点击图标 复制 -》 黏贴到项目 

<el-icon><cherry /></el-icon>

#### 4. **全屏**

参考：https://www.cnblogs.com/libo0125ok/p/15438067.html 

function useFullscreen(isFullScreen){ 

// 全屏 

if(isFullScreen.value){//全屏 

let doc = document.webkitCancelFullScreen||document.mozCancelFullScreen||document.exitlFullScreen 

//doc()//取消全屏 this指向window 

doc.call(document) 

}else{ 

let el = document.documentElement 

let fullscreen = el.webkitRequestFullScreen||el.mozRequestFullScreen||el.requestFullScreen 

fullscreen.call(el) 

} 

isFullScreen.value = !isFullScreen.value 

} 

export default useFullscreen

#### 5. **可视化**

Hcharts：国外的一款图表库，是图表库的领头羊 

Echarts：百度开发的数据可视化库，国内图表库的 “领军人物” 

https://echarts.apache.org/zh/index.html 

AntV：是蚂蚁金服开发的数据可视化库 

###### **Vue使用vue-echarts图表**

**vue-echarts和echarts的区别：** 

vue-echarts是封装后的vue插件， 基于 [ECharts](http://echarts.baidu.com/index.html) v4.0.1+ 开发，依赖 [Vue.js](https://vuejs.org/) v2.2.6+，功能一样的只是把它封装成vue插件 这样更方便以vue的方式去使用它。 

echarts就是普通的js库 

**vue-echarts特征：** 

轻量，高效，按需绑定事件 

支持按需导入ECharts.js图表和组件 

支持组件调整大小事件自动更新视图 

git地址：https://github.com/ecomfe/vue-echarts 

###### **vue-schart 使用**

文档：https://lin-xin.gitee.io/example/schart/ 

git库：https://github.com/lin-xin/vue-schart 

npm install vue-schart -S 

引入该组件：import Schart from 'vue-schart' 

注册该组件：components:{ Schart } 

在template标签中使用： 

<schart :canvasId="canvasId" 

:type="type" 

:width="width" 

:data="data" 

:options="options"> 

</schart> 

图表中的数据： 

// bar表示的是柱状图 

//只能使用name和value。name为坐标的标识，value为当前数据的数量 

data(){ 

return { 

canvasId: 'myCanvas', 

type: 'bar', 

width: 600, 

height: 500, 

data: [ 

{name: '这'，value: 1}, 

{name: '是'，value: 2}, 

{name: '一'，value: 3}, 

{name: '个'，value: 4}, 

{name: '例'，value: 5}, 

{name: '子'，value: 6} 

], 

//可选参数 

options: { 

title: 'Total sales of stores in recent years' 

} 

} 

} 

数据响应： 

methods: { 

changeData(){ 

// 整个 data 被重新赋值时 

this.data = [ 

{name:'短袖', value:1200}, 

{name:'休闲裤', value:1222}, 

{name:'连衣裙', value:1283}, 

{name:'外套', value:1314} 

]; 

// data 里一个属性值被修改时 

this.$set(this.data, 1, {name:'短裙',value:500}); 

} 

}

#### 6. **vue2和vue3 添加公共的插件 getCurrentInstance**

vue2 

import axios from 'axios' 

Vue.prototype.$axios = axios 

thi.$axios.get() 

vue3 

let app = createApp(options) 

app.config.globalProperties.$axios = axios 

//组件中 

mport { getCurrentInstance } from 'vue' 

const MyComponent = { 

setup() { 

const internalInstance = getCurrentInstance() 

internalInstance.appContext.config.globalProperties // 访问 globalProperties 

} 

} 

getCurrentInstance 只能在 [setup](#setup) 或[生命周期钩子](#生命周期钩子)中调用。 

如需在 [setup](#setup) 或[生命周期钩子](#生命周期钩子)外使用，请先在 setup 中调用 getCurrentInstance() 获取该实例然后再使用。

#### 7. **拖拽 vue-draggable**

https://github.com/SortableJS/vue.draggable.next 

npm i -S vuedraggable@next 

import draggable from 'vuedraggable'

###  移动端

1.vue create projectname 

2.下载 less 包 

npm i less less-loader@7.3.0 

3.下载 vant ui组件库 

https://vant-contrib.gitee.io/vant/#/zh-CN 

npm i vant -S 

//main.js 

import Vant from 'vant'; 

import 'vant/lib/index.css'; 

app.use(Vant); 

4.下载 axios 

npm i axios -S 

5.rem适配 (将[lib-flexible](https://github.com/amfe/lib-flexible)插件换成amfe-flexible） 

用 vant 提供的 rem适配方案 

npm install postcss-pxtorem -D 

npm install amfe-flexible -S -D 

直接使用 px 

#### 1. **vant3修改主题色**

Vant 提供了更方便的 [ConfigProvider 全局配置](#/zh-CN/config-provider) 组件进行主题配置。基于 Less 变量进行定制的方式将在下个大版本废弃。 

Vant 组件通过丰富的 [CSS 变量](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Using_CSS_custom_properties) 来组织样式，通过覆盖这些 CSS 变量，可以实现定制主题、动态切换主题等效果。 

1.自定义 CSS 变量 

:root { 

--van-button-primary-background-color: red; 

} 

2.通过 ConfigProvider 覆盖 

ConfigProvider 组件提供了覆盖 CSS 变量的能力，你需要在根节点包裹一个 ConfigProvider 组件，并通过 theme-vars 属性来配置一些主题变量。 

注意：ConfigProvider 仅影响它的子组件的样式，不影响全局 root 节点。 

#### 2. **rem适配(vant组件库中有适配方案)**

[postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 是一款 PostCSS 插件，用于将 px 单位转化为 rem 单位 

[lib-flexible](https://github.com/amfe/lib-flexible) 用于设置 rem 基准值 

1、安装postcss、lib-flexible 

安装postcss-pxtorem和amfe-flexible的时候，要注意版本， 否者可能会出现如下 

Error: PostCSS plugin tailwindcss requires PostCSS 8 这个因为版本出问题了，下载之后， 

npm install postcss-pxtorem -D 

npm install amfe-flexible -S -D 

2、main.js中引入 

import 'amfe-flexible'; 

3、创建名为postcss.config.js的文件放在根目录，记得要重新运行项目 

module.exports = { 

plugins: { 

'autoprefixer': {}, 

'postcss-pxtorem': { 

rootValue: 75, //一个rem等于75个px;相当于vue/css文件中写width:75;会转换成width:1rem;再根据html的fontsize计算实际的width: 

propList: ['*'], //允许哪些属性转成rem; 

unitPrecision: 5, //最多小数位数; 

selectorBlackList: [/^\.van-/,/^\.mescroll/], //忽略.van-和.mescroll开头的类名; 

replace: true, 

mediaQuery: false, //允许在媒体查询中转换px 

minPixelValue: 2, //设置要替换的最小像素值 

} 

} 

} 

先使用下边简化的 

module.exports = { 

plugins: { 

'postcss-pxtorem': { 

rootValue: 37.5, 

propList: ['*'], 

}, 

}, 

}; 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps50.jpg) 

<style lang="less"></style> 报错问题

使用lang=“less“时，会报错，因为没有less-loader 插件，需要下载，但是还报错，是因为下载的less-loader版本太高（查看webpack的版本 npm view webpack version，查看less-loader版本 npm view less-loader version）安装适合webpack 4的less-loader版本为7， 

安装 npm i less-loader@7 

lang=”less“ 可以嵌套着写

##### **样式穿透**

样式穿透主要应用场景是在项目中运用UI组件库（element, vant, vuetify）后要改动其内在样式，因此采用深度选择器 

三大样式穿透 >>> , ::v-deep , /deep/ 

\>>> 只作用于css 

::v-deep 只作用于sass 

/deep/ 只作用于less

##### **setup中变量的赋值与取值**

import { ref,reactive } from 'vue' 

setup(){ 

基本数据类型：变量用 let 声明 

let index = ref(0) 

//赋值 

index.value = 1 

//取值 

// return导出 在 标签中 直接使用index即可 

引用数据类型：let const 都可以声明 

let obj = reactive({pro:{}) // let obj = reactive({pro:[]) 数组 

//赋值 

obj.pro = data 

//取值 

// return导出 在 标签中 使用obj.pro才行 

return { 

index,obj 

} 

} 

下面是 例子

![1709185421981](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709185421981.png)

### 项目优化

项目优化:1.减少http请求，把小图片做成雪碧图，2.压缩html,css,js代码，3.减少回流和重绘，4.尽量使用事件委托，5.减少使用css表达式express，6.最好不要修改图片的大小，7.不滥用float

