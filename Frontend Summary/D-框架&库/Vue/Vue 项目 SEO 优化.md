# Vue 项目 SEO 优化的关键

最近在一个 Vue 项目中，发现了许多在开发前、中、后期需要特别注意的细节，以确保性能和 SEO 的最佳结合。

本文将聊聊 Vue 项目要做 SEO 优化方向侧的一些相关内容

# **什么是 SEO，为什么它如此重要？**

`SEO（Search Engine Optimization）`，即搜索引擎优化，是指通过优化网站的内容、结构和技术，使其在搜索引擎的自然搜索结果中获得更高的排名，从而增加网站的**曝光率和流量**。SEO 涉及多方面的优化措施，包括关键词研究、内容创作、技术优化、用户体验提升等，目标是让搜索引擎更容易理解和评估你的网站内容。

我们耳熟能详的搜索引擎包括 Google、百度、360 搜索等。

![1725190280071](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725190280071.png)

# **Vue 项目中的 SEO 挑战**

Vue.js 是一个非常受欢迎的前端框架，凭借其灵活性和高效性赢得了广泛的开发者喜爱。然而，`Vue 的默认渲染方式是客户端渲染（CSR）`，即页面内容是在浏览器加载后通过 JavaScript 渲染出来的。这种渲染方式虽然提升了交互性，但对 SEO 造成了很大的挑战。

要提升搜索引擎，先了解下搜索引擎的大致工作原理是什么。进而来寻找可以从哪些方面入手。

### **搜索引擎的工作原理**

搜索引擎使用爬虫程序（例如 360 搜索的“360 蜘蛛”）抓取和索引互联网上的内容。这些爬虫会自动扫描网页、跟踪链接并更新索引，以帮助提供准确的搜索结果。为了确保你的内容能够被这些爬虫正确抓取和索引，需要对网站的结构、内容和技术进行优化。

大致流程：

```
抓取互联网上面的网站/网页数据 -> 存储记录到数据库中做预处理（网页数据库、索引数据库）-> 分析用户搜索内容 -> 展示搜索结果
```

