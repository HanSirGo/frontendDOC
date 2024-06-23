# qiankun实战踩坑记

## 问题1：子应用加载的资源会 404

![1719132270231](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132270231.png)

## 子应用是webpack项目

### 官方参考：

https://qiankun.umijs.org/zh/faq#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%BE%AE%E5%BA%94%E7%94%A8%E5%8A%A0%E8%BD%BD%E7%9A%84%E8%B5%84%E6%BA%90%E4%BC%9A-404

https://www.webpackjs.com/configuration/output/#outputpublicpath

### 解决方案：

1. 在子应用项目根目录下创建public-path.js文件，文件内容如下：

```js
/ 解决子应用加载资源404
if (window.__POWERED_BY_QIANKUN__) {
  // eslint-disable-next-line 
  __webpack_public_path__ = window.__INJECTED_PUBLIC_PATH_BY_QIANKUN__;
  }
```

2. 在项目入口文件中引入这个文件。

## 子应用是vite项目

在vite配置文件-服务器选项下增加origin配置。

![1719132309134](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132309134.png)

server.origin 用于定义开发调试阶段生成资源的origin。

## 问题2: 子应用devServer中的websocket连接报错：Invalid Host/Origin header

![1719132354970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132354970.png)

![1719132368848](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132368848.png)

## 解决方案

在子应用的vue-cli的配置中增加如下内容：

![1719132382909](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132382909.png)

## 子应用样式丢失

原因： 错误的将子应用挂载到了container指定的节点上，正确的是将子应用挂载到container指定的节点下的`#app`节点上。

- 错误代码

```js
function render(props = {}) {
  const { container } = props
  instance = createApp(App).mount(container? container : '#app')
}
```

- 正确代码

```js
function render(props = {}) {
  const { container } = props
  instance = createApp(App).mount(container? container.querySelector('#app') : '#app')
}
```

## 问题3: Cannot GET /__vite_ping

vite子应用请求`https://www.ysg.com:3001/__vite_ping`变成了`https://www.ysg.com:8088/__vite_ping`（8088是主应用的端口， 3001是子应用的端口）

![1719132442219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132442219.png)

## 子应用何时会发起请求`https://www.ysg.com:3001/__vite_ping`？

浏览器与vite的开发服务器建立websocket连接，当浏览器监听到close事件，就会通过fetch发起`https://www.ysg.com:3001/__vite_ping`请求，该请求是用来判断开发服务器是否正常。下面截图是源码实现。 源码中fetch请求的url有一个变量base。

![1719132456823](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132456823.png)

## url中的base是怎么赋值的呢？

vite内部加入的插件中，clientInjectionsPlugin插件是专门解析ws client文件的，作用就是直接替换掉ws client文件里创建ws 客户端所需要的变量。

![1719132489731](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132489731.png)

从上面的源码中分析出， base变量是从config.base中获取的， 我已经在vite的配置文件增加了base的配置项`base:https://www.ysg.com:3001`，但调试到这里的时候， base依旧是`/`。

## 调试下config.base的生成逻辑

![1719132499016](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132499016.png)

![1719132510781](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132510781.png)

![1719132523321](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132523321.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E) 从源码中分析得到，不是build构建，则config.base不能使用链接。官方文档也有这个描述，如下截图。

![1719132535564](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132535564.png)

## 最终的解决办法

1. 子应用vite配置设置base为`/vue3-element-admin`， 此时子应用的首页地址`https://www.ysg.com:3001/#/home`变成了`https://www.ysg.com:3001/vue3-element-admin/#/home`。
2. 主应用设置webpack配置设置proxy代理如下：

![1719132559892](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719132559892.png)

此时，当子应用开发服务器中断时， 子应用发起`https://www.ysg.com:8088//vue3-element-admin/__vite_ping`就会代理到`https://www.ysg.com:3001//vue3-element-admin/__vite_ping`。