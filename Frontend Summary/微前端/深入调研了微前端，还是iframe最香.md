# 深入调研了微前端，还是iframe最香

## 微前端是什么

微前端是一种类似于微服务的架构，它将微服务的理念应用于浏览器端，即将 Web 应用由单一的单体应用转变为**「多个小型前端应用聚合为一的应用」**。各个前端应用还可以**「独立运行」**、**「独立开发」**、**「独立部署」**。

简单来说，就是利用一系列工具和技术，将各个团队的UI页面 组装成用户可以连贯的应用程序。

### 后端解耦，前端聚合

采用微服务的原因主要还是在于，**「使用微服务架构来解耦服务间依赖」**。

而在前端微服务化上，则是恰恰与之相反的，人们更想要的结果是**「聚合」**，尤其是那些 To B（to Bussiness）的应用。

在这两三年里，移动应用出现了一种趋势，用户不想装那么多应用了。而往往一家大的商业公司，会提供一系列的应用。这些应用也从某种程度上，反应了这家公司的组织架构。然而，在用户的眼里他们就是一家公司，他们就只应该有一个产品。相似的，这种趋势也在桌面 Web 出现。**「聚合」**成为了一个技术趋势，体现在前端的聚合就是微服务化架构。

### 目的

- 减少团队件的等待时间
- 不再有前端巨石架构

![1717909991366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717909991366.png)

### 优点

- 可独立部署
- 将故障风险的粒度隔离到更小的范围
- 职责范围更窄，更加易于理解
- 拥有更小的代码库，有利于重构和替换
- 状态更易于预测，因为它不与其他系统共享状态

### 缺点

- 冗余

- - 各个团队需要建立维护自己的服务器，构建流程和持续集成的管道，可能还加载冗余的js/css

- 一致性

- - 后端团队有独立的数据库，团队之间需要定期复制数据，一旦出现错误，容易引起数据不一致

- 异质性

- - 技术栈可选择性多

- 更多的前端代码

## 微前端实施方式

### 路由分发式微前端

通过路由将不同的业务**「分发到不同的、独立前端应用」**上。其通常可以通过 HTTP 服务器的反向代理来实现，又或者是应用框架自带的路由来解决。

```js
http {
  server {
    listen       80;
    server_name  www.phodal.com;
    location /api/ {
      proxy_pass http://http://172.31.25.15:8000/api;
    }
    location /web/admin {
      proxy_pass http://172.31.25.29/web/admin;
    }
    location /web/notifications {
      proxy_pass http://172.31.25.27/web/notifications;
    }
    location / {
      proxy_pass /;
    }
  }
}
```

- 不同技术栈之间差异比较大，难以兼容、迁移、改造
- 项目不想花费大量的时间在这个系统的改造上
- 现有的系统在未来将会被取代
- 系统功能已经很完善，基本不会有新需求

### iframe

顾名思义,通过iframe加载子应用。 通信可以通过postMessage进行通信。

#### 优点

- 简单
- 隔离
- 安全

#### 缺点

- 布局约束
- 性能开销
- 破坏了语义化，对无障碍可访问性支持不好哦
- 不利于seo，会当成2个页面
- url 不同步。浏览器刷新 iframe url 状态丢失、后退前进按钮无法使用。
- UI 不同步，DOM 结构不共享。想象一下屏幕右下角 1/4 的 iframe 里来一个带遮罩层的弹框，同时我们要求这个弹框要浏览器居中显示，还要浏览器 resize 时自动居中..
- 全局上下文完全隔离，内存变量不共享。iframe 内外系统的通信、数据同步等需求，主应用的 cookie 要透传到根域名都不同的子应用中实现免登效果。
- 慢。每次子应用进入都是一次浏览器上下文重建、资源重新加载的过程。

### ajax

ajax 请求服务端，直接在主页面区域返回拼装好的html

#### 优点

- 简单
- 自然的文档流
- 利于seo
- 利于无障碍可访问性
- 渐进式增强
- 灵活的错误处理

#### 缺点

- 异步加载
- 缺少隔离性
- 需要向服务器发送请求
- 脚步缺少生命周期

### web component

将前端应用程序分解为自定义 HTML 元素。 基于CustomEvent实现通信
Shadow DOM天生的作用域隔离

重写现有的前端应用，使用 Web Components 来完成整个系统的功能。

- 被Web标准广泛支持
- 自定义元素 shadow DOM 支持隔离
- 引入了生命周期
- shadow 兼容性支持度不够好

## single-spa

- 实现一套生命周期，在 load 时加载子 app，由开发者自己玩，别的生命周期里要干嘛的，还是由开发者造的子应用自己玩
- 监听 url 的变化，url 变化时，会使得某个子 app 变成 active 状态，然后走整套生命周期
- 子应用最关键的一步就是导出 bootstrap, mount, unmount 三个生命周期钩子。
- 基于浏览器原生的事件系统，无框架耦合，全局开箱可用。
- load 方法需要知道子项目的入口文件
- 把多个应用的运行时集成起来需要项目间自行处理内存泄漏，样式污染问题
- 没有提供父子数据通信的方式

