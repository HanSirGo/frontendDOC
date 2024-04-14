# Webpack 的 Code Splitting 代码分割

Webpack 默认会将尽可能多的模块代码打包在一起，优点是能减少最终页面的 HTTP 请求数，但缺点也很明显：

- 页面初始代码包过大，影响首屏渲染性能
- 将所有资源达成一个包后，即使只是修改了一个字符，客户端都需要重新下载整个代码包，缓存命中率极低

在 webpack 中，我们只需要配置多入口打包和动态引入，以及防止重复（入口依赖/splitChunksPlugin）就可以简单的实现代码分包

- 入口起点：使用 entry 配置手动地分离代码。
- 防止重复：使用 入口依赖 或者 SplitChunksPlugin 去重和分离 chunk。
- 动态导入：通过模块的内联函数调用分离代码。

## **多入口**

```js
 const path = require('path'); module.exports = {  // entry: './src/index.js',  mode: 'development',  entry: {    index: './src/index.js',    main: './src/main.const path = require('path');

 module.exports = {
  // entry: './src/index.js',
  mode: 'development',
  entry: {
    index: './src/index.js',
    main: './src/main.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
};js',  },  output: {    filename: '[name].bundle.js',    path: path.resolve(__dirname, 'dist'),  },};
```

构建结果如下：

```js
...
[webpack-cli] Compilation finished
asset index.bundle.js 553 KiB [emitted] (name: index)
asset main.bundle.js 553 KiB [emitted] (name: main)
runtime modules 2.49 KiB 12 modules
cacheable modules 530 KiB
  ./src/index.js 257 bytes [built] [code generated]
  ./src/main.js 84 bytes [built] [code generated]
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
webpack 5.4.0 compiled successfully in 245 ms
```

这样就可以简单的解决文件多大的问题，但这种方法的能力有限，并且有很大的局限性

- 如果入口 chunk 之间包含一些重复的模块，那么这些重复模块会被引入到各个 bundle 中
- 这种方法不够灵活，并且不能动态地拆分应用程序逻辑中的核心代码。

以上两点中，第一点所对应的问题已经在我们上面的实例中体现出来了。我们在 ./src/main.js 和 ./src/index.js 中都引入过 lodash，我们可以发现打出来的文件都是 553 Kib，不难得出里面都含有lodash的代码，也侧面放映出 lodsh.js 重复引用了

## **防止重复引用**

### **入口依赖**

在配置文件中配置 dependOn 选项，以在多个 chunk 之间共享模块：

```js
const path = require('path');

 module.exports = {
   mode: 'development',
   entry: {
    index: {
      import: './src/index.js',
      dependOn: 'shared',
    },
    main: {
      import: './src/main.js',
      dependOn: 'shared',
    },
    shared: 'lodash',
   },
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
 };
```

如果想要在一个 HTML 页面上使用多个入口起点，还需设置 optimization.runtimeChunk: 'single'

```js
const path = require('path');

 module.exports = {
   mode: 'development',
   entry: {
    index: {
      import: './src/index.js',
      dependOn: 'shared',
    },
    main: {
      import: './src/main.js',
      dependOn: 'shared',
    },
    shared: 'lodash',
    // shared: ['lodash']
   },
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
   },
   optimization: {
    runtimeChunk: 'single',
   },
 };
```

构建结果如下：

```js
...
[webpack-cli] Compilation finished
asset shared.bundle.js 549 KiB [compared for emit] (name: shared)
asset runtime.bundle.js 7.79 KiB [compared for emit] (name: runtime)
asset index.bundle.js 1.77 KiB [compared for emit] (name: index)
asset main.bundle.js 1.65 KiB [compared for emit] (name: main)
Entrypoint index 1.77 KiB = index.bundle.js
Entrypoint main 1.65 KiB = main.bundle.js
Entrypoint shared 557 KiB = runtime.bundle.js 7.79 KiB shared.bundle.js 549 KiB
runtime modules 3.76 KiB 7 modules
cacheable modules 530 KiB
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
  ./src/main.js 84 bytes [built] [code generated]
  ./src/index.js 257 bytes [built] [code generated]
webpack 5.4.0 compiled successfully in 249 ms
```

