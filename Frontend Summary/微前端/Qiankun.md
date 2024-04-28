#### 1. **什么是qiankun**

```
qiankun 是一个基于 [single-spa](https://github.com/CanopyTax/single-spa) 的[微前端](https://micro-frontends.org/)实现库，旨在帮助大家能更简单、无痛的构建一个生产可用微前端架构系统。
```

#### 2. **qiankun 的核心设计理念**

```js
// 1. 简单

由于主应用微应用都能做到技术栈无关，qiankun 对于用户而言只是一个类似 jQuery 的库，你需要调用几个 qiankun 的 API 即可完成应用的微前端改造。同时由于 qiankun 的 HTML entry 及沙箱的设计，使得微应用的接入像使用 iframe 一样简单。

// 2. 解耦/技术栈无关

微前端的核心目标是将巨石应用拆解成若干可以自治的松耦合微应用，而 qiankun 的诸多设计均是秉持这一原则，如 HTML entry、沙箱、应用间通信等。这样才能确保微应用真正具备 独立开发、独立运行 的能力。
```

#### 3. **为什么不是 iframe**

```
iframe 最大的特性就是提供了浏览器原生的硬隔离方案，不论是样式隔离、js 隔离这类问题统统都能被完美解决。但他的最大问题也在于他的隔离性无法被突破，导致应用间上下文无法被共享，随之带来的开发体验、产品体验的问题。

\1. url 不同步。浏览器刷新 iframe url 状态丢失、后退前进按钮无法使用。

\2. UI 不同步，DOM 结构不共享。想象一下屏幕右下角 1/4 的 iframe 里来一个带遮罩层的弹框，同时我们要求这个弹框要浏览器居中显示，还要浏览器 resize 时自动居中..

\3. 全局上下文完全隔离，内存变量不共享。iframe 内外系统的通信、数据同步等需求，主应用的 cookie 要透传到根域名都不同的子应用中实现免登效果。

\4. 慢。每次子应用进入都是一次浏览器上下文重建、资源重新加载的过程。
```

#### 4. **特性**

```js
·  基于 [single-spa](https://github.com/CanopyTax/single-spa) 封装，提供了更加开箱即用的 API。

·  技术栈无关，任意技术栈的应用均可 使用/接入，不论是 React/Vue/Angular/JQuery 还是其他等框架。

·  HTML Entry 接入方式，让你接入微应用像使用 iframe 一样简单。

·  样式隔离，确保微应用之间样式互相不干扰。

·  JS 沙箱，确保微应用之间 全局变量/事件 不冲突。

· 资源预加载，在浏览器空闲时间预加载未打开的微应用资源，加速微应用打开速度。

·  umi 插件，提供了 [@umijs/plugin-qiankun](https://github.com/umijs/plugins/tree/master/packages/plugin-qiankun) 供 umi 应用一键切换成微前端架构系统。
```

#### 5. **搭建qiankun**

> 主应用不限技术栈，只需要提供一个容器 DOM，然后注册微应用并 start 即可。

##### (1) **主应用(vue)**

###### **1. 安装 qiankun**

```
$ yarn add qiankun 
# 或者 
npm i qiankun -S
```

###### **2. 在主应用中注册微应用**

```js
// Main.js

import { registerMicroApps, start } from 'qiankun';
//import { registerMicroApps, runAfterFirstMounted, setDefaultMountApp, start, initGlobalState } from 'qiankun';

// 注册 微应用
registerMicroApps([
  {
    name: 'mcro-vue2', // 微应用的名字
    entry: '//localhost:8888', //访问微应用的路径，可以在微应用的vue.config.js中配置，使用fetch请求接口
    container: '#container', //在主应用的 容器，微应用渲染到主应用的容器
    activeRule: '/mcro-vue2', //激活规则，点击访问该微应用
    props: {}, //传入微应用的参数
    loader: loading => { //加载状态
      console.log(loading)
    }
  },
  {
    name: 'vueApp',
    entry: '//localhost:8080',
    container: '#container',
    activeRule: '/app-vue',
  },
  {
    name: 'angularApp',
    entry: '//localhost:4200',
    container: '#container',
    activeRule: '/app-angular',
  }
],{ //生命周期钩子
  beforeLoad:()=>{
    console.log('加载前');
  },
  beforeMount:()=>{
    console.log('挂载前');
  },
  afterMount:()=>{
    console.log('挂载后');
  },
  beforeUnmount:()=>{
    console.log('销毁前');
  },
  afterUnmount:()=>{
    console.log('销毁后');
  },
});

// 定义全局状态，并返回两个通信方法
//const { onGlobalStateChange, setGlobalState } = initGlobalState({
//  user: 'qiankun',
//});

// 监听全局状态的更改，当状态发生改变时执行回调函数
//onGlobalStateChange((value, prev) => console.log('[onGlobalStateChange - master]:', value, prev));

// 设置新的全局状态，只能设置一级属性，微应用只能修改已存在的一级属性
// setGlobalState({
//  ignore: 'master',
//  user: {
//    name: 'master',
//  },
//});

 
// 设置默认进入的子应用，当主应用启动以后默认进入指定微应用
// setDefaultMountApp('/react16');


// 启动 qiankun

start();

```

