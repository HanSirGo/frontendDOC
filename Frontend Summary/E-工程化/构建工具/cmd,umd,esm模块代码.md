# 一套代码如何同时搞定cmd，umd，esm模块代码？

在日常开发中，我们难免会遇到一些重复的工作，例如我们编写的工具函数，可能多个项目之间都有可能使用到，就好像我写的这个脚手架一样，在脚手架中使用到的工具函数和在 webpack 配置中使用到的工具函数是一样的，但是他们一个是使用 cjs 模块开发的，一个是使用 esm 模块开发的，这样的话，我们就需要有一个东西可以使我们在编写工具类库的同时，将一份代码编译成多个不同的模块。

# **什么是 Rollup**

Rollup 是一个 JavaScript 模块打包器，主要用于将小块代码编译成更大且更复杂的结构，如库或应用程序。它非常适合用于构建 JavaScript 库，因为它可以生成更小、更高效的包。

Rollup 主要有一下几个特点：

1. ES Module 格式支持：Rollup 最初是为了支持 ES6 模块（即 ES Module）而设计的。它能将多个 JS 文件打包成一个文件，同时保持 ES6 模块的结构。
2. Tree Shaking：这是 Rollup 的一个显著特点。Tree Shaking 指的是移除未使用的代码，从而减小最终打包文件的体积。这在创建轻量级库时非常有用。
3. 简洁的输出：相比于其他打包器如 Webpack，Rollup 生成的包通常更小、更简洁。这是因为 Rollup 更专注于生成 JavaScript 库和应用程序，而不是处理复杂的应用程序资产。
4. 插件生态系统：Rollup 支持插件，这意味着你可以添加额外的功能来处理不同类型的文件、转换代码或执行其他构建任务。

Rollup 由于其独特的特性和优势，适合于多种应用场景，尤其是在 JavaScript 和 Web 开发领域。通常被应用与 JavaScript 库和框架开发、组件库的打包和发布 NPM 模块。

# **项目初始化**

首先，创建你的项目文件夹并初始化 package.json 文件：

```
mkdir momentcd momentpnpm init
```

安装 TypeScript 和 Rollup 相关依赖：

```
pnpm add typescript rollup @rollup/plugin-typescript tslib --save-dev
```

相关依赖安装完成之后，我们在根目录下创建并配置 tsconfig.json 文件：

```
{  "compilerOptions": {    "outDir": "./dist",    "module": "ESNext",    "target": "es5",    "declaration": true,    "strict": true  },  "include": ["src/**/*"],  "exclude": ["node_modules", "dist"]}
```

继续在根目录下创建一个 rollup.config.js 文件用于配置 Rollup：

```
import typescript from "@rollup/plugin-typescript";export default {  input: "src/index.ts", // 你的主入口文件  output: [    {      file: "dist/moment.cjs.js",      format: "cjs",    },    {      file: "dist/moment.esm.js",      format: "esm",    },    {      name: "Moment",      file: "dist/bundle.umd.js",      format: "umd",      globals: {},    },  ],  plugins: [typescript()],  external: [],};
```

这些代码都编写完成之后，我们要在 package.json 文件中添加这样的一段代码，如下：

```
 "scripts": {    "build": "rollup -c"  },
```

当这些代码编辑完成之后，我们在根目录下创建一个 src 目录并创建一个 index.ts 文件并编写以下代码：

```
export default function Moment<T>(m: T): T {  return m;}
```

在终端中输入以下命令：

```
pnpm build
```

最终代码输出的结果如下图所示：

![1709971086800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971086800.png)

**总结**

通过这些步骤，你可以利用 Rollup 构建一个同时支持 CJS, ESM 和 UMD 格式的 JavaScript 库，确保其在不同环境中的兼容性。

除了 Rollup 之外，我们还可以使用 webpack，typescript 来实现相同的功能。