这样我们就解决了第一个问题，但是 webpack 官网中并不推荐这种方法

尽管 webpack 允许每个页面使用多个入口起点，但在可能的情况下，应该避免使用多个入口起点，而使用具有多个导入的单个入口起点：entry: { page: ['./analytics', './app'] }。这样可以获得更好的优化效果，并在使用异步脚本标签时保证执行顺序一致。

### **SplitChunksPlugin**

SplitChunksPlugin 插件可以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk。让我们使用这个插件去除之前示例中重复的 lodash 模块：

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: {
    index: './src/index.js',
    main: './src/main.js',
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  optimization: {
    splitChunks: {
      // chunks 可以设置的值有三个
      // 1. async 对异步(async)引入的包进行分包操作 默认值
      // 2. inital 对同步引入的包进行分包操作
      // 3. all 对所有引入的包全部进行分包操作
      chunks: 'all',
      // 默认情况下，所有被抽取出去的包都会合并到[id].bundle.js中
    },
  },
};
```

使用 optimization.splitChunks 配置选项后构建，将会发现 index.bundle.js 和 main.bundle.js 已经移除了重复的依赖模块。从插件将 lodash 分离到单独的 chunk，并且将其从 main bundle 中移除，减轻了 bundle 大小。执行 npm run build 查看效果：

```js
...
[webpack-cli] Compilation finished
asset vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB [compared for emit] (id hint: vendors)
asset index.bundle.js 8.92 KiB [compared for emit] (name: index)
asset main.bundle.js 8.8 KiB [compared for emit] (name: main)
Entrypoint index 558 KiB = vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB index.bundle.js 8.92 KiB
Entrypoint main 558 KiB = vendors-node_modules_lodash_lodash_js.bundle.js 549 KiB another.bundle.js 8.8 KiB
runtime modules 7.64 KiB 14 modules
cacheable modules 530 KiB
  ./src/index.js 257 bytes [built] [code generated]
  ./src/main.js 84 bytes [built] [code generated]
  ./node_modules/lodash/lodash.js 530 KiB [built] [code generated]
webpack 5.4.0 compiled successfully in 241 ms
```

splitChunks 主要有两种类型的配置：

- minChunks/minSize/maxInitialRequest 等分包条件，满足这些条件的模块都会被执行分包；
- cacheGroup ：用于为特定资源声明特定分包条件，例如可以为 node_modules 包设定更宽松的分包条件。

#### **根据 Module 使用频率分包**

当然，我们也可以通过 splitChunks 按 Module 被 Chunk 引用的次数决定是否分包，借助这种能力我们可以轻易将那些被频繁使用的模块打包成独立文件，减少代码重复

```js
module.exports = {
  optimization: {
    splitChunks: {
      // 设定引用次数超过 2 的模块才进行分包
      minChunks: 2,
      
    },
  },
}
```

另外，splitChunks 还能自定义限制分包数量和限制分包体积大小

#### **限制分包数量**

为防止最终产物文件数量过多导致 HTTP 网络请求数剧增，导致降低应用性能。为了解决这个问题，Webpack 提供了两个配置项，用于限制分包数量：

- maxInitialRequest：用于设置 Initial Chunk 最大并行请求数；
- maxAsyncRequests：用于设置 Async Chunk 最大并行请求数。

这里所说的“请求数”，是指加载一个 Chunk 时所需要加载的所有分包数。

例如对于一个 Chunk A，如果根据分包规则(如模块引用次数、第三方包)分离出了若干子 Chunk A[i]，那么加载 A 时，浏览器需要同时加载所有的 A[i]，此时并行请求数等于 i 个分包加 A 主包，即 i+1。

```js
// common.js
export default "common chunk";

//=======================

// entry-a.js
import common from './common'

// entry-b.js
import common from './common'
```

若 minChunks = 2 ，则 common 模块命中 minChunks 规则被独立分包，浏览器请求 entry-a 时，则需要同时请求 common 包，并行请求数为 1 + 1=2。

```js
// common1.js
export default "common chunk1";

