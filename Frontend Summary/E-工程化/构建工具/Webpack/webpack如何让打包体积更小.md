# webpack如何让打包体积更小

## 配置与方法

要使Webpack打包的bundle更小，你可以尝试以下一些策略：

### 1. **「代码分割」**

- 使用Webpack的动态`import()`语法进行代码分割，按需加载代码。

```js

button.addEventListener('click', event => {  
  import('./dynamic-module.js')  
    .then(module => {  
      // 使用module  
    })  
    .catch(err => {  
      // 加载失败处理  
    });  
});
```

- 使用`SplitChunksPlugin`插件来分割公共库和代码块。

```js

module.exports = {  
  // ... 其他配置  
  optimization: {  
    splitChunks: {  
      chunks: 'all', // 表示作用于异步和同步代码块  
      minSize: 30000, // 最小尺寸，只有超过这个大小的模块才会被分割  
      maxSize: 0, // 最大值，超过这个值会分割成新的代码块  
      minChunks: 1, // 最小被引用次数，默认是1  
      maxAsyncRequests: 30, // 按需加载时最大的并行请求数  
      maxInitialRequests: 30, // 一个入口点初始并行请求的最大数量  
      automaticNameDelimiter: '~', // 自动生成的块名的连接符  
      name: true, // 使用缓存组中的name属性作为输出块的名字  
      cacheGroups: {  
        vendors: {  
          test: /[\\/]node_modules[\\/]/,  
          priority: -10,  
          name: 'vendors',  
        },  
        default: {  
          minChunks: 2,  
          priority: -20,  
          reuseExistingChunk: true,  
        },  
      },  
    },  
  },  
};
```

### 2. **「Tree Shaking」**

- 确保在Webpack配置中设置`optimization.usedExports`为`true`（在Webpack 4+中默认启用）。

### 3. **「压缩和优化」**

- 使用`TerserPlugin`来压缩JavaScript。
- 使用`MiniCssExtractPlugin`和`cssnano`来提取和压缩CSS。
- 使用`OptimizeCSSAssetsPlugin`来进一步压缩CSS。

### 4. **「Scope Hoisting」**

- 在配置中使用`optimization.moduleIds: 'deterministic'`（或`'named'`）来启用Scope Hoisting，以减少函数声明和减少文件大小。

### 5. **「移除未使用的代码」**

- 使用`PurgeCSS`或`PurgeIcons`等工具来移除未使用的CSS和图标。
- 对于JavaScript，确保Tree Shaking能够正确工作，并移除所有未使用的依赖项和代码。

### 6. **「优化图片和字体」**

- 使用`url-loader`或`file-loader`来内联小图片到base64字符串中，避免额外的HTTP请求。
- 使用`image-webpack-loader`或`imagemin-webpack-plugin`来压缩图片。
- 使用`optimize-css-assets-webpack-plugin`来压缩CSS中的字体文件。

```js

module.exports = {  
  // ... 其他配置  
  module: {  
    rules: [  
      {  
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,  
        loader: 'image-webpack-loader',  
        options: {  
          mozjpeg: {  
            progressive: true,  
            quality: 65,  
          },  
          // 其他选项...  
        },  
      },  
    ],  
  },  
};
```

### 7. **「压缩HTML」**

- 使用`HtmlWebpackPlugin`的`minify`选项来压缩HTML输出。

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');  
  
module.exports = {  
  // ... 其他配置  
  plugins: [  
    new HtmlWebpackPlugin({  
      // ... 其他HtmlWebpackPlugin配置  
      minify: {  
        removeComments: true,  
        collapseWhitespace: true,  
        removeRedundantAttributes: true,  
        useShortDoctype: true,  
        removeEmptyAttributes: true,  
        removeScriptTypeAttributes: true,  
        removeStyleLinkTypeAttributes: true,  
        minifyJS: true,  
        minifyCSS: true,  
        minifyURLs: true,  
      },  
    }),  
  ],  
};
```

### 8. **「使用CDN」**

- 将大型库（如React、Vue、Angular等）和静态资源（如图片、字体等）托管在CDN上，减少主bundle的大小。

### 9. **「避免生产模式下的source map」**

- 在生产模式下，使用`source-map`会显著增加bundle的大小。考虑使用`hidden-source-map`、`nosources-source-map`或完全禁用source map。

### 10. **「优化Webpack配置」**

- 移除不必要的loader和plugin。
- 优化`resolve`配置，减少Webpack搜索模块的路径。
- 使用`externals`选项来排除大型库，并在HTML中直接引入。

### 11. **「使用Webpack Bundle Analyzer」**

- 使用`webpack-bundle-analyzer`插件来分析打包后的bundle，找出可以优化的地方。

安装并配置：

```
npm install --save-dev webpack-bundle-analyzer
```

```js

const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;  
  
module.exports = {  
  // ... 其他配置  
  plugins: [  
    // ... 其他插件  
    new BundleAnalyzerPlugin(),  
  ],  
};
```

### 12. **「持续监控和更新」**

- 监控Webpack和相关插件、loader的更新，以获取最新的优化和性能改进。确保Loader配置是高效的，并避免不必要的文件处理。例如，你可以使用include和exclude选项来指定Loader应该应用于哪些文件。

```js
module.exports = {  
  module: {  
    rules: [  
      {  
        test: /\.js$/,  
        exclude: /node_modules/, // 排除node_modules目录中的文件  
        use: 'babel-loader',  
      },  
      // ... 其他Loader配置  
    ],  
  },  
  // ... 其他配置  
};
```

#### 总结

Webpack打包优化涉及多个方面，以下是一些主要的优化策略：

1. **「设置生产模式」**：在Webpack配置中设置mode为production，这会启用Webpack的默认优化配置，包括代码压缩、作用域提升、去除无用代码等。这可以自动对输出文件进行优化，使其体积更小，加载速度更快。
2. **「代码分割」**：通过将代码分割成不同的块，按需加载，可以减小初始加载的文件体积，提高页面加载速度。Webpack提供了动态import()语法或SplitChunksPlugin插件来实现代码分割。
3. **「Tree Shaking」**：这是一种代码优化技术，可以帮助在打包过程中剔除掉未使用的模块和代码。在Webpack配置中启用optimization.uglifyOptions.treeshake选项可以开启Tree Shaking功能，只打包项目中实际使用的代码。
4. **「优化加载速度」**：使用Webpack的插件如MiniCssExtractPlugin来提取CSS代码，或使用babel-loader的缓存机制等，以减少构建时间和加载时间。
5. **「并行构建」**：使用Webpack的thread-loader或happypack插件将任务分发给多个子进程并行处理，提高构建速度。
6. **「优化文件体积」**：使用Webpack的压缩插件如terser-webpack-plugin来压缩JavaScript代码，使用cssnano等工具压缩CSS代码，减小文件体积。
7. **「按需加载」**：对于路由组件、第三方组件和插件等，可以按需加载，避免在初始加载时加载过多的内容。
8. **「优化正则匹配」**：在配置loader时，通过include、exclude等选项来减少被处理的文件，优化正则匹配，避免不必要的处理。
9. **「优化构建目标」**：缩小构建目标，只构建实际需要的部分，避免构建不必要的文件。
10. **「使用CDN」**：将静态资源如图片、CSS、JavaScript等部署到CDN上，可以加快资源加载速度。