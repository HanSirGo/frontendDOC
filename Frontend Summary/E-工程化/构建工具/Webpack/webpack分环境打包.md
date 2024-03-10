# webpack分环境打包（生产/开发两套打包）

## 前言

在实际开发中，开发环境和生产环境的配置有很多是相同的，例如都会配置相同的entry。考虑到代码的复用性和可维护性，我们通常要把相同的配置提取出来，以供开发环境和生产环境来使用。

## 采用环境变量

可能用的比较多的（一些配置量较少的场景）会采用环境变量判断，定义不同环境变量下不同的loaders、plugins、mode、devtool等配置，来根据变量来打包，就像这样的代码：

```js
const path = require('path');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

const env = process.env.NODE_ENV;
const loaders = [
  {
    test: /\.js$/,
    exclude: /node_modules/,
    use: {
      loader: 'babel-loader',
      options: {
        presets: ['@babel/preset-env'],
        cacheDirectory: true,
      }
    }
  },
  {
    test: /\.jpeg$/,
    type: 'asset/resource',
  }
];
const plugins = [
  new HtmlWebpackPlugin({
    template: './index.html'
  }),
  new CleanWebpackPlugin(),
];
let devtool = "";
const mode = env;
let devServer = {};

if (env === "development") {
  loaders.push(
    {
      test: /\.css|scss|sass$/,
      use: ['style-loader', 'css-loader', 'sass-loader']
    }
  )
  plugins.push(
    new CleanWebpackPlugin()
  )
  devtool = "inline-source-map";
  devServer = {
    historyApiFallback: true,
    publicPath: '/',
    open: true,
    compress: true,
    hot: true,
    port: 8888
  }
} else {
  loaders.push(
    {
      test: /\.css|scss|sass$/,
      use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader']
    }
  )
  plugins.push(
    new MiniCssExtractPlugin({
      filename: 'index-[contenthash:8].css',
      chunkFilename: '[id].css'
    })
  )
  devtool = "cheap-module-source-map";
}


module.exports = {
  entry: './a.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    assetModuleFilename: 'static/[hash:6][ext][query]'
  },
  module: {
    rules: loaders
  },
  plugins,
  mode,
  devtool,
  devServer: {
    historyApiFallback: true,
    publicPath: '/',
    open: true,
    compress: true,
    hot: true,
    port: 8888
  },
}
```

可以看到，在开发环境下，我们使用css-loader解析完css文件后直接使用style-loader将其打包到bundle.js文件中；而生产环境则使用mini-css-extract-plugin插件将css文件单独抽离出来，并且分别执行了npm run buildD、npm run buildP命令打包后，产出了不同的dist文件夹。

```js
scripts: {
  "buildD": "cross-env NODE_ENV=development webpack --config webpack.config.js",
    "buildP": "cross-env NODE_ENV=production webpack --config webpack.config.js",
}
```

**开发环境的打包结果：**

![1710061915901](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710061915901.png)

**生产环境的打包结果：**

![1710061933865](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710061933865.png)

在这个例子中，我们把开发环境和生产环境的配置项写在了同一个文件里，在项目简单时，可以接受这样的写法，但是复杂起来就会变得难以维护，针对此问题，社区所提供的的webpack-merge工具支持webpack配置文件的合并，解决了这个难题。

## 采用webpack-merge

对于大型项目，具有大量的loader、plugin时，通过环境变量、分支语句在webpack.config.js中进行配置的代码是不健全的，因此推荐使用webpack社区的webpack-merge工具，非常适合webpack文件的合并。

```
npm i --save-D webpack-merge@5.7.3
```

现在将上面的例子进行改写，首先我们抽离出三份webpack配置文件：

- 公共文件（相同配置汇集） webpack.config.js
- 开发环境配置文件（开发单例）    webpack.development.js
- 生产环境配置文件（生产单例） webpack.production.js

webpack.config.js文件内容如下：

```js
const path = require('path');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './a.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js',
    assetModuleFilename: 'static/[hash:6][ext][query]'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            cacheDirectory: true,
          }
        }
      },
      {
        test: /\.jpeg$/,
        type: 'asset/resource',
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html'
    }),
    new CleanWebpackPlugin(),
  ],
}
```

webpack.development.js文件内容如下：

```js
const { merge } = require('webpack-merge');
const common = require('./webpack.config.js');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = merge(common, {
  module: {
    rules: [
      {
        test: /\.css|scss|sass$/,
        use: ['style-loader', 'css-loader', 'sass-loader']
      }
    ]
  },
  plugins: [
    new CleanWebpackPlugin(),
  ],
  devtool: "inline-source-map",
  mode: "development",
  devServer: {
    historyApiFallback: true,
    publicPath: '/',
    open: true,
    compress: true,
    hot: true,
    port: 8888
  },
})
```

webpack.production.js文件内容如下：

```js
const { merge } = require('webpack-merge');
const common = require('./webpack.config.js');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = merge(common, {
  module: {
    rules: [
      {
        test: /\.css|scss|sass$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader']
      }
    ]
  },
  plugins: [
    new MiniCssExtractPlugin({
      filename: 'index-[contenthash:8].css',
      chunkFilename: '[id].css'
    })
  ],
  devtool: 'cheap-module-source-map',
  mode: 'production'
})
```

最后我们在package.json中加入两种打包模式，根据目前的任务需求（开发/项目上线）来进行不同的选择打包。

```js
scripts: {
  "buildD": "cross-env NODE_ENV=development webpack --config webpack.development.js",
    "buildP": "cross-env NODE_ENV=production webpack --config webpack.production.js",
}
```

## 总结

webpack-merge工具给我们的配置文件增加了灵活性和可维护性，在之前的版本还支持merge.smart方法进行智能合并，但由于该方法要考虑的边界条件过多，在2020年开始该工具已不再支持merge.smart方法了。在我们平时的前端开发工作中，只需要最基础的merge方法，就可以很好的完成配置文件的编写工作。