// common2.js
export default "common chunk2";

//=======================

// entry-a.js
import common1 from './common1'

// entry-b.js
import common1 from './common1'
import common2 from './common2'

// entry-c.js
import common2 from './common2'
```

这里我们看到 common1 和 common2 都被引用了 2 次，当 minChunks = 2 时，浏览器请求 entry-b 时需要同时请求 common-1 、common-2 两个分包，并行数为 **2 + 1 = 3**，此时若 **maxInitialRequest = 2**，则分包数超过阈值，SplitChunksPlugin 会 放弃 common-1、common-2 中**体积较小的分包**。**maxAsyncRequest 逻辑与此类似，不在赘述。**

------

**并行请求数关键逻辑总结如下：**

------

- Initial Chunk 本身算一个请求；
- Async Chunk 不算并行请求；
- 通过 runtimeChunk 拆分出的 runtime 不算并行请求；
- 如果同时有两个 Chunk 满足拆分规则，但是 maxInitialRequests(或 maxAsyncRequest) 的值只能允许再拆分一个模块，那么体积更大的模块会被优先拆解。

#### **限制分包体积**

Webpack 提供了一系列与 Chunk 大小有关的分包判定规则，借助这些规则我们可以实现当包体过小时直接取消分包 —— 防止产物过"碎"；当包体过大时尝试对 Chunk 再做拆解 —— 避免单个 Chunk 过大

|                      | 释义                                                      |
| :------------------- | :-------------------------------------------------------- |
| minSize              | 超过这个尺寸的 Chunk 才会正式被分包                       |
| maxSize              | 超过这个尺寸的 Chunk 会尝试进一步拆分出更小的 Chunk       |
| maxAsyncSize         | 与 maxSize 功能类似，但只对异步引入的模块生效             |
| maxInitialSize       | 与 maxSize 类似，但只对 entry 配置的入口模块生效          |
| enforceSizeThreshold | 超过这个尺寸的 Chunk 会被强制分包，忽略上述其它 Size 限制 |

- SplitChunksPlugin 尝试将命中 minChunks 规则的 Module 统一抽到一个额外的 Chunk 对象
- 判断该 Chunk 是否满足 maxInitialRequests 阈值，若满足则进行下一步
- 判断该 Chunk 资源的体积是否大于上述配置项 minSize 声明的下限阈值
- 如果体积小于 minSize 则取消这次分包，对应的 Module 依然会被合并入原来的 Chunk
- 如果 Chunk 体积大于 minSize 则判断是否超过 maxSize、maxAsyncSize、maxInitialSize 声明的上限阈值，如果超过则尝试将该 Chunk 继续分割成更小的部分

提示：虽然 maxSize 等阈值规则会产生更多的包体，但缓存粒度会更小，命中率相对也会更高，配合持久缓存与 HTTP2 的多路复用能力，网络性能反而会有正向收益。

#### **缓存组 cacheGroups**

```js
module.exports = {
  optimization: {
    splitChunks: {
      cacheGroups: {
        vendors: {
          test: /[/]node_modules[/]/,
          priority: -10,
          reuseExistingChunk: true,
          minChunks: 1,
          minSize: 0
        }
      },
    },
  },
};
```

示例通过 cacheGroups 属性设置 vendors 的缓存组，所有命中 defaultVendors.test 规则的模块都会被归类 vendors 分组，优先应用该组下的 minChunks、minSize 等分包配置

cacheGroups 支持上述 minSice/minChunks/maxInitialRequest 等条件配置，此外还支持一些与分组逻辑强相关的属性，包括：

- test：接受正则表达式、函数及字符串，所有符合 test 判断的 Module 或 Chunk 都会被分到该组
- type：接受正则表达式、函数及字符串，与 test 类似均用于筛选分组命中的模块，区别是它判断的依据是文件类型而不是文件名，例如 type = 'json' 会命中所有 JSON 文件
- idHint：字符串型，用于设置 Chunk ID，它还会被追加到最终产物文件名中，例如 idHint = 'vendors' 时，输出产物文件名形如 vendors-xxx-xxx.js
- priority：数字型，用于设置该分组的优先级，若模块命中多个缓存组，则优先被分到 priority 更大的组

缓存组的作用在于能为不同类型的资源设置更具适用性的分包规则，一个典型场景是将所有 node_modules 下的模块统一打包到 vendors 产物，从而实现第三方库与业务代码的分离

在 webpack 官网中写道，webpack会默认开启 cacheGroups 配置

```js
cacheGroups: {
  // 如果属于第三方库，就按照 cacheGroups 下的 defaultVendors 配置打包
  defaultVendors: {
    test: /[/]node_modules[/]/,
    priority: -10, // 优先级
    reuseExistingChunk: true, // 如果一个模块已经被打包过了，那么这个模块也不会被打包
  },
  default: {
    minChunks: 2,
    priority: -20,
    reuseExistingChunk: true,
  },
}
```

- 将所有 node_modules 中的资源单独打包到 vendors-xxx-xx.js 命名的产物
- 对引用次数大于等于 2 的模块 —— 也就是被多个 Chunk 引用的模块，单独打包

如果不需要这个默认配置，则可以将 default 设置为 false

```js
cacheGroups: {
  default: false
}
```

## **动态加载**

webpack 提供了两个类似的技术实现动态代码分离。第一种，也是推荐选择的方式，是使用符合 ECMAScript 提案的 import() 语法实现动态导入。第二种则是 webpack 的遗留功能，使用 webpack 特定的 require.ensure.

当 webpack 识别到这两个语法时，会自动将被导入的模块分割成一个单独的文件，然后在需要的时候再去加载这个文件。

这两种语法其实本质上是一样的，不过下面会以 import 为例进行讲解

动态导入通常是一定会打包成独立的文件的，默认情况下这些异步模块的命名规则和 output.filename 的命令规则是一致的，但是我们通常希望异步的模块取名叫做 xxx.chunk.js，此时，我们可以通过 output.chunkFilename 属性来命名

```js
module.exports = {
  output: {
    path: path.resolve(__dirname, '../dist'),
    // 如果是同步模块的时候，一般命名为bundle
    filename: '[name].bundle.js',
    // 如果是动态引入，一般命名为chunk
    chunkFilename: '[name].chunk.js'
  }
}
```

你会发现默认情况下我们获取到的 [name] 是和id的名称保持一致的，如果我们希望修改name的值，可以通过magic comments(魔法注释)的方式

```js
import(/* webpackChunkName: 'foo' */'./foo').then(res => console.log(res))
```

此时，打包出来的模块名就会是foo.chunk.js

```js
// index.js
function getComponent() {
  return import(/* webpackChunkName: "lodash" */ 'lodash').then(({ default: _ }) => {
    let element = document.createElement('div');
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
    return element;
  });
}

document.addEventListener('click', () => {
  getComponent().then(element => {
    document.body.appendChild(element);
  });
});
```

这些动态引入的代码，在首次加载时并不会被引入，当发生点击事件，就会通过 jsonp 获取 lodash.js 文件，由此可见，在一定程度上，动态引入的方式是优于同步引入的，这也就是为什么 webpack 默认对异步代码进行分离的原因。同时，使用异步代码，那么代码利用率也会得到提升。

异步加载的代码，会保存在一个全局的 webpackJsonp 中。

- webpackJsonp.push 的的值，两个参数分别为异步加载的文件中存放的需要安装的模块对应的 id 和异步加载的文件中存放的需要安装的模块列表。
- 在满足某种情况下，会执行具体模块中的代码

## **预获取/预加载模块**

- 预获取 prefetch：在浏览器加载完必要的资源后，空闲时就会去获取可能需要的资源。
- 预加载 preload：预先加载当前页面可能需要的资源，它与必要资源并行请求。

```js
import(/* webpackPrefetch: true */ './a')
```

a 文件会在所有必要资源加载完成后，网络空闲就预先获取了添加了魔法注释的异步模块。

