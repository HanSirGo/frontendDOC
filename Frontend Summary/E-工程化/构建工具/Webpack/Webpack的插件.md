####  webpack-bundle-analyzer
##### 打包体积分析工具
webpack-bundle-analyzer它是一个webpack优化分析工具，他通过可缩放图像的形式，帮我们分析打包后的资源体积大小，并可以分析该资源由那些模块组成

```bash
npm install -D webpack-bundle-analyzer
```
```bash
// webpack.config.js
var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  ...
  plugins:[
    new BundleAnalyzerPlugin(),
  ]
};
```
当我们项目使用了BundleAnalyzerPlugin性能分析插件后，执行npx webpack打包，可以看到浏览器会自动打开一个页面
#### speed-measure-webpack-plugin
##### 打包速度分析工具
此工具可以帮我们分析webpack 在打包过程中预处理器和插件等花费的时间
在前端项目安装npm包以及在配置文件中调用其wrap方法即可

在使用speed-measure-webpack-plugin工具时，只需要使用它提供的SpeedMeasurePlugin类来生成一个实例，然后调用其实例的wrap方法来包裹这个对象，最后对外输出export即可
```bash
const path = require('path');
const SpeedMeasurePlugin = require("speed-measure-webpack-plugin");
const smp = new SpeedMeasurePlugin();
let config = {
  entry: './a.js',
  output: {
    path: path.resolve(__dirname, ''),
    filename: 'bundle.js'
  },
  mode: 'none'
}

module.exports = smp.wrap(config);
```
在配置完成SpeedMeasurePlugin 插件后，我们执行npx webpack就会在控制台中看到，在实际开发的时候预处理器和插件旺旺占据时间花费的主要部分，
#### simple-progress-webpack-plugin 
##### 可以显示打包百分比
```bash
npm i simple-progress-webpack-plugin -d

```
```bash
const simpleprogresswebpackplugin = require( 'simple-progress-webpack-plugin' )

...

 plugins: [
  new simpleprogresswebpackplugin()
 ]

```
#### 压缩JS文件
在webpack4之前，我们会使用webpack-optimize。UglifyJsPlugin或webpack-parallel-uglify-plugin这一类插件对JS文件压缩

现在我们使用terser-webpack-plugin差劲进行对JS文件进行压缩，terser-webpack-plugin是webpack5自带的插件，无需安装

在使用terser-webpack-plugin插件进行js文件压缩时，有两种方案可以选择，一种是在webpack配置项plugins里使用该插件进行压缩，另一种是通过optimization配置项来配置该插件作为压缩器进行压缩，先看第一种

##### 1.在plugins配置项里配置terser-webpack-plugin插件
它配置与普通插件的使用是一样的
```bash
// webpack.config.js
var TerserPlugin = require("terser-webpack-plugin");

module.exports = {
  plugins: [
   new TerserPlugin(),
  ]
};
```
我们观察到打包生成的bundle.js文件里的代码被压缩成一行了

##### 2. 在optimization配置terser-webpack-plugin拆件

在optimization配置项里配置terser-webpack-plugin和普通的插件使用方法不太一样，首先要开启optimization.minimize，完整配置如下
```bash
var path = require('path');
var TerserPlugin = require("terser-webpack-plugin");

module.exports = {
  optimization: {
    minimize: true,
    minimizer: [new TerserPlugin()],
  }
};
```
optimization.minimize是一个布尔值，optimization.minimizer是一个数组，用于存放压缩器

当将 optimizationminimize 的值设为 true 时，Webpack 会使用optimization.minimize里配置的压缩器进行压缩。在这个例子里，我们配置的压缩器是new TerserPlugin0,在执行 npx webpack命令进行打包后,生成的 bundlejs 文件里的代码会与前面的例子里一样被压缩成一行。

当将 optimization.minimize 的值设为 false 时，不会使用 optimization.minimize里配置的压缩器进行压缩。optimization.minimize 参数就像一个开关，控制着压缩器是否工作。optimization,minimizer 里除了可以配置压缩JS 文件的压缩器，还可以配置压缩CSS文件的压缩器
#### 压缩 CSS 文件
在 Webpack4时期，用来压缩CSS 文件的插件通常是 optimize-css-assets-webpack.plugin 插件

在 Webpack5支持，建议使用 css-minimizer-webpack-plugin 插件对CSS文件进行压缩
```bash
var path = require('path');
var MiniCssExtractPlugin = require('mini-css-extract-plugin'); // 提取css代码为一个文件
var CssMinimizerPlugin = require('css-minimizer-webpack-plugin') // 压缩css代码为一行

module.exports = {
  module: {
    rules: [{
      test: /\.css$/,
      use: [
        MiniCssExtractPlugin.loader,
        'css-loader'
      ],
    }],
  },
  optimization: {
    minimize: true,
    minimizer: [new CssMinimizerPlugin()],
  },
  plugins:[
    new MiniCssExtractPlugin({
      filename: '[name]-[contenthash:8].css',
      chunkFilename: '[id].css',
    }),
  ]
};
```
其中terser-webpack-plugin 与 css-minimizer-webpack-plugin 插件还支持非常多的个性化参数，例如配置使用CPU的线程数及过滤需要压缩的文件