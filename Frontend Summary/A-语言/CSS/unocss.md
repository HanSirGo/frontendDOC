# 原子CSS引擎——unocss

![1713073754464](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713073754464.png)

- unocss是一个即时的原子CSS引擎，它可以让你用简短的类名来控制元素的样式，而不需要写复杂的CSS代码。
- 当然，原子样式也有很多选择，最著名的就是 Tailwind。但由于Tailwind 会生成大量样式定义，会导致全量的 CSS 文件往往体积会多至数 MB，从而有性能上有一些不足

![1713073792820](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713073792820.png)

## unocss的优点

- 它可以让你快速地开发和原型设计，而不需要考虑CSS的细节。
- 它可以让你的CSS文件更小，因为它只生成你用到的工具类。
- 它可以让你的CSS更一致，因为它遵循一套预设的规则和变量。
- 它可以让你的CSS更灵活，因为它支持自定义工具类，变体，指令和图标。
- 它可以让你的CSS更易于维护，因为它避免了样式冲突和重复代码。

## 项目中使用 unocss

参考 官方文档Vite配置，webpack 往下翻即可

> https://unocss.dev/integrations/vite

### 安装

```
npm install -D unocss
```

### 打包配置

#### vite配置

```js
// vite.config.js
import UnoCSS from 'unocss/vite'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    UnoCSS(),
  ],
})
```

#### webpack5中配置

```js
// webpack.config.js
const UnoCSS = require('@unocss/webpack').default

module.exports = {
  plugins: [
    UnoCSS(),
  ],
  optimization: {
    realContentHash: true,
  },
}
```

#### webopack4中配置

```js
// webpack.config.js
const UnoCSS = require('@unocss/webpack').default

module.exports = {
  plugins: [
    UnoCSS(),
  ],
  css: {
    extract: {
      filename: '[name].[hash:9].css',
    },
  },
}
```

### 创建一个`uno.config.ts 配置文件

- 用于编写用户想要的 unocss 配置

```js
// uno.config.js
import { defineConfig } from 'unocss'

export default defineConfig({
  // ...UnoCSS options
})
```

### 全局引入

```js
 // main.js
 import 'virtual:uno.css';
```

### 常见用法

- 对应刚入手 unocss 的同学来说，规则怎么写是不知道的，但不用着急，官方（大佬 antfu）给出了 交互式文档，输入你想要的css样式，就可以获得对应的class名称

> https://www.tailwindcss.cn/docs/font-family(中文文档)
>
> https://unocss.dev/interactive/(官方文档)

![1713074037175](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713074037175.png)

### 常见基础用法

> unocss支持用预设单位（预设单位是rem），也可以自定义单位

![1713074067268](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713074067268.png)

### 自定义规则

- 在vscode中我们都会有用到快捷键一键生成代码，unocss也不例外，假如你有个盒子，里面的内容需要垂直居中，那么就可以定义为：

```js
export default defineConfig({
  shortcuts: [
   {'flex-center': 'flex items-center justify-center',}, // 静态快捷方式，是一个对象，可配多个
  ]
})
```

- 当然，也可以精选 动态配置快捷方式

```js
export default defineConfig({
 shortcuts: [
    [/^base-border-(.*)$/, match => `border-1 border-style-dashed border-${match[1]}`], // 动态快捷方式，一个配置为一个数组
  ]
})
```

### 安装vscode插件

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713074129953.png)

- 安装启用后，页面上就能看出哪些 class 使用 unocss 提供 （带有虚线），并且能显示类名对应的样式内容，如下：

![1713074146310](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713074146310.png)