当微应用信息注册完之后，一旦浏览器的 url 发生变化，便会自动触发 qiankun 的匹配逻辑，所有 activeRule 规则匹配上的微应用就会被插入到指定的 container 中，同时依次调用微应用暴露出的生命周期钩子。

如果微应用不是直接跟路由关联的时候，你也可以选择手动加载微应用的方式：

```javascript
import { loadMicroApp } from 'qiankun';

loadMicroApp({
  name: 'app',
  entry: '//localhost:7100',
  container: '#yourContainer',
});
```

--------------------------------------------------------------------------------------------

```vue
// APP.vue

<template>
  <div id="app">
    <div>main</div>
    <a href="/mcro-vue2">mcro-vue2</a>

	<!-- 用router-link 会报错 -->
    <!-- <router-link to="/mcro-vue2">mcro-vue2</router-link> -->
    <!-- <router-view></router-view> -->
    <!-- 微应用放置的容器 -->

    <div id="container"></div>
  </div>
</template>
```

##### (2) **Vue 微应用** **(不需要安装qiankun)**

在 src 目录新增 public-path.js：

```js
/**
 * 在入口文件中使用 ES6 模块导入，则在导入后对 __webpack_public_path__ 进行赋值。
 * 在这种情况下，必须将公共路径(public path)赋值移至专属模块，然后将其在最前面导入
 */

 
// qiankun 设置的全局变量，表示应用作为微应用在运行

if (window.__POWERED_BY_QIANKUN__) {
  // eslint-disable-next-line no-undef
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
}
```

入口文件 main.js 修改，为了避免根 id #app 与其他的 DOM 冲突，需要限制查找范围。

```js
 // 动态设置 __webpack_public_path__

import './public-path'; //静态资源在主应用中没加载出来，可能是没引入这个文件
import Vue from 'vue';
import VueRouter from 'vue-router';
import App from './App.vue';
import routes from './router';
import store from './store';
Vue.config.productionTip = false;
let router = null;
let instance = null;

/**/ 不能直接挂载，需要切换的时候，调用 mount方法后再去挂载**
// new Vue({
//   render: h => h(App),
// }).$mount('#app')


function render(props = {}) {
  const { container } = props;

  // 实例化 router，根据应用运行环境设置路由前缀

  router = new VueRouter({
    // 作为微应用运行，则设置 /vue 为前缀，否则设置 /
    base: window.__POWERED_BY_QIANKUN__ ? '/app-vue/' : '/',
    mode: 'history',
    routes,
  });
  instance = new Vue({
    router,
    store,
    render: (h) => h(App),
  }).$mount(container ? container.querySelector('#app') : '#app');

}

// 独立运行时

if (!window.__POWERED_BY_QIANKUN__) {
  render();
}

/**
 * 从 props 中获取通信方法，监听全局状态的更改和设置全局状态，只能操作一级属性
 * @param {*} props 
 */

//function storeTest(props)
//  props.onGlobalStateChange &&
//    props.onGlobalStateChange(
//      (value, prev) => console.log(`[onGlobalStateChange - ${props.name}]:`, value, prev),
//      true,
//    );

//  props.setGlobalState &&
//    props.setGlobalState({
//      ignore: props.name,
//      user: {
//        name: props.name,
//      },
//    });
//}

export async function bootstrap() {
  console.log('[vue] vue app bootstraped');
}

export async function mount(props) {
  console.log('[vue] props from main framework', props);
  //storeTest(props);

  // 挂载时需要重新 渲染
  render(props);
}

export async function unmount() {
  instance.$destroy();
  instance.$el.innerHTML = '';
  instance = null;
  router = null;
}
```

打包配置修改（vue.config.js）：

```js
const { name } = require('./package');
module.exports = {
  // publicPath: '//localhost:8888', //保证微应用静态资源都向8888端口上发送 
  // publicPath 没在这里设置，是通过 webpack 提供的全局变量 __webpack_public_path__ 来即时设置的，
  // webpackjs.com/guides/public-path/
  devServer: {
	// 设置跨域，因为主应用需要通过 fetch 去获取微应用引入的静态资源的，所以必须要求这些静态资源支持跨域
    // port:8888,// 使用fetch请求接口
    headers: {
      'Access-Control-Allow-Origin': '*', //跨域
    },
  },
  configureWebpack: {// 打包 vite不可以  systemjs => 转 umd
    output: {
      library: `${name}-[name]`,
      libraryTarget: 'umd', // 把微应用打包成 umd 库格式
      jsonpFunction: `webpackJsonp_${name}`,
    },
  },
};
```

