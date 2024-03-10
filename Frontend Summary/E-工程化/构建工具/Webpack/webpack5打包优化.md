# webpack5 打包优化

## 问题背景

目前项目开发较久文件较多，线上打包时间在有插件更新情况下超过 23min,平均时间在 20min,测试反应线上构建时间过长，急需优化（主要是缩短打包时间）

## 优化前的思考

本次优化的目标主要是针对构建时间，提升打包速度，打包速度的提升方向主要是在并行构建和合理利用缓存等方面进行入手 本次使用的 webpack 版本为 5.64.4。

## 优化准备

安装打包速度分析插件 speed-measure-webpack-plugin，使用插件分析打包时间

```
const SpeedMeasurePlugin = require('speed-measure-webpack-plugin'); // 打包速度分析
```

分析发现 在本地打包情况下 用时 8-10min 左右

## 优化方案

### 首次优化

**并行构建**

webpack 开启并行构建可以同时处理多个任务，提升构建效率 代码并行构建常用的是 HappyPack 和 Thread-loader，由于 HappyPack 官方已经不再维护了，上一次维护还是 5 年前，所以这次在多进程这方面采用的是与其类似的 Thread-loader。

Thread-loader 相较于 HappyPack 上手更为容易，只要把这个 loader 放在需要处理的其他 loader 之前使用就可以了。它是会创建多个 worker 来进行控制的，相当于把在它之后的 loader 处理放进一个独立的 worker 里面去执行。

添加多进程 Thread-loader

```js
module.exports = {
  module: {
    rules: [
      {
        test: /.(js|mjs|jsx|ts|tsx)$/,
        use: [
          // 开启多进程打包。
          {
            loader: "thread-loader", // 需要提前安装thread-loader插件
            options: {
              workers: 3, // 进程3个
            },
          },
          {
            loader: "babel-loader",
          },
        ],
      },
    ],
  },
};
```

**需要注意的是，每个 worker 的开启都是需要一定的时间的，并不是开启的越多越好，只有在工程量大，代码量多的时候是会有明显提升的，所以使用前需要多加斟酌。**

**开启 cache 缓存，缓存 babel 编译，js 解析等。**

webpack5 的 cache 属性，可以设置为 filesystem 进行缓存自定义的配置

没有缓存配置的 loader，可以使用 cache-loader 进行缓存，同样也是把它放在需要处理的 loader 之前使用即可。

```js
module.exports = {
  module: {
    rules: [
      {
        test: /.(js|mjs|jsx|ts|tsx)$/,
        use: ["cache-loader", "babel-loader"],
      },
    ],
  },
};
```

优化效果

- **本地打包用时 6min,线上打包用时 15min**
- **对比原线上打包时间 20min,提升 25%**

### 二次优化

对于优化后的效果感觉不够明显，又进行了二次优化

多次数度分析发现其中 source-map 和 terser-webpack-plugin 用时最久

针对这一情况进行优化，选择在开发环境及测试环境关闭 source-map（极大的减小体积及部分减少用时），关闭 terser-webpack-plugin（代码压缩混淆，极大的减少用时，会增加小部分体积）

我是在 package.json 中对不同环境利用 cross-env 传递变量，控制对不同环境的 source-map 和 terser-webpack-plugin 是否使用。

```
 "build-dev": "cross-env DISABLE_TerserPlugin=true GENERATE_SOURCEMAP=false……"
```

在 webpack 配置中使用变量进行控制，对于需要使用 TerserPlugin 的情况，设置 parallel 为 true 可以开启并行压缩，提高效率。

```js
 optimization: {
   minimize: !disableTerserPlugin ? true : false,
   minimizer: [
     new TerserPlugin({
       parallel: true, // 启用并行压缩
       exclude: /node_modules/,
       terserOptions: {
         ……
       }
     }),
   ],}
```

优化效果

- **本地打包时间用时 2min,线上打包用时在有插件下载情况下 4min30s 左右，平均用时 2min10s 左右， 体积从 440M 降低到 345M**
- **时间整体对比初始提升 90%，体积缩小 21.6%**

## 对比效果图

### 首次优化对比

![1709980975294](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709980975294.png)首次打包优化对比 .png

### 最终优化对比

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709980990150.png)打包优化终版.png

## 对于采用控制 TerserPlugin 的思考

由于我们目前的项目是经常构建 dev 和 fat 环境，线上环境构建次数相对较少，不那么频繁，发版基本是按月发版的，所以真正影响构建工作效率的是 dev 和 fat 两个环境，所以采用了禁用 TerserPlugin 的方法去减少它的构建用时，但是真正到线上环境还是需要开启 TerserPlugin 进行代码压缩的。所以对于是否可以采用这种类似方法去适配其他项目，小伙伴们需要因地制宜，仔细考虑是否合适。

## 后序

- 对于本次的优化结果，测试小伙伴表示非常满意，极大的提高了构建速度，减少了他们构建时需要等待的时间！
- 本次优化方案的遗憾之处是没有找到更好的办法去各个环境都能达到一个满意的构建速度，比如线上环境，如果小伙伴们有更好的办法，欢迎提出！
- 本文是作者对于个人的学习与总结笔记，如果有谬误的地方，欢迎各位提出并指正~