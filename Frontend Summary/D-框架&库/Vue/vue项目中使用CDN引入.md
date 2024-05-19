# vue项目中使用CDN引入

前言
CDN（内容分发网络）指请求资源的方式，即通过script头去请求对应的脚本资源的一种方式，项目里配置之后不需要通过npm包管理工具去下载配置的包。
目的：将引用的外部js、css文件剥离开来，不编译到vendor.js中，而是用资源的形式引用，这样浏览器可以使用多个线程异步将vendor.js、外部的js等加载下来，达到加速首页展示效果。

1. 在vue.config.js进行配置
本人对vue、vuex、vue-router、axios、element-ui、echarts进行了cdn引用。（请求element-ui、echarts的cdn较慢）

```js
//生产环境标记
const IS_PRODUCTION = process.env.NODE_ENV === 'production'
//配置引用cdn的js、css地址
const cdn = {
    css: [
        'https://unpkg.com/element-ui@2.13.2/lib/theme-chalk/index.css'
    ],
    js: [
        'https://cdn.bootcdn.net/ajax/libs/vue/2.6.10/vue.min.js',
        'https://cdn.bootcdn.net/ajax/libs/vue-router/3.0.2/vue-router.min.js',
        'https://cdn.bootcdn.net/ajax/libs/vuex/3.1.0/vuex.min.js',
        'https://cdn.bootcdn.net/ajax/libs/axios/0.18.1/axios.min.js',
        'https://unpkg.com/element-ui@2.13.2/lib/index.js',
        'https://cdn.bootcdn.net/ajax/libs/echarts/5.0.1/echarts.min.js'
    ]
}
//配置打包时使用CDN节点（加入externals外部扩展）， 忽略打包的第三方库
//左面放package.json中的扩展的名称,右面放项目依赖的名称(项目初始化要用的名称)
const externals = {
  // 属性名称 vue, 表示遇到 import xxx from 'vue' 这类引入 'vue'的，不去 node_modules 中找，而是去找全局变量 Vue（其他的为VueRouter、Vuex、axios、ELEMENT、echarts，注意全局变量是一个确定的值，不能修改为其他值，修改为其他大小写或者其他值会报错，若有异议可留言）
    vue: 'Vue',
    'vue-router': 'VueRouter',
    vuex: 'Vuex',
    axios: 'axios',
    'element-ui': 'ELEMENT',
    'echarts': 'echarts'
}
chainWebpack(config) {
        if (IS_PRODUCTION) {
            config.plugin('html').tap(args => {
                args[0].cdn = cdn
                return args
            })
            //视为一个外部库，而不将它打包进来
            config.externals(externals)
        }
    }
```

2.在public/index.html文件配置
使用 webpack中自带的插件 html插件进行配置，在 index.html 中增加判断，是否使用 CDN， htmlWebpackPlugin.options 使用的是vue.config中的config.plugin('html')的插件属性。

```html
  <!-- 使用CDN的CSS文件 -->
     <% for (var i in
     htmlWebpackPlugin.options.cdn&&htmlWebpackPlugin.options.cdn.css) { %>
     <link href="<%= htmlWebpackPlugin.options.cdn.css[i] %>" rel="preload" as="style" />
     <link href="<%= htmlWebpackPlugin.options.cdn.css[i] %>" rel="stylesheet" />
   <% } %>
    <!-- 使用CDN加速的JS文件，配置在vue.config.js下 -->
   <% for (var i in
     htmlWebpackPlugin.options.cdn&&htmlWebpackPlugin.options.cdn.js) { %>
     <script src="<%= htmlWebpackPlugin.options.cdn.js[i] %>"></script>
   <% } %>
```

### 3.易出错点

1、Router is not defined

![1715511862614](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1715511862614.png)

![1715511875052](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1715511875052.png)

![1715511886251](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1715511886251.png)

解决方案： 将Router 改为 ‘VueRouter’

2、Uncaught TypeError: Illegal constructor

![1715511904817](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1715511904817.png)

 解决方案：修改externals 中‘'[element-ui](https://so.csdn.net/so/search?q=element-ui&spm=1001.2101.3001.7020)’的value

```javascript
const externals = {    
    'element-ui': 'ELEMENT',
}
```