![1717910037348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717910037348.png)

## qiankun

![1717910055292](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717910055292.png)

qiankun基于single-spa进行了二次开发

- 主应用：只需要输入子应用的html入口

- 子应用：与single-spa基本一致，导出了三个生命周期函数。

- js隔离

- - Proxy沙箱，它将window上的所有属性遍历拷贝生成一个新的fakeWindow对象，紧接着使用proxy代理这个fakeWindow，用户对window操作全部被拦截下来，只作用于在这个fakeWindow之上

- css隔离

- - ShadowDOM样式沙箱会被开启。在这种模式下 qiankun 会为每个微应用的容器包裹上一个 shadow dom 节点，从而确保微应用的样式不会对全局造成影响。
  - Scoped CSS，qiankun会遍历子应用中所有的CSS选择器，通过对选择器前缀添加一个固定的带有该子应用标识的属性选择器的方式来限制其生效范围，从而避免子应用间、主应用与子应用的样式相互污染。
  - 但如果用户在运行时引入了新的外联样式或者自行创建了新的内联标签，那么qiankun并不会做出反应

- qiankun在框架内部预先设计实现了完善的发布订阅模式

## 无界

使用iframe有三个难以解决的问题，

- **「路由状态丢失」**，刷新一下，iframe的url状态就丢失了
- **「dom割裂严重」**，弹窗只能在iframe内部展示，无法覆盖全局
- **「通信非常困难」**，只能通过postmessage传递序列化的消息

无界微前端框架通过继承iframe的优点，解决iframe的缺点，打造一个接近完美的iframe方案

在应用A中构造一个shadow和iframe，然后将应用B的html写入shadow中，js运行在iframe中，**「注意」** **「iframe」** **「的」** **「url」**，iframe保持和主应用同域但是保留子应用的路径信息，这样子应用的js可以运行在iframe的location和history中保持路由正确。

在iframe中拦截document对象，统一将dom指向shadowRoot，此时比如新建元素、弹窗或者冒泡组件就可以正常约束在shadowRoot内部。

- dom割裂严重的问题，主应用提供一个容器给到shadowRoot插拔，shadowRoot内部的弹窗也就可以覆盖到整个应用A
- 路由状态丢失的问题，浏览器的前进后退可以天然的作用到iframe上，此时监听iframe的路由变化并同步到主应用，如果刷新浏览器，就可以从url读回保存的路由
- 通信非常困难的问题，iframe和主应用是同域的，天然的共享内存通信，而且无界提供了一个去中心化的事件机制

iframe+ web component

- 接入简单：安装一个组件即可 wujie-vue
- css隔离
- js 隔离

![1717910075618](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717910075618.png)

Shadow DOM 是 Web Components 技术的一部分，它允许开发者创建封装、可复用的组件。当一个元素使用 Shadow DOM 创建时，它会包含一个 Shadow Root，这是一个独立的 DOM 子树，与文档中的其他部分相互隔离，可以在其中定义和控制样式和行为。因此，Shadow DOM 的插拔机制也是非常重要的。

在 Shadow DOM 中，插入和移除节点的过程称为“插拔”。Shadow DOM 提供了以下方法来实现插拔：

- attachShadow(options) 方法：该方法将返回一个 ShadowRoot 对象，通过该对象可以管理 Shadow DOM 的内容。
- appendChild(node) 和 removeChild(node) 方法：这些方法允许向 Shadow DOM 中添加或删除节点。
- MutationObserver API：使用该API，可以监视 DOM 树的变化，并在变化发生时采取适当的行动。

需要注意的是，在 Shadow DOM 中，被插入到 Shadow Root 中的元素有可能难以再次获取或操作，因为它们可能不会出现在文档的正常 DOM 树中。为了解决这个问题，我们可以使用 getElementById() 或 querySelector() 等方法，或者在创建自定义元素时定义自定义方法。

### 渲染子应用步骤

- 创建一个iframe，插入主应用document

- 立即停止iframe 的加载

- 

- - 因为 iframe 的 src 要设置为主应用的域名，继续请求资源会失败
  - 修改为主应用域名是为了通信

- 修改请求的域名为子应用的真实域名

- 

- - 所以子应用需要能支持跨域

- 解析子应用的入口html

- 

- - 识别出html 部分，分离style 和js
  - 处理css 重新注入html （有插件系统，可以对子应用的css定义）
  - 创建 webComponent 并挂载 HTML

- 

- - CSS 由于在 shadowDOM 内，样式也不会影响到外部，也不会受外部样式影响。

- - 

- **「创建」** **「script」** **「标签，并插入到 iframe 的 head 中」**