打开 jsonpFunction 会 报错的问题

在 webpack 4 中，多个 webpack 运行时可能会在同一个 HTML 页面上发生冲突，因为它们使用同一个全局变量进行代码块加载。为了解决这个问题，需要为 output.jsonpFunction 配置提供一个自定义的名称。

Webpack 5 确实会从 package.json name 中自动推断出一个唯一的构建名称，并将其作为 output.uniqueName 的默认值。

-------------------------------------------------------------------------------------------

子应用

###### vue2

1. 在 src 目录新增 public-path.js：

```javascript
if (window.__POWERED_BY_QIANKUN__) {
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
}
```
2. 入口文件 main.js 修改，为了避免根 id #app 与其他的 DOM 冲突，需要限制查找范围。

```javascript
import './public-path';
import Vue from 'vue';
import VueRouter from 'vue-router';
import App from './App.vue';
import routes from './router';
import store from './store';

Vue.config.productionTip = false;

let router = null;
let instance = null;
function render(props = {}) {
  const { container } = props;
  router = new VueRouter({
    base: window.__POWERED_BY_QIANKUN__ ? '/app-vue/' : '/',
    mode: 'history',
    routes,
  });

  instance = new Vue({
    router,
    store,
    render: (h) => h(App),
  }).$mount(container ? container.querySelector('#app') : '#app');
}

// 独立运行时
if (!window.__POWERED_BY_QIANKUN__) {
  render();
}

export async function bootstrap() {
  console.log('[vue] vue app bootstraped');
}
export async function mount(props) {
  console.log('[vue] props from main framework', props);
  render(props);
}
export async function unmount() {
  instance.$destroy();
  instance.$el.innerHTML = '';
  instance = null;
  router = null;
}
```
3. 打包配置修改（vue.config.js）：

```javascript
const { name } = require('./package');
const timeStemp = new Date().getTime()
module.exports = {
  devServer: {
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  },
  configureWebpack: {
    output: {
      filename: `js/[name].${timeStamp}.js`,
      chunkFilename: `js/chunk.[id].${timeStemp}.js`,
      library: `${name}-[name]`,
      libraryTarget: 'umd', // 把微应用打包成 umd 库格式
      jsonpFunction: `webpackJsonp_${name}`, // webpack 4
      // chunkLoadingGlobal: `webpackJsonp_${name}`,// webpack 5 需要把 jsonpFunction 替换成 chunkLoadingGlobal
    },
  },
};
```

###### vue3
1. 在 src 目录新增 public-path.js：

```javascript
if (window.__POWERED_BY_QIANKUN__) {
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
}
```
2. main.ts文件

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import routes from './router'
import { createRouter, createWebHashHistory } from 'vue-router'
import './public-path.js'
export { mount, unmount, bootstrap } from './qiankun'

if (!(window as any).__POWERED_BY_QIANKUN__) {
  const router = createRouter({
    history: createWebHashHistory(),
    routes
  })
  createApp(App).use(router).mount('#app')
}

```
3. qiankun.ts

```javascript
import { RouterHistory, createRouter, createWebHashHistory } from 'vue-router'
import { App, inject, InjectionKey, createApp } from 'vue'
import routes from './router'
import MainApp from './App.vue'
let history: RouterHistory | null = null
let app: any = null

interface IGlobalState {
  setGlobalState: (state: Record<string, any>)=> void
  onGlobalStateChange: (func: (state: Record<string, any>, prev: Record<string, any>)=> void)=> void
  offGlobalStateChange: () => boolean
}

export const GlobalStateKey: InjectionKey<IGlobalState> = Symbol('')

// 全局调用乾坤框架消息方便进行消息传递
const createGlobalState = (props: any) => {
  const globalState = {
    install (app: App) {
      app.config.globalProperties.$globalState = props
      app.provide(GlobalStateKey, props)
    }
  }
  return globalState
}

// vue3 use
const useGlobalState = () => {
  // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
  return inject(GlobalStateKey)!
}

/**
 * 应用每次进入都会调用 mount 方法，通常我们在这里触发应用的渲染方法
 */
const mount = async (props: any) => {
  const { container } = props
  history = createWebHashHistory('/meeting/')
  const router = createRouter({
    history,
    routes
  })
  app = createApp(MainApp)
  app.use(router).use(createGlobalState(props)).mount(container.querySelector('#app'))
}

