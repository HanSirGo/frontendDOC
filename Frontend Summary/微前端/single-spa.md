## 使用single-spa创建一个微前端项目

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711200706081.png" alt="1711200706081" style="zoom:50%;" />

> ```
> single-spa是一个很好的微前端基础框架，qiankun框架就是基于single-spa来实现的，在single-spa的基础上做了一层封装，也解决了single-spa的一些缺陷。
> ```
>
> Single-spa 是一个将多个单页面应用聚合为一个整体应用的 JavaScript 微前端框架。使用 single-spa 进行前端架构设计可以带来很多好处，例如:
>
> - • 在同一页面上使用多个前端框架 而不用刷新页面 (React, AngularJS, Angular, Ember, 你正在使用的框架)
> - • 独立部署每一个单页面应用
> - • 新功能使用新框架，旧的单页应用不用重写可以共存
> - • 改善初始加载时间，延迟加载代码

## 新建子应用 app1

### 配置子应用

#### **修改vue.config.js**

在项目根目录下修改`vue.config.js`文件

```js
const { defineConfig } = require('@vue/cli-service')
const package = require('./package.json')

module.exports = defineConfig({
  transpileDependencies: true,
  // 告诉子应用在这个地址加载静态资源，否则会去基座应用的域名下加载
  publicPath: '//localhost:8081',
  // 开发服务器
  devServer: {
    port: 8081,
    open: true
  },
  configureWebpack: {
    // 导出umd格式的包，在全局对象上挂载属性package.name，基座应用需要通过这个全局对象获取一些信息，比如子应用导出的生命周期函数
    output: {
      // library的值在所有子应用中需要唯一
      library: package.name,
      libraryTarget: 'umd'
    }
  }
})
```

#### **安装single-spa-vue**

终端执行命令：`npm i single-spa-vue -S`

single-spa-vue负责为vue应用生成通用的生命周期钩子，在子应用注册到single-spa的基座应用时需要用到

#### 改造入口文件

```js
import Vue from 'vue'
import App from './App.vue'
// import router from './router'
import singleSpaVue from 'single-spa-vue'

Vue.config.productionTip = false

const appOptions = {
  el: '#app', 
  // router,
  render: h => h(App)
}

// 支持应用独立运行、部署，不依赖于基座应用
if (!window.singleSpaNavigate) {
  delete appOptions.el
  new Vue(appOptions).$mount('#app')
}

// 基于基座应用，导出生命周期函数
const vueLifecycle = singleSpaVue({
  Vue,
  appOptions
})

export function bootstrap (props) {
  console.log('app1 bootstrap')
  console.log(props)
  return vueLifecycle.bootstrap(() => {})
}

export function mount (props) {
  console.log('app1 mount')
  console.log(props)
  return vueLifecycle.mount(() => {})
}

export function unmount (props) {
  console.log('app1 unmount')
  console.log(props)
  return vueLifecycle.unmount(() => {})
}
```

#### 更改视图文件

```vue
<!-- /views/Home.vue -->
<template>
  <div class="home">
    <h1>app1 home page</h1>
  </div>
</template>

<!-- /views/About.vue -->
<template>
  <div class="about">
    <h1>app1 about page</h1>
  </div>
</template>
```

#### 环境配置文件

**.env**

`应用独立运行时`的开发环境配置

```
NODE_ENV=development
VUE_APP_BASE_URL=/
```

**.env.micro**

`作为子应用运行时`的开发环境配置

```
NODE_ENV=development
VUE_APP_BASE_URL=/app1
```

**.env.buildMicro**

`作为子应用构建生产环境bundle时的`环境配置，但这里的NODE_ENV为development，而不是production，是为了方便，这个方便其实single-spa带来的弊端（js entry的弊端）

```
NODE_ENV=development
VUE_APP_BASE_URL=/app1
```

#### 修改路由文件

```js
// /src/router/index.js
// ...
const router = new VueRouter({
  mode: 'history',
  // 通过环境变量来配置路由的 base url
  base: process.env.VUE_APP_BASE_URL,
  routes
})
// ...
```

