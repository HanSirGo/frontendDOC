# Webpack之Tree Shaking



# **一、是什么**

Tree shaking 是一个用于优化 JavaScript 应用程序的术语，特别是在构建过程中减少生成的代码大小。它的主要目标是消除未使用的代码，即那些在应用程序中没有被引用或调用的部分，从而减小最终生成的 JavaScript 文件的大小。

在 JavaScript 中，项目通常会包含许多模块、库和依赖项。在构建应用程序时，为了确保最终生成的代码尽可能小，开发者可以使用 tree shaking 技术来去除未被使用的部分。这对于前端开发领域尤其重要，因为用户需要下载和执行的 JavaScript 代码越小，页面加载速度就越快。

## **Tree Shaking 具体做了什么**

我们通过例子来详细了解一下 Webpack 中 Tree Shaking 到底做了什么

- 未使用的函数消除

```js
// utils.js
export function sum(x, y) {
  return x + y;
}

export function sub(x, y) {
  return x - y;
}
```

```js
// index.js
import { sum } from "./utils";
// import * as math from "./utils";

console.log(sum(1, 2));
```

我们在 utils 中定义了 sum 与 sub 两个方法， 仅使用了 sum 方法，而 sub 方法并没有被使用。我们一起看一下打包后的结果

```js
(()=>{"use strict";console.log(3)})();
```

- 未使用的 JSON 数据消除

```js
// main.json
{
  "a": "a",
  "b": "b"
}
```

```js
// index.js
import main from "./main.json";

console.log(main.a);
```

可以看到仅使用了 JSON 中的 a 。我们一起看一下打包后的结果

```js
(()=>{"use strict";console.log("a")})();
```

# **二、怎么用**

## **在 Vite 中**

在 Vite 中，Tree shaking 是默认启用的，因为 Vite 使用 ES6 模块系统并支持原生的 ESM（ECMAScript Modules）。Vite 利用这种模块系统的特性，可以在构建过程中识别和剔除未使用的代码，实现 Tree shaking。

不过，有一些注意事项和配置项可以帮助你更好地理解和控制 Tree shaking 在 Vite 中的行为：

1. **ES6 模块系统：** 确保你的代码采用 ES6 模块的形式，以便 Vite 能够充分利用这种模块系统的静态分析特性。使用 `import` 和 `export` 语法来定义模块。
2. **Package.json 中的 "sideEffects" 字段：** 如果你的项目中有一些模块具有副作用（例如修改全局状态或引入样式），你可能需要在 `package.json` 文件中的 `"sideEffects"` 字段中进行配置。如果你确定某个模块没有副作用，可以将其添加到 `"sideEffects"` 字段中，帮助 Vite 更好地进行 Tree shaking。

```js
"sideEffects": ["src/your-module-without-side-effects.js"]
```

3. **Vite 配置中的 build.rollupOptions：** 如果你需要更进一步的配置，你可以使用 Vite 的 `build.rollupOptions` 选项，这是一个传递给 Rollup 的配置对象。你可以在这里配置一些与 Tree shaking 相关的选项，例如 `treeshake`。

```js
// vite.config.js
export default {
 build: {
   rollupOptions: {
     treeshake: true // 默认就是 true，可以省略这个配置
   }
 }
}
```

## **在Webpack中**

1. **使用UglifyJsPlugin或TerserPlugin：** Tree Shaking通常需要与代码压缩工具结合使用，以确保未使用的代码被正确地剔除。在Webpack中，可以使用`UglifyJsPlugin`（对于Webpack 4及以下版本）或`TerserPlugin`（对于Webpack 5及以上版本）。

如何配置Tree Shaking：

```js
const path = require('path');
const TerserPlugin = require('terser-webpack-plugin');

module.exports = {
  mode: 'production',
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
  },
  optimization: {
    minimize: true,
    minimizer: [new TerserPlugin()],
  },
};
```

解释：

- `mode: 'production'` 设置了Webpack的生产模式，启用了一些优化功能，包括Tree Shaking。
- `optimization` 部分配置了压缩选项，使用了`TerserPlugin`插件。