(这里不展开去讲，每个部分都需要十分复杂的程序去进行筛选、处理）

![1725190292526](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725190292526.png)

### **Vue 项目中的 SEO 优化方向**

Vue本身的脚手架是解决不了SEO的一些问题的，例如：打包后的产物只有一个index.html文件，并且发送给浏览器时，该页面只有一个`id = app` 的`dom`,并且里面的html内容为空。

- 客户端渲染的问题

Vue 的客户端渲染（CSR）意味着页面内容通过 JavaScript 在浏览器中动态加载。搜索引擎爬虫在抓取页面时可能无法执行 JavaScript，因此可能无法看到和索引动态加载的内容。为了解决这个问题，我们可以考虑以下优化策略：

1. `预渲染（Prerendering）`: 在构建时生成静态 HTML 文件，确保搜索引擎爬虫可以抓取并索引所有页面内容。可以使用 `prerender-spa-plugin` 插件来实现这一点。
2. `服务端渲染（SSR）`: 在服务器端生成完整的 HTML 内容，然后将其发送到客户端，从而确保页面在加载时已经包含了所有内容。`Vue 提供了 Nuxt.js` 这一框架，专门用于实现服务端渲染。

- 页面初始加载速度

首屏内容加载慢可能导致页面在搜索引擎中的排名下降。因为需要等到 JavaScript 执行完毕，页面内容才会显示。搜索引擎通常偏好加载速度快、用户体验好的网页。通过服务端渲染（SSR）或预渲染，可以显著提升页面的加载速度，提供更好的用户体验。

- 动态 Title、Meta 标签的优化

为了确保搜索引擎能够正确抓取页面的 Meta 信息，可以使用 `vue-meta-info` 插件动态设置页面的 `title、meta description 和 meta keywords`。

- 链接和路由

Vue 项目通常是单页应用，所有页面内容通过 JavaScript 动态加载。这种路由方式可能不够友好，尤其是当页面内容需要通过 Ajax 请求来加载时，这可能影响搜索引擎的索引。

**因此在 Vue 项目中进行 SEO 优化时，服务端渲染（SSR）和预渲染（Prerendering）是两个重要的技术手段。**

### **预渲染（Prerendering）**

预渲染（Prerendering）是在构建时生成静态 HTML 文件的过程。与 SSR 不同，预渲染是在**构建阶段生成静态页面**，而不是在每次请求时动态生成。这些静态 HTML 文件可以直接提供给搜索引擎爬虫和用户浏览器，确保内容被正确索引。

可以借助 `prerender-spa-plugin` 插件进行优化,在`config.js`文件引入即可，这是官网给出来的一个简单的基础demo，还有很多配置可以去官网进行查看。

在这需要注意的是，项目中有多少个路由，就需要在`routes`配置中配置几个路由，最终的产物形态会把配置的路由打包成多页的形式产出。

```
const path = require('path')
const PrerenderSPAPlugin = require('prerender-spa-plugin')

module.exports = {
 publicPath: './',
 configureWebpack:{
 plugins: [
 ...
 new PrerenderSPAPlugin({
  // Required - The path to the webpack-outputted app to prerender.
  staticDir: path.join(__dirname, 'dist'),
  // Required - Routes to render.
  routes: [ '/', '/about', '/some/deep/nested/route' ],
  })
 ]}
}
```

解决动态的渲染 title、meta的相关信息 `vue-meta-info`

```
import MetaInfo from 'vue-meta-info'
Vue.use(MetaInfo)
```

在需要动态变更的页面中加入：

```
<template>
...
</template>

<script>
export default {
 metaInfo: {
 title: 'My Example App', // set a title
 meta: [ // set meta
  {
   name: 'Home',
   content: 'Hello Word!!!'
  },
  {
   name: 'content',
   content: 'hi 我是content🤝'
  }
 ]
 link: [{ // set link
  rel: 'asstes',
  href: 'https://xxx/'
 }]
  }
}
</script>
```

- 改善 SEO: 静态 HTML 文件可以确保搜索引擎爬虫能够抓取并索引所有页面内容，无需执行 JavaScript。
- 较低的服务器负担: 预渲染是在构建时完成的，服务器在运行时只需提供静态 HTML 文件，减轻了服务器的负担。

预渲染的配置通常比 SSR 更简单，特别适合内容较少或内容不频繁更新的页面。

如果页面内容频繁更新，可能需要定期重新生成静态 HTML 文件，以确保内容的最新性。

**缺点：**

**1. 对于需要频繁动态更新的内容，预渲染可能不是最佳选择，因为它生成的是静态页面。**

**2. 有非常多的页面，例如商品页，需要seo，预渲染也是不适合的**

**3. 动态的更新title、描述、关键字 无效**

### **服务端渲染（SSR）**

服务端渲染（SSR）指的是在服务器端生成完整的 `HTML` 内容，并将这些内容发送到客户端。这意味着，用户在访问页面时，服务器会先处理和渲染所有的内容，然后将渲染后的 `HTML` 发送给用户浏览器。这种方式使得页面在加载时已经包含了全部的内容，浏览器只需显示这些内容，而不需要等待 `JavaScript` 完全执行。

Vue有一套自己的服务端渲染框架 - `Nuxt.js`

`Nuxt.js` 是 `Vue` 的一个框架，专门设计用于实现服务端渲染。它为 `Vue` 应用提供了 `SSR` 支持，并自动处理许多配置和优化任务。

`Vue Server Renderer`: `Vue` 也提供了官方的服务端渲染工具包——`vue-server-renderer`，允许开发者手动实现 `SSR`，适合需要高度定制的项目。

按照官网上安装即可：

```
npx create-nuxt-app nuxtdemo
```

列表结构：

![1725190332055](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725190332055.png)

`Nuxt.js` 会依据 pages 目录中的所有 *.vue 文件生成应用的路由配置。Nuxt.js 依据 pages 目录结构自动生成 `vue-router` 模块的路由配置。

```
假设 pages 的目录结构如下：
pages/
--| user/
-----| index.vue
-----| one.vue
--| index.vue

那么，Nuxt.js 自动生成的路由配置如下：
router: {
 routes: [{
  name: 'index',
  path: '/',
  component: 'pages/index.vue'
 },
 {
  name: 'user',
  path: '/user',
  component: 'pages/user/index.vue'
 },
 {
  name: 'user-one',
  path: '/user/one',
  component: 'pages/user/one.vue'
 }]
}
```

不仅在`pages`下面自动生成的路由配置，在创建组件时，只需将组件放入到`component`下即可，引用时无需导入直接创建。

可以参考官方文档直接调取里面的API，因为是Vue深度结合的，用起来也是极为方便的。

服务端渲染解决了上面所说的预渲染的一些问题，并且 `Nuxt`可以很好的结合`Vue`项目做SSR:

- 提高 SEO 性能: 搜索引擎爬虫可以直接抓取服务器端生成的 HTML 内容，确保所有页面都能够被索引。
- 提升加载速度: 由于服务器发送的是完整的 HTML 内容，用户可以更快地看到页面内容，改善用户体验。
- 更好的用户体验: 初始页面加载速度较快，用户可以更快地看到和交互内容，特别是对于内容丰富的页面。

不足：

- 服务端渲染增加了服务器的负担，因为服务器需要处理渲染任务
- 实现 SSR 需要处理更多的服务器配置和开发细节，可能会增加开发和维护的复杂性
- 如果是一个较为成熟的vue项目，后期引入`Nuxt`，学习成本和接入的成本较高

vue项目可以使用Nuxt.js，Nuxt.js是使用 Webpack 和 Node.js 进行封装的基于Vue的SSR框架，使用它，你可以不需要自己搭建一套 SSR 程序，而是通过其约定好的文件结构和API就可以实现一个首屏渲染的 Web 应用。

如果是项目开始的时候，就知道要做SEO，建议使用比较成熟的SSR框架

```
基于Vue的 Nuxtjs
基于React的 Nextjs
```

### **相关信息：**

nuxt官网 https://www.nuxtjs.cn/