#### 修改package.json中的script

```json
{
  "name": "app1",
  // ...
  "scripts": {
    // 独立运行
    "serve": "vue-cli-service serve",
    // 作为子应用运行
    "serve:micro": "vue-cli-service serve --mode micro",
    // 构建子应用
    "build": "vue-cli-service build --mode buildMicro"
  },
  // ...
}
```

#### 启动应用

**应用独立运行**

npm run serve

![1711201378534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711201378534.png)

当然下面的启动方式也可以，只不过会在pathname的开头加了/app1前缀

npm run serve:micro

![1711201399830](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711201399830.png)

**作为子应用运行**

```
npm run serve:micro
```

## 新建子应用 app2

在/micro-frontend目录下新建子应用app2:

- • 步骤同app1
- • 需把过程中出现的'app1'字样改成'app2'
- • vue.config.js中的8081改成8082`

## 基座应用 layout

在/micro-frontend目录下新建基座应用，为了简洁明了，新建项目时选择的配置项和子应用一样。

在本示例中基座应用采用了vue来实现，用别的方式或者框架实现也可以，比如自己用webpack构建一个项目。

### 安装single-spa

```
npm i single-spa -S
```

### 改造基座项目

**入口文件**

```js
import Vue from 'vue'
import router from './router'
import App from './App.vue'
import { registerApplication, start } from 'single-spa'

Vue.config.productionTip = false
// 远程加载子应用
function createScript(url) {
  return new Promise((resolve, reject) => {
    const script = document.createElement('script')
    script.src = url
    script.onload = resolve
    script.onerror = reject
    const firstScript = document.getElementsByTagName('script')[0]
    firstScript.parentNode.insertBefore(script, firstScript)
  })
}

// 记载函数，返回一个 promise
function loadApp(url, globalVar) {
  // 支持远程加载子应用
  return async () => {
    await createScript(url + '/js/chunk-vendors.js')
    await createScript(url + '/js/app.js')
    // 这里的return很重要，需要从这个全局对象中拿到子应用暴露出来的生命周期函数
    return window[globalVar]
  }
}

// 子应用列表
const apps = [
  {
    // 子应用名称
    name: 'app1',
    // 子应用加载函数，是一个promise
    app: loadApp('http://localhost:8081', 'app1'),
    // 当路由满足条件时（返回true），激活（挂载）子应用
    activeWhen: location => location.pathname.startsWith('/app1'),
    // 传递给子应用的对象
    customProps: {}
  }
  /*,
  {
    name: 'app2',
    app: loadApp('http://localhost:8082', 'app2'),
    activeWhen: location => location.pathname.startsWith('/app2'),
    customProps: {}
  },
  {
    // 子应用名称
    name: 'app3',
    // 子应用加载函数，是一个promise
    app: loadApp('http://localhost:3000', 'app3'),
    // 当路由满足条件时（返回true），激活（挂载）子应用
    activeWhen: location => location.pathname.startsWith('/app3'),
    // 传递给子应用的对象，这个很重要，该配置告诉react子应用自己的容器元素是什么，这块儿和vue子应用的集成不一样，官网并没有说这部分，或者我没找到，是通过看single-spa-react源码知道的
    customProps: {
      domElement: document.getElementById('microApp'),
      // 添加 name 属性是为了兼容自己写的lyn-single-spa，原生的不需要，当然加了也不影响
      name: 'app3'
    }
  }*/
]

// 注册子应用
for (let i = apps.length - 1; i >= 0; i--) {
  registerApplication(apps[i])
}

new Vue({
  router,
  mounted() {
    // 启动
    start()
  },
  render: h => h(App)
}).$mount('#app')
```

**App.vue**

```vue
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/app1">app1</router-link> |
      <router-link to="/app2">app2</router-link>
    </div>
    <!-- 子应用容器 -->
    <div id = "microApp">
      <router-view/>
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
```

**路由**

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = []

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

### 启动基座应用

```
npm run serve
```