确保你的`package.json`文件中的`sideEffects`字段正确配置。`sideEffects`字段允许你指定哪些文件是没有副作用的，这对于Tree Shaking来说很重要。如果你的代码库中的所有文件都是"pure"（没有副作用），可以将`sideEffects`设置为`false`，以进一步帮助Webpack进行优化。

```js
"sideEffects": false
```

# **三、作用**

Tree shaking的主要作用是优化前端应用程序的性能，特别是在减小生成的 JavaScript 文件大小方面发挥关键作用。

列举回答：

1. **减小文件大小：** Tree shaking通过静态分析代码，识别和移除未使用的代码块。这样可以消除应用程序中未被引用或调用的部分，从而减小最终生成的 JavaScript 文件的大小。
2. **提高加载速度：** 通过减小文件大小，Tree shaking有助于提高应用程序的加载速度。用户在访问网站时需要下载的JavaScript代码更少，因此页面加载时间更短，用户体验更好。
3. **网络传输优化：** 较小的文件大小意味着在网络上传输数据的成本更低。这对于用户在慢速或不稳定的网络条件下访问网站时尤为重要，可以减少加载时间和提高可访问性。
4. **资源利用率：** 通过消除未使用的代码，Tree shaking可以提高资源的利用率。只有实际需要的代码被包含在最终的构建中，因此减小了浏览器需要处理的代码量。
5. **版本控制和部署：** Tree shaking在版本控制和部署过程中也有帮助。较小的文件大小意味着在版本控制系统中占用的空间更小，并且在部署到生产环境时传输的数据更少，减少了部署时间和成本。
6. **优化用户体验：** 更快的加载速度和更小的文件大小可以显著改善用户体验。用户更倾向于与快速加载的应用程序进行交互，因此通过Tree shaking优化可以提高应用的用户满意度。

# **四、原理**

## **4.1 三个步骤**

Webpack 中，Tree-shaking 的实现一是先**标记**出模块导出值中哪些没有被用过，二是使用 Terser 删掉这些没被用到的导出语句。标记过程大致可划分为三个步骤：

- Make 阶段，收集模块导出变量并记录到模块依赖关系图 ModuleGraph 变量中
- Seal 阶段，遍历 ModuleGraph 标记模块导出变量有没有被使用
- 生成产物时，若变量没有被其它模块使用则删除对应的导出语句

> 标记功能需要配置 `optimization.usedExports = true` 开启

也就是说，标记的效果就是删除没有被其它模块使用的导出语句，比如：

![1713681215246](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713681215246.png)

示例中，`bar.js` 模块(左二)导出了两个变量：`bar` 与 `foo`，其中 `foo` 没有被其它模块用到，所以经过标记后，构建产物(右一)中 `foo` 变量对应的导出语句就被删除了。作为对比，如果没有启动标记功能(`optimization.usedExports = false` 时)，则变量无论有没有被用到都会保留导出语句，如上图右二的产物代码所示。

注意，这个时候 `foo` 变量对应的代码 `const foo='foo'` 都还保留完整，这是因为标记功能只会影响到模块的导出语句，真正执行“**Shaking**”操作的是 Terser 插件。例如在上例中 `foo` 变量经过标记后，已经变成一段 Dead Code —— 不可能被执行到的代码，这个时候只需要用 Terser 提供的 DCE 功能就可以删除这一段定义语句，以此实现完整的 Tree Shaking 效果。。

## **4.2 收集模块导出**

首先，Webpack 需要弄清楚每个模块分别有什么导出值，这一过程发生在 make 阶段。

1. 将模块的所有 ESM 导出语句转换为 Dependency 对象，并记录到 `module` 对象的 `dependencies` 集合，转换规则：

- 具名导出转换为 `HarmonyExportSpecifierDependency` 对象
- `default` 导出转换为 `HarmonyExportExpressionDependency` 对象

例如对于下面的模块：

```js
export const bar = 'bar';
export const foo = 'foo';

export default 'foo-bar'
```

对应的`dependencies` 值为：

![1713681237437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713681237437.png)

2. 所有模块都编译完毕后，触发 `compilation.hooks.finishModules` 钩子，开始执行 `FlagDependencyExportsPlugin` 插件回调

3. `FlagDependencyExportsPlugin` 插件从 entry 开始读取 ModuleGraph 中存储的模块信息，遍历所有 `module` 对象