/**
 * 应用每次 切出/卸载 会调用的方法，通常在这里我们会卸载微应用的应用实例
 */
const unmount = async () => {
  app.unmount()
  if (history) {
    history.destroy()
  }
}

// bootstrap 只会在微应用初始化的时候调用一次，下次微应用重新进入时会直接调用 mount 钩子，不会再重复触发 bootstrap。
// 通常我们可以在这里做一些全局变量的初始化，比如不会在 unmount 阶段被销毁的应用级别的缓存等。
const bootstrap = async () => {
  console.log('%c%s', 'color: green;', 'vue3.0 app bootstrap')
}

/**
 * 可选生命周期钩子，仅使用 loadMicroApp 方式加载微应用时生效
 */
const update = async (props: any) => {
  console.log('update props', props)
}

export { mount, unmount, bootstrap, update, useGlobalState }

```
###### vue3（vite）

> 1. vite和webpack不一致没有 webpack_public_path这个东西需要安装插件：vite-plugin-qiankun
> 2. vite-plugin-qiankun 提供了一些公共方法可以直接使用避免了很多麻烦
1. 配置vite.config.ts

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import qiankun from 'vite-plugin-qiankun'
import path from 'path'
//! useDevMode = true 时不开启热更新
const useDevMode = true;
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    qiankun('micro-org', {useDevMode})
  ],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src')  // 别名
    }
  },
  server: {
    port: 8081
  }
})
```
2. 在 src 目录新增 public-path.js：

```javascript
if (window.__POWERED_BY_QIANKUN__) {
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
}
```
3. main.ts文件

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import { createRouter, createWebHashHistory } from 'vue-router'
import routes from './router'
import './public-path.js'
import {
  renderWithQiankun,
  qiankunWindow
} from 'vite-plugin-qiankun/dist/helper'
// 通过renderWithQiankun导出
import { mount, unmount, bootstrap, update } from './qiankun'

const initQianKun = () => {
  // QiankunLifeCycle
  renderWithQiankun({
    mount,
    bootstrap,
    unmount,
    update
  })
}

const render = () => {
  const router = createRouter({
    history: createWebHashHistory(),
    routes,
  })
  createApp(App).use(router).mount('#app')
}

qiankunWindow.__POWERED_BY_QIANKUN__ ? initQianKun() : render()
```
4. qiankun.ts

```javascript
import { RouterHistory, createRouter, createWebHashHistory } from 'vue-router'
import { App, inject, InjectionKey, createApp } from 'vue'
import { QiankunProps } from 'vite-plugin-qiankun/dist/helper'
import routes from './router'
import MainApp from './App.vue'
let history: RouterHistory | null = null
let app: any = null

interface IGlobalState {
  setGlobalState: (state: Record<string, any>) => void
  onGlobalStateChange: (
    func: (state: Record<string, any>, prev: Record<string, any>) => void
  ) => void
  offGlobalStateChange: () => boolean
}

export const GlobalStateKey: InjectionKey<IGlobalState> = Symbol('')

// 全局调用乾坤框架消息方便进行消息传递
const createGlobalState = (props: any) => {
  const globalState = {
    install(app: App) {
      app.config.globalProperties.$globalState = props
      app.provide(GlobalStateKey, props)
    },
  }
  return globalState
}

// vue3 use
const useGlobalState = () => {
  // eslint-disable-next-line @typescript-eslint/no-non-null-assertion
  return inject(GlobalStateKey)!
}

/**
 * 应用每次进入都会调用 mount 方法，通常我们在这里触发应用的渲染方法
 */
const mount = async (props: QiankunProps) => {
  const { container } = props
  history = createWebHashHistory('/org/')
  const router = createRouter({
    history,
    routes,
  })
  app = createApp(MainApp)
  if (container) {
    app
      .use(router)
      .use(createGlobalState(props))
      .mount(container.querySelector('#app'))
  }
}

/**
 * 应用每次 切出/卸载 会调用的方法，通常在这里我们会卸载微应用的应用实例
 */
const unmount = async () => {
  app.unmount()
  if (history) {
    history.destroy()
  }
}

// bootstrap 只会在微应用初始化的时候调用一次，下次微应用重新进入时会直接调用 mount 钩子，不会再重复触发 bootstrap。
// 通常我们可以在这里做一些全局变量的初始化，比如不会在 unmount 阶段被销毁的应用级别的缓存等。
const bootstrap = async () => {
  console.log('%c%s', 'color: green;', 'vue3.0 app bootstrap')
}

/**
 * 可选生命周期钩子，仅使用 loadMicroApp 方式加载微应用时生效
 */
const update = async (props: any) => {
  console.log('update props', props)
}

export { mount, unmount, bootstrap, update, useGlobalState }
```