- 对 iframe 的 document.querySelector 进行改造，需要劫持 document**「改为从」** **「shadowRoot」** **「里面查找」**，才能**「使 Vue 组件能够正确找到挂载点」**

### micro app

> ❝
>
> micro-app并没有沿袭single-spa的思路，而是借鉴了WebComponent的思想，通过CustomElement结合自定义的ShadowDom，将微前端封装成一个类WebComponent组件，从而实现微前端的组件化渲染。并且由于自定义ShadowDom的隔离特性，micro-app不需要像single-spa和qiankun一样要求子应用修改渲染逻辑并暴露出方法，也不需要修改webpack配置，是目前市面上接入微前端成本最低的方案。、
>
> ❞

它在 **「基座应用」** 和 **「子应用」** 之间充当桥梁胶水的作用。

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)![1717910101571](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1717910101571.png)

#### 接入方式

```js
import microApp from '@micro-zoe/micro-app';
microApp.start();

export function MyPage () {
  return (
  <div>
    <h1>子应用</h1>
    <micro-app
      name='app1' // name(必传)：应用名称
      url='http://localhost:3000/' // url(必传)：应用地址，会被自动补全为http://localhost:3000/index.html
      baseroute='/my-page' // baseroute(可选)：基座应用分配给子应用的基础路由，就是上面的 `/my-page`
      ></micro-app>
  </div>
  )
}
```

#### 加载子应用过程

microApp.start() 后，会注册一个名为 micro-app 的自定义 webComponent 标签。

通过 fetch 拿到 url 对应的 html 字符串，然后替换 head 和 body 标签为自定义标签，避免污染主应用**「micro-app-head micro-app-body」**

```js
htmlstr.replace(/<head/i, '<micro-app-head')
htmlstr.replace(/<body/i, '<micro-app-body')
```

处理link标签

- 处理 href 属性，在原本的 href 的前面拼接上 app.url ，相对路径改绝对路径
- 若为样式链接，ref 的属性是 stylesheet，删除该link,记录href内容，创建一个style标签插入 `<micro-app-head>`
- 创建style标签时会，给子应用的style标签添加作用域，实现样式隔离

处理style 标签

- 给子应用的 style 标签加上作用域，前缀是 ${microApp.tagName}[name=xxx]

```js
例如：
.test { height: 100px; }

添加前缀后变为：
micro-app[name=xxx] .test { height: 100px; }
```

处理script 标签

- src 属性上拼接app.url，重新加载script 标签，将其内容保存下来。

挂载子应用

- 当处理完html 后，将之前处理过的 html 内容放入 webComponent 容器 () 中

绑定沙箱

- 元素隔离，拦截document 对象，当寻找根元素时，判断当前的appName
- js 隔离，对window 对象做了一层代理

#### 隔离

MicroApp借鉴了qiankun的js沙箱和样式隔离方案，这也是目前应用广泛且成熟的方案。

## 微前端蓝图

新团队组建后，首先不得不完成很多设置工作，例如创建一个基本的应用程序，梳理构建流程以及其他繁琐的任务

而共用前端蓝图这一概念能够帮助我们解决上述问题。蓝图实际上是一个示例项目，其中包括微前端项目需要的所有重要部分。可以将蓝图划分为两大类：技术和项目细节

### 技术细节

- 目录结构
- 测试（单元测试，端到端测试）
- 代码检查以及格式化规则
- 代码风格
- API通信
- 性能的最佳实践（优化静态文件）
- 编译工具配置

上述这些方向是所有项目都必须要考虑的，但并不具有很大的挑战性。大多数主流框架都提供了脚手架工具，能为你生成一个示例项目，但是对于一个团队来说，仅使用默认的前端配置是远远不够的。

### 项目细节

你的前端代码需要和其他团队进行整合，并且整合必须遵循整体架构的指南。全新的前端项目必须要考虑项目的一些细节。因此，我们的蓝图还应包括：

- 组合示例

- - 接入其他微前端的示例
  - 令你的微前端可以被接入的示例

- 通信示例

- 如何为团队设置CSS和URL前缀

- 微前端相关文档的模版

- 如何整合托管在中心化服务中的库

- 如何引入本地的库

- 如何开发通用服务，如异常跟踪，分析等

- CI/CD流程

新加入团队可以复制上面的蓝图，根据实际需要稍加改动，即可变成他们自己的蓝图。基于现有蓝图进行开发能够大大的节约时间。

## 参考链接

https://www.modb.pro/db/610768

https://qiankun.umijs.org/zh/cookbook

https://microfrontends.cn/https://zhuanlan.zhihu.com/p/378346507

https://zhuanlan.zhihu.com/p/442815952

https://juejin.cn/post/7215967453913317434

https://juejin.cn/post/7125646119727529992#heading-5

https://segmentfault.com/a/1190000040462400