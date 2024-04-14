# Webpack to Vite

### **Webpack开发环境运行原理**

1、读取`webpack`的配置参数；

2、启动`webpack`，创建`Compiler`对象并开始解析项目；

3、从入口文件（`entry`）开始解析，并且找到其导入的依赖模块，递归遍历分析，形成依赖关系树；

4、对不同文件类型的依赖模块文件使用对应的`Loader`进行编译，最终转为`Javascript`文件；

5、根据入口和模块之间的依赖关系，组合成一个个包含多个模块的`Chunk`，再把每个`Chunk`转换成一个单独的文件加入到输出列表。

6、`Webpack`根据配置，确定输出的路径和文件名，把文件内容写入到文件系统，文件内容包括且不限于

- 打包后的 JavaScript 文件
- HTML 文件
- CSS 文件
- 其他资源文件

> 这些文件都是在内存中生成的，目的是为了加快开发过程中的构建和重载速度。这意味着你不会在你的项目目录中看到这些文件，但它们可以通过 Webpack dev server 提供的开发服务器访问。这种方式使得开发更加高效，因为内存中的读写速度远快于磁盘。

![1712994804788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994804788.png)image.png

### **Vite开发环境运行原理**

1. 运行 vite 命令，Vite 会启动一个本地开发服务器。这个服务器负责提供静态文件服务、处理 HTTP 请求等
2. 当浏览器请求一个页面时，Vite 开发服务器会拦截这些请求。对于 HTML 页面请求，服务器会处理并返回 HTML 内容![1712994819979](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994819979.png)
3. 对于 JavaScript 模块的请求，Vite 会解析这个模块的依赖关系。如果模块是一个第三方库（位于 node_modules 中），Vite 会优先检查是否有预构建的缓存，如果没有，会对该库进行预构建并缓存，以提高后续加载速度。![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994837140.png)
4. Vite 会按需编译请求的模块。这意味着只有当浏览器实际请求某个模块时，Vite 才会编译这个模块
5. Vite 支持模块热更新，当源代码发生变化时，Vite 会快速重新编译改动的模块，并通过 WebSocket 推送更新到浏览器

> 整个流程的核心在于利用现代浏览器支持的 ES 模块特性，以及按需编译和模块热更新技术，从而实现快速的开发体验。Vite 的这种设计避免了传统前端工具在开发模式下需要打包整个应用的繁琐过程，显著提高了开发效率

![1712994847894](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994847894.png)image.png

#### **关于预构建**

vite使用esbuild进行预构建，主要做了两件事情：

- 将非 `ESM` 规范的代码转换为符合 `ESM` 规范的代码；
- 将第三方依赖内部的多个文件合并为一个，减少 `http` 请求数量；（lodash-es）

![1712994864445](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994864445.png)

使用esbuild构建依赖的原因：

- esbuild是用Go语言编写的，而webpack是用JavaScript编写的。Go语言是一种编译型语言，执行效率高于解释型语言JavaScript。
- esbuild利用Go语言的并发特性，可以在多个CPU核心上并行处理文件，这大大提高了构建速度。

### **热更新的实现方式**

#### **webpack热更新**

1. 启动开发服务器，并启动本地服务端的websocket服务

![1712994921476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994921476.png)image.png

1. 修改entry入口配置，将websocket客户端代码，和热更新代码一起打包到bundle文件中去

![1712994934304](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994934304.png)image.png

1. 监听文件变化，文件变化时重新编译，编译结束后通过websocket给浏览器发送通知
2. 浏览器接收到通知后，利用上次保存的hash值xxx，发送xxx.hot-update.json的ajax请求，获取热更新模块以及下次热更新的hash标识
3. 请求xxx.hot-update.js的代码，并且立即执行，完成热更新

![1712994946685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994946685.png)image.png

#### **vite热更新**

1. 启动开发服务器，并启动本地服务端的websocket服务

![1712994969057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994969057.png)image.png

1. 修改index.html代码，插入`<script type="module" src="/@vite/client"></script>`，启动websocket客户端服务![1712994980256](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994980256.png)
2. 监听文件的更改，进行处理和判断通过`WebSocket`给客户端发送消息通知客户端去请求新的模块代码。![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712994991130.png)
3. 浏览器接收到通知后，重新请求热更新的模块![1712995003136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712995003136.png)

## Webpack to Vite

迁移工作我们使用webpack-to-vite插件进行迁移

### **插件使用**

```js
$ npx @originjs/webpack-to-vite <project path>
```

or

```js
$ npm install @originjs/webpack-to-vite -g
$ webpack-to-vite <project path>
```

根据官方文档进行操作即可。

### **踩坑汇总**

- 无法正确解析vue文件：script代码使用了jsx语法的.vue文件需要在`<script>`标签后面增加`lang="jsx"`

```js
// index.vue

<script lang="jsx">
<script>
```

- 无法正确解析js文件：用到jsx的js文件需要把后缀修改成jsx
- 具名导入报错：`export default`导出的对象无法被具名导入，需要手动具名导出

```js
// index.js
export default {
  code: 1
}

import { code } from 'index' // 会导致报错
```

- require报错：vue文件中不要再使用require对静态资源进行请求

```js
<!-- bad -->
<img :src="require('image01.png')"/>

<!-- good -->
<img src="image01.png"/>

<!-- bad -->
const imgSrc = {
  403: require('image403.png'),
  404: require('image404.png'),
  500: require('image405.png')
}

<!-- good -->
import Page403 from 'image403.png'
import Page404 from 'image404.png'
import Page500 from 'image405.png'

const imgSrc = {
  403: Page403,
  404: Page404,
  500: Page500
}
```

- cjs语法报错：使用标准esm语法

```js
// bad
module.exports = {
  code: 1
}

// good
export default {
  code: 1
}
```

- 自引用组件报错：使用动态引入

```js
// Button.vue
export default {
    name: 'Button',
    components:{
        // 动态引入
        Button: ()=>import('.../Button')
    }
}
```

- 修改node_module不生效：需要重新触发预构建

1. 删除 node_modules/.vite 目录
2. 使用 Vite 的命令行选项：vite --force
3. optimizeDeps.force，设置为 `true` 可以强制依赖预构建