4. 遍历 `module` 对象的 `dependencies` 数组，找到所有 `HarmonyExportXXXDependency` 类型的依赖对象，将其转换为 `ExportInfo` 对象并记录到 ModuleGraph 体系中

经过 `FlagDependencyExportsPlugin` 插件处理后，所有 ESM 风格的 export 语句都会记录在 ModuleGraph 体系内，后续操作就可以从 ModuleGraph 中直接读取出模块的导出值。

## **4.3 标记模块导出**

模块导出信息收集完毕后，Webpack 需要标记出各个模块的导出列表中，哪些导出值有被其它模块用到，哪些没有，这一过程发生在 Seal 阶段，主流程：

1. 触发 `compilation.hooks.optimizeDependencies` 钩子，开始执行 `FlagDependencyUsagePlugin` 插件逻辑
2. 在 `FlagDependencyUsagePlugin` 插件中，从 entry 开始逐步遍历 ModuleGraph 存储的所有 `module` 对象
3. 遍历 `module` 对象对应的 `exportInfo` 数组
4. 为每一个 `exportInfo` 对象执行 `compilation.getDependencyReferencedExports` 方法，确定其对应的 `dependency` 对象有否被其它模块使用
5. 被任意模块使用到的导出值，调用 `exportInfo.setUsedConditionally` 方法将其标记为已被使用。
6. `exportInfo.setUsedConditionally` 内部修改 `exportInfo._usedInRuntime` 属性，记录该导出被如何使用
7. 结束

上面是极度简化过的版本，中间还存在非常多的分支逻辑与复杂的集合操作，我们抓住重点：标记模块导出这一操作集中在 `FlagDependencyUsagePlugin` 插件中，执行结果最终会记录在模块导出语句对应的 `exportInfo._usedInRuntime` 字典中。

## **4.4 生成代码**

经过前面的收集与标记步骤后，Webpack 已经在 ModuleGraph 体系中清楚地记录了每个模块都导出了哪些值，每个导出值又没那块模块所使用。接下来，Webpack 会根据导出值的使用情况生成不同的代码，例如：

![1713681275593](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713681275593.png)

重点关注 `bar.js` 文件，同样是导出值，`bar` 被 `index.js` 模块使用因此对应生成了 `__webpack_require__.d` 调用 `"bar": ()=>(/* binding */ bar)`，作为对比 `foo` 则仅仅保留了定义语句，没有在 chunk 中生成对应的 export。

这一段生成逻辑均由导出语句对应的 `HarmonyExportXXXDependency` 类实现，大体的流程：

1. 打包阶段，调用 `HarmonyExportXXXDependency.Template.apply` 方法生成代码
2. 在 `apply` 方法内，读取 ModuleGraph 中存储的 `exportsInfo` 信息，判断哪些导出值被使用，哪些未被使用
3. 对已经被使用及未被使用的导出值，分别创建对应的 `HarmonyExportInitFragment` 对象，保存到 `initFragments` 数组
4. 遍历 `initFragments` 数组，生成最终结果

基本上，这一步的逻辑就是用前面收集好的 `exportsInfo` 对象未模块的导出值分别生成导出语句。

## **4.5 删除 Dead Code**

经过前面几步操作之后，模块导出列表中未被使用的值都不会定义在 `__webpack_exports__` 对象中，形成一段不可能被执行的 Dead Code 效果，如上例中的 `foo` 变量：

![1713681288985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713681288985.png)

在此之后，将由 Terser、UglifyJS 等 DCE 工具“摇”掉这部分无效代码，构成完整的 Tree Shaking 操作。

## **4.6 总结**

综上所述，Webpack 中 Tree Shaking 的实现分为如下步骤：

- 在 `FlagDependencyExportsPlugin` 插件中根据模块的 `dependencies` 列表收集模块导出值，并记录到 ModuleGraph 体系的 `exportsInfo` 中
- 在 `FlagDependencyUsagePlugin` 插件中收集模块的导出值的使用情况，并记录到 `exportInfo._usedInRuntime` 集合中
- 在 `HarmonyExportXXXDependency.Template.apply` 方法中根据导出值的使用情况生成不同的导出语句
- 使用 DCE 工具删除 Dead Code，实现完整的树摇效果