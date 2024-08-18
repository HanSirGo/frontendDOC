# 一文彻底解决新手对 webpack 的恐惧！

# 前言

首先抛出结论，webpack是一个非常简单的工具，毫无难度可言。webpack阻碍很多人的根本原因是大家对一些概念的不熟悉、对webpack丰富的loader和plugin望而怯步。当你把概念弄清楚后，你自然而然的就知道自己需要什么loader和plugin了。

看完后你一定会说：“就这？webpack就这？”。

**个人建议：例如create-react-app就是一个很好的脚手架，你可以写不出这样的脚手架，但你一定要能看懂，在你有需要的时候有能力去修改源码。webpack掌握到这个程度很多时候就完全够用了。在最后我还会补充如何自己写一个loader和plugin（不过我相信不用到最后，认真阅读的你就已经知道要如何编写了）**

# 准备环境、版本

node: ^12.16.1

webpack: ^4.44.2

webpack-cli: ^3.3.12

**webpack5升级了一些新特性，所以我们这篇文章以目前常用的webpack4来讲解。不过完全不用担心都出5了怎么还学4的问题，道理都是相通的，4都会了，5它不就是多点新特性了吗。**

# 你必须知道的知识

## webpack概念

webpack是一个现代JavaScript应用程序的**静态模块打包器**。如果你接触js够早的话，你一定知道最初的工程可没有这样的打包工具，一个文件就相当于一个模块，最终这些文件需要按照一定的顺序使用script标签引入html（因为如果顺序不对就会导致依赖变量丢失等错误问题）。

但是这个写项目不仅麻烦而且不优雅，随着node.js的出现和发展，才出现了这类基于node.js运行的打包工具（gulp、grunt，以及现在最流行的webpack），因为node.js拥有可对文件操作的能力。所以webpack本质就是为我们打包js的引用，而我们常听到各种loader、各种plugin、热更新、热模块替换等等都是webpack的一个升华，使得webpack能为我们提供更多的帮助。

- loader：webpack本身只能打包js和json格式的文件，但实际项目中我们还有会css、scss、png、ts等其他文件，这时我们就需要使用loader来让它正确打包。

- - **总结：loader是处理编译js和json以外的文件时用的**
  - 常见的loader：

- `style-loader  css-loader  sass-loader  ts-loader  file-loader  babel-loader  postcss-loader  ...    `

- plugin：plugin可以在webpack运行到某个阶段时候，帮你做一些事情，类似react/vue中的生命周期。具体的某个插件（plugin）就是在webpack构建过程中的特定时机注入扩展逻辑来改变构建结果，作用于整个构建过程。

## 弄明白这些东西

- **postcss**：postcss是一个用 JavaScript 工具和插件转换 CSS 代码的工具。你可以把他理解成babel，他本身作用不大，我们很多具体需求的实现都需要他的插件来完成，他本身更像一个平台。

  例如，项目中我们需要webpack自动的帮我们为css样式加上兼容性前缀，实际帮我们加上前缀的插件是`autoprefixer`[1]，但他能为我们加前缀的前提又是我们有postcss。**千万不要把postcss误解postcss成scss、less替代品**

- - 对于“ PostCSS”一词，我们可以替代地指两件事：

- 1. 该工具本身就是**PostCSS**[2]（运行时会得到什么）`npm install postcss`，以及
  2. 该工具支持**的PostCSS插件生态系统**[3]

- **babel**：babel 是一个 JavaScript 编译器，他可以让我们不再考虑兼容性，尽情的使用下一代的 JavaScript 语法编程。但是要实现具体的语法转换，我们还是要使用他的**插件**[4]才能实现

- - 在babel7后为我们提供了**预设**[5]，可以让我们不再自己麻烦的对插件进行组合，想在什么环境运行就写什么预设即可（相当于每个预设选项中都帮你组合好了这个环境中需要用到的插件）

- **es6+语法**：babel默认会转换语法，例如：let、const、() => {}、class

- **es6+特性**：babel不会转换特性，特性需要js代码垫片来兼容低版本的浏览器。例如：Iterator、Generator、Set、Map、Proxy、Reflect、Symbol、Promise

- **@babel/core**：@babel/core是babel的核心库，所有的核心api都在这个库里，这些api可供babel-loader调用

- **@babel/preset-env**：这是一个预设的插件集合，包含了一组相关的插件，Bable中是通过各种插件来指导如何进行代码转换。该插件包含所有es6转化为es5的翻译规则

- **@babel/polyfill**：@babel/preset-env只是提供了语法转换的规则，但是它并不能弥补浏览器缺失的一些新的功能，如一些内置的方法和对象，如Promise，Array.from等，此时就需要polyfill来做js的垫片，弥补低版本浏览器缺失的这些新功能。**注意：Babel 7.4.0该包将被废弃**

- **core-js**：它是JavaScript标准库的polyfill，而且它可以实现按需加载。使用@babel/preset-env的时候可以配置core-js的版本和core-js的引入方式。**注意：@babel/polyfill依赖core-js**

- **regenerator-runtime**：提供generator函数的转码

### 补充知识点（重要）

- **browserslist**：browserslist实际上就是声明了一段浏览器的合集，我们的工具可以根据这个合集描述，针对性的输出兼容性代码，browserslist应用于babel、postcss等工具当中。

- - browserslist可以在package.json文件配置，也可以单出写一个.browserslistrc文件进行配置

  - 工具会自动查找.browserslistrc中的配置，如果没有发现.browserslistrc文件，则会去package.json中查找

  - 例子：

    ```
    // 在.browserslistrc中的写法  
    > 1%  
    last 2 versions  
    
    // 还可以配置不同环境下的规则（在.browserslistrc中）  
    [production]  
    > 1%  
    ie 10  
    
    [development]  
    last 1 chrome version  
    last 1 firefox version  
    
    // 在package.json中的写法  
    {  
      "browserslist": ["> 1%", "last 2 versions"]  
    }  
    
    // 还可以配置不同环境下的规则（在package.json中）  
    // production和development取决你webpack中mode字段的配置  
    {  
      "browserslist": {  
      "production": [  
      ">0.2%",  
      "not dead",  
      "not op_mini all"  
     ],  
       "development": [  
       "last 1 chrome version",  
       "last 1 firefox version",  
       "last 1 safari version"  
      ]  
     }  
     }
    ```

 “> 1%”表示兼容市面上使用量大于百分之一的浏览，“last 1 chrome version”表示兼容到谷歌的上一个版本，具体的可以使用命令`npx browserslist "> 1%"`的方式查看都包含了哪些浏览器，**browserslist的github地址**[6]

- **chunk**：它不是库也不是插件，它就是一个名词，顾名思义就是代码块。为什么要单独把他拎出来说呢，因为你明白它以后你自然就能理解webpack中配置一些参数的意思了

- **chunks**：一个chunks至少包含一个chunk，chunks是多个chunk的合集

  ```
  // index.js  
  import { a } from 'a.js'  
  console.log('我是index文件')  
  
  //a.js  
  const a = '我是a文件'  
  export { a }  
  ```

  上面代码来说，a.js就是chunk，而index.js就是chunks

  在webpack构建中入口是chunks，出口是chunk（知道这个概念就行）

# 搭建webpack项目

## 初始化项目

- 新建一个文件夹

  ```
  mkdir learnWebpack    
  ```

- 进入到文件夹中

  ```
  cd learnWebpack/    
  ```

- 初始化package.json文件

  ```
  # 可以输入配置  
  npm init  
  
  # 默认配置创建  
  npm init -y  
  
  # 是用npm init -y后得到的内容  
  {  
    "name": "learnwebpack",  
    "version": "1.0.0",  
    "description": "",  
    "main": "index.js",  
    "scripts": {  
      "test": "echo \"Error: no test specified\" && exit 1"  
    },  
    "author": "",  
    "license": "ISC"  
  }  
  ```

## 安装

- 安装webpack、webpack-cli依赖

  ```
  # 我更喜欢yarn，当然你也可以使用
  npm  yarn add webpack@4.44.2 webpack-cli@3.3.12 -D 
  ```

## 创建目录

- 根目录下创建src、src下创建index.js

- 在index.js中添加一段代码:

  ```
  console.log('hello webpack')    
  ```

- 在package.json文件中的scripts字段中添加`"start": "webpack"`

  ```
  "scripts": {  
    "test": "echo \"Error: no test specified\" && exit 1",  
    "start": "webpack"  
  }    
  ```

- 在命令行中执行`yarn start`，我们会发现编译成功，但有个警告！在webpack自动在根目录下创建了一个dist文件夹和dist文件夹中的main.js文件

- - 关于警告：ARNING in configuration The 'mode' option has not been set, webpack will fallback to 'production' for this value. Set 'mode' option to 'development' or 'production' to enable defaults for each environment. You can also set it to 'none' to disable any default behavior. Learn more: **webpack.js.org/configurati…**[7]; 警告中已经说的很清楚了，我们没有设置mode字段。我们只需在webpack的配置中配置mode字段即可消除该警告

  - 关于dist文件夹和main.js文件 ：这是webpack4当初宣传的零配置使用。很显然我们这里什么都没配置，就帮我们成功打包了一个src下的代码。该功能实际上是wabpack默认帮我们配置了一套简单的打包配置，让我们看看webpack默认为我们配置了什么：

    ```
    const path = require('path')  
    module.exports = {  
      extry: './src/index.js',  
      output: {  
        filename: 'main.js',  
        path: path.resolve(__dirname, './dist')   
      }  
    }  
    ```

  - 注意：这份默认配置是不是太过简单了。看了这个配置后，我想你也知道为什么当我们创建的不是src或者src下不是index.js时候无法使用零配置功能的原因了

## webpack的零配置

上面我们看到了，零配置很弱，真正的项目中他完全不可能满足我们的需求

我们想要自定义配置webpack的话，需要在根目录上创建一个`webpack.config.js`的文件，这个文件的内容可以覆盖webpack的零配置

- 使用默认的配置文件：webpack.config.js

  ```
  # 使用webpack.config.js配置文件时，输入该命令即可启动webpack打包  
  webpack    
  ```

- 使用其他配置文件：如yzyConfig.js，可以通过`--config yzyConfig.js`来指定webpack使用哪个配置文件来执行构建

  ```
  # 通过--config来指定其他配置文件，并按照指定的配置文件的配置内容进行打包  
  webpack --config yzyConfig.js    
  ```

## webpack配置核心概念

- chunk：指代码块，一个chunk可能由多个模块组合而成，也用于代码合并与分割（这里的合并分割主要指指纹策略的判断），指纹策略简单来说就是文件名后的hash

- bundle：资源经过webpack流程解析编译后最终输出的成果文件（一个.js格式的文件，也就是我们的output文件）

- entry：文件打包的入口，webpack会根据entry递归的去寻找依赖，每个依赖都将被它处理，最后打包到集合文件中

- output：配置打包输出的位置、文件名等

- loader：默认情况下，webpack仅支持js和json文件，通过loader，可以让它解析其他类型的文件。理论上只要有相应的loader，webpack可以处理任何类型的文件

- plugin：loader主要的职责是让webpack认识更多的文件类型，而plugin的职责则是让其可以控制构建流程，从而执行一些特殊的任务。插件的功能非常强大，可以完成各种各样的任务

- mode：目标环境，不用的目标环境会影响webpack打包时的决策

- - production：码进行压缩等一系列优化操作
  - development：有利于热更新的处理，识别哪个模块变化代
  - none：什么都不做，打包时会有提示警告

# 配置webpack.config.js

## 项目搭建目标

咱们目标不是搭建一个完整全面的项目工程，目标是以一些有代表性的功能作为切入点学习webpack，我相信这些你认真看过后一定能做到举一反三

- 实现加载css
- 实现css效果展示
- 实现css前缀自动补充
- 实现css以文件形式导出
- 实现自动生成html文件
- 实现打包清空dist文件夹
- 实现图片在js文件中引入
- 实现图片在css文件中引入
- 实现webpack本地服务
- 实现多页面打包

## 实现加载css

我们已经知道.css文件无法正常被webpack打包进bundle（bundle的解释可查看“webpack配置核心概念”部分）文件，所以我们需要一个loader作为加载器将它正确打包进bundle文件

文件内容

![1721470171737](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470171737.png)

安装css-loader

```
yarn add css-loader -D    
```

配置css-loader

```
const path = require('path')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: "development",  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: "css-loader"  
      }  
    ]  
  }  
}
```

运行webpack命令，检查一下dist/main.js中是否包含了css文件中的内容

![1721470193779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470193779.png)

我们发现css文件的内容被成功打包了，这时如果你在dist文件夹下创建了一个html页面给div元素加上了box类，并引入main.js文件，你会发现完全看不到样式效果。

因为此时css中的内容只是被作为一段字符串引入了js中（相当于对css文件的内容进行了JSON.stringify），所以你自然是看不到效果的。

想要看到效果要怎么办？当然是将css内容放进style标签啦！不过这步不需要我们做，因为我们有style-loader为我们做这件事情！

## 实现css效果展示

安装style-loader

```
yarn add style-loader -D    
```

配置style-loader

```
const path = require('path')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: ['style-loader', 'css-loader']  
      }  
    ]  
  }  
}
```

这里需要注意的是，对同一种类型文件使用多个 loader的时候，use属性接收一个数组，并且从右向左执行。所以style-loader要写在css-loader前面

运行webpack命令，看一下结果

![1721470214070](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470214070.png)

成功！

但是我们知道，css3在浏览器中会存在兼容性问题，我们可以通过给属性加上前缀来解决该问题。前端丰富的生态当然不会让你自己傻傻的做这件事情，我们可以通过autoprefixer这个插件帮我们完成

## 实现css前缀自动补充

已经知道autoprefixer是postcss工具的插件，所以我们需要安装postcss和postcss-loader

安装postcss、postcss-loader、autoprefixer，**这里postcss-loader需要指定4.x的版本，因为4.x的版本和webpack4会存在报错问题**

```
yarn add autoprefixer postcss-loader@4.2.0 postcss -D    
```

配置postcss-loader和插件autoprefixer

```
const path = require('path')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          'style-loader',  
          'css-loader',  
          {  
            loader: 'postcss-loader',  
            options: {  
              postcssOptions: {  
                plugins: [require('autoprefixer')]  
              }  
            }  
          }  
        ]  
      }  
    ]  
  }  
}
```

当loader需要写配置的时候，我们可以把loader写成一个对象，loader属性就是要使用的loader名称，options属性就是这个loader的配置对象。autoprefixer是postcss的插件，所以autoprefixer的使用自然就要写在postcss-loader的配置中了

因为postcss有自己的配置文件，所以这里还可以写成这样：

```
// webpack.config.js  
const path = require('path')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: ['style-loader', 'css-loader', 'postcss-loader']  
      }  
    ]  
  }  
}  
  
// 根目录下新建postcss.config.js文件  
module.exports = {  
  plugins: [require('autoprefixer')],  
}
```

这里我们需要配置一下browserslist，否则插件不知道按照什么样的规则进行前缀补全

```
// 在package.json文件中添加  
// 这里的意思表示目标浏览器为ie浏览器，并需要兼容到ie8以上  
"browserslist": ["ie > 8"]    
```

运行`webpack`命令看一下结果

![1721470250936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470250936.png)

成功！

## 实现css以文件形式导出

随着项目的增大，我们不想把那么多的样式都放在style标签中，我们想用link标签引入，这时我们就需要使用**mini-css-extract-plugin**[8]

安装mini-css-extract-plugin

```
yarn add mini-css-extract-plugin -D    
```

配置mini-css-extract-plugin插件和它的loader，这时我们不需要style-loader了，我们要style-loader替换成MiniCssExtractPlugin.loader

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      }  
    ]  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css"  
    })  
  ]  
}
```

- MiniCssExtractPlugin可以配置输出文件名
- [name]为占位符，引入的时候是什么名字，导出的时候就是什么名字
- css/表示导出到css文件夹下

运行`webpack`命令看一下结果

![1721470272778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470272778.png)

成功！

## 实现自动生成html文件

我们发现dist下的html是我们自己手动创建的，这显然不够优雅。**html-webpack-plugin**[9]帮你解决！

安装html-webpack-plugin，这里也要制定一下4.x的版本

```
yarn add html-webpack-plugin@4.5.2 -D    
```

配置html-webpack-plugin

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      }  
    ]  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    })  
  ]  
}
```

运行`webpack`命令看一下结果

![1721470292936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470292936.png)

成功！很显然HtmlWebpackPlugin根据我们的模版为我们生成了新的html页面，并自动引入了dist包下的依赖。查看更多**HtmlWebpackPlugin配置**[10]

## 实现打包清空dist文件夹

我们会发现每次打包dist文件夹的内容会被覆盖，但是如果下次打包出来的文件名不同，那旧的打包文件还会存在，这是我们不想要的。**clean-webpack-plugin**[11]来帮我们解决这个问题

安装clean-webpack-plugin

```
yarn add clean-webpack-plugin -D    
```

配置clean-webpack-plugin

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      }  
    ]  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    }),  
    new CleanWebpackPlugin()  
  ]  
} 
```

这时，你也在dist文件夹下随意建一个其他文件，运行`webpack`命令看一下结果，你会发现你随意建的文件不在了。验证了这一点，就说明你成功了

## 实现图片在js文件中引入

实现这个功能我们使用url-loader，当然你也可以使用file-loader。url-loader是file-loader的升级版，它内部也依赖了file-loader。**file-loader和url-loader在webpack5后都被废弃了，使用asset modules代替**

安装url-loader和file-loader

```
yarn add url-loader file-loader -D    
```

**你可以会疑问为什么要装file-loader，因为url-loader依赖file-loader。若不装，当url-loader将图片转换为base64导入bundle时不会存在问题，但直接输出图片到dist文件夹下就会报错，告诉你缺少file-loader**

配置url-loader

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      },  
      {  
        test: /\.(png|jpe?g|gif)$/,  
        use: {  
          loader: 'url-loader',  
          options: {  
            name: '[name].[ext]',  
            limit: 1024 * 3  
          }  
        }  
      }  
    ]  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    }),  
    new CleanWebpackPlugin()  
  ]  
}
```

- [ext]表示导入时是什么格式，导出时就是什么格式
- limit表示图片大小的阈值，超过阈值的图片会被原封不动的放置到打包文件夹下（file-loader做的这件事情，在js中引入时会帮你生成一个地址，即打包后对应的访问地址），没有超过会处理成base64放在bundle文件中

在入口文件中引入图片

```
// index.js  
import './index.css'  
import mk85 from './assets/images/mk85.jpeg'  
console.log(mk85) // mk85.jpeg  
const img = document.createElement('img')  
img.src = mk85  
const BoxDiv = document.getElementsByClassName('box')  
BoxDiv[0].appendChild(img)
```

运行`webpack`命令看一下结果

![1721470329389](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470329389.png)

## 实现图片在css文件中引入

在css中引入图片我们依旧使用url-loader，但需要对配置稍微进行修改

css代码

```
.box {  
  width: 100px;  
  height: 100px;  
  /* background-color: yellowgreen; */  
  background-image: url("./assets/images/mk85.jpeg");  
  display: flex;  
}   
```

直接引用并打包，打包成功！打开html页面，发现看不到图片，因为地址不对。打包后mk85图片在dist文件夹下，而index.css的引用路径依旧是mk85.jpeg，可index.css是在css文件夹下的，所以自然是无法引用到。

那如何才能引用到呢？最简单的方法就是加上`/`，**但这里有坑（其实也不是坑，这是一个关于/images、./images、image三者有什么不同的知识点）**。补充：create-react-app也是通过`/` 实现引用统一的

![1721470347624](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470347624.png)

修改url-loader配置

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      },  
      {  
        test: /\.(png|jpe?g|gif)$/,  
        use: {  
          loader: 'url-loader',  
          options: {  
            name: '[name].[ext]',  
            limit: 1024 * 3,  
            outputPath: "images/",  
            publicPath: "/images",  
          }  
        }  
      }  
    ]  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    }),  
    new CleanWebpackPlugin()  
  ]  
} 
```

- outputPath表示输出的到哪里（file-loader提供的）
- `name: images/[name].[ext]`这样写和用outputPath设置效果一样吗？在配合publicPath字段时不一样。所以当你不需要配置publicPath字段时，可以通过name设置输出路径（file-loader提供的）

```
 options: {  
    name: '[name].[ext]',  
   limit: 1024 * 3,  
   outputPath: "images/",  
    publicPath: "/images",  
 }  
 
 // 等价于  
 
 options: {  
  name: 'images/[name].[ext]',  
  limit: 1024 * 3,  
  publicPath: "/",  
 } 
```

- publicPath表示资源引用的路径

运行`webpack`命令看一下结果

![1721470373382](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470373382.png)

成功了！是我们想要的结果，不过问题又来了，当你打开html页面时发现并不能看到图片正常显示，这里就牵扯到**关于/images、./images、image三者有什么不同的知识点**

简单来说，如果我起了服务，我的实际路径就是“localhost:8080/images/mk85.jpeg”，如果没有起服务那就是“/images/mk85.jpeg”

所以让我们开启一个服务吧！

## 实现webpack本地服务

安装webpack-dev-server

```
yarn add webpack-dev-server -D    
```

配置url-loader

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          MiniCssExtractPlugin.loader,  
          'css-loader',  
          'postcss-loader'  
        ]  
      },  
      {  
        test: /\.(png|jpe?g|gif)$/,  
        use: {  
          loader: 'url-loader',  
          options: {  
            name: '[name].[ext]',  
            limit: 1024 * 3,  
            outputPath: "images/",  
            publicPath: "/images",  
          }  
        }  
      }  
    ]  
  },  
  devServer: {  
    open: true,  
    port: 8080,  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    }),  
    new CleanWebpackPlugin()  
  ]  
}
```

- 只需要加上devServer配置即可
- open表示打开浏览器
- port表示服务的端口号

**注意：这时就不是使用webpack命令来启动项目了，需使用webpack-dev-server来启动**

## 实现多页面打包

顾名思义，多页面自然是有多个html页面，每个html页面都有自己的js文件，那么，**有多少个入口就要有多少个出口**

我们首先要设置一下目录形式，以适应多页面打包的形式（以下形式不是唯一的，但有助于大家的理解）

![1721470400262](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470400262.png)

- 这里不需要src/index.js
- 新建一个webpack.multiple.config.js
- 新建src/pages/login/js/index.js
- 新建src/pages/main/js/index.js

安装glob，用于处理文件

```
yarn add glob -D    
```

配置webpack.multiple.config.js

```
module.exports = {  
  entry: {  
    login: './src/pages/login/js/index.js',  
    main: './src/pages/main/js/index.js'  
  },  
  output: {  
    filename: '[name].js',  
    path: path.resolve(__dirname, './dist')  
  },  
  plugins: [  
    new HtmlWebpackPlugin({  
      template: './src/index.html',  
      filename: 'login.html',  
      chunks: ['login'] // chunks的名字对应entry中的名字  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html',  
      filename: 'main.html',  
      chunks: ['main']  
    })  
  ]  
}
```

这样就完成了！你可以使用`webpack --config ./`webpack.multiple.config.js` 命令运行一下。结果会如你所愿的

但是，这时你肯定会想难道我每写一个页面就重新配置一次吗？这也太麻烦了，也不优雅！那我们现在解决一下这个问题吧，直接上代码

```
// 我们写一个方法自动做我们上面配置的事情  
const glob = require("glob")  
  
const setMpa = () => {  
  const entry = {}  
  const htmlwebpackplugins = []  
 // 通过glob库拿到我们的入口文件数组  
  const entryFiles = glob.sync(path.resolve(__dirname, "./src/pages/*/*/index.js"))  
 // console.log(entryFiles)  
  // 打印结果  
  // [  
  //  '/Users/yzy/Desktop/learnSpace/learnWebpack/src/pages/login/js/index.js',  
  //  '/Users/yzy/Desktop/learnSpace/learnWebpack/src/pages/main/js/index.js'  
  // ]  
  entryFiles.forEach((item) => {  
    const entryFile = item  
    const match = entryFile.match(/src\/pages\/(.*)\/js\/index\.js$/)  
    // console.log(match)  
    // 打印结果  
    // [  
    //   'src/pages/login/js/index.js',  
    //   'login',  
    //   index: 43,  
    //   input: '/Users/yzy/Desktop/learnSpace/learnWebpack/src/pages/login/js/index.js',  
    //   groups: undefined  
    // ]  
    const pageName = match[1]  
    entry[pageName] = entryFile  
    htmlwebpackplugins.push(  
      new HtmlWebpackPlugin({  
        template: `./src/index.html`,  
        filename: `${pageName}.html`,  
        chunks: [pageName]  
      })  
    )  
  })  
  
  return {  
    entry,  
    htmlwebpackplugins,  
  }  
}  
```

有了这个方法以后，我们把它加到配置文件里

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
const glob = require("glob")  
  
const setMpa = () => {  
  const entry = {}  
  const htmlwebpackplugins = []  
  const entryFiles = glob.sync(path.resolve(__dirname, "./src/pages/*/*/index.js"))  
  entryFiles.forEach((item) => {  
    const entryFile = item  
    const match = entryFile.match(/src\/pages\/(.*)\/js\/index\.js$/)  
    const pageName = match[1]  
    entry[pageName] = entryFile  
    htmlwebpackplugins.push(  
      new HtmlWebpackPlugin({  
        template: `./src/index.html`,  
        filename: `${pageName}.html`,  
        chunks: [pageName]  
      })  
    )  
  })  
  
  return {  
    entry,  
    htmlwebpackplugins,  
  }  
}  
const { entry, htmlwebpackplugins } = setMpa()  
module.exports = {  
  entry,  
  output: {  
    filename: '[name].js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.css$/,  
        use: [  
          'style-loader',  
          'css-loader',  
          'postcss-loader'  
        ]  
      },  
      {  
        test: /\.(png|jpe?g|gif)$/,  
        use: {  
          loader: 'url-loader',  
          options: {  
            name: '[name].[ext]',  
            limit: 1024 * 3,  
            outputPath: "images/",  
            publicPath: "/images",  
          }  
        }  
      }  
    ]  
  },  
  devServer: {  
    open: true,  
    port: 8080,  
    hot: true  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new CleanWebpackPlugin(),  
    ...htmlwebpackplugins  
  ]  
}
```

我们再使用`webpack --config ./`webpack.multiple.config.js` 命令运行一下，成功！

## 小结

到这里就算是完成了一个简单的webpack项目配置，看到这里先不要着急往下看，思考一下是否真的了解了loader和plugin，如果让你写一个loader和plugin，你有思路吗

我想不出意外的话，你应该是已经有了思路。如果没有也不用担心，看看下面的内容

# 实现一个loader

先上链接，**官网编写loader文档**[12]

首先loader是一个函数，注意这里不能是箭头函数

编写一个替换字符串的loader

```
// replaceLoader.js  
module.exports = function (soure) {  
  console.log(soure, this) // 这里可以自己打印看一下信息，内容太长我就不放进来了  
 return source.replace('hello webpack', "你好呀，webpack！")  
}    
```

- loader是一个函数
- 不能使用箭头函数，因为要用到上下文的this
- soure接收到的是待处理的文件源码
- return处理后的文件源码，也是下一个loader接收到的文件源码

使用loader

```
{  
  test: /\.js$/,  
  use: path.resolve(__dirname, './loader/replaceLoader.js')  
} 
```

运行`webpack`命令看一下结果

![1721470509556](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470509556.png)

成功！是不是发现原来自定义loader这么简单，感兴趣可以自己尝试写一下css、png等其他文件的loader

# 实现一个plugin

线上链接，**官网编写plugin文档**[13]

同样简单，我们已经用了很多次plugin了，发现是不是都需要new一下。很显然，自定义loader是一个构造函数。

我们看一下格式：

```
class PluginName {  
 constructor (options) {  
 }  
 apply(compiler) {  
  ...  
 }  
}
```

- 这里一定要写apply方法，webpack会通过apply方法启动插件
- PluginName可以写成普通function，apply必须挂载到原型对象上（`PluginName.prototype`）
- class类中的apply不能写成箭头函数
- webpack的**compiler钩子**[14]，查看钩子决定你的插件要在哪一步做什么

编写一个假的html-webpack-plugin，输出一个fake.html文件

```
class HtmlPlugin {  
 constructor (options) {  
 }  
 apply(compiler) {  
  compiler.hooks.emit.tap('HtmlPlugin', (compolation) => {  
      const content = '<html><body>fake html</body></html>'  
      compolation.assets['fake.html'] = {  
        source: function () {  
          return content  
        },  
        size: function () {  
          return content.length  
        }  
      }  
    })  
 }  
}  
module.exports = HtmlPlugin
```

使用这个plugin

```
plugins: [    new HtmlPlugin()  ]    
```

运行`webpack`命令看一下结果

![1721470536649](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721470536649.png)

成功！你也可以试着完善一下这个插件，加上配置，加上引入资源文件等

# 指纹策略

## 关于浏览器缓存

- **现代浏览器**都会有缓存机制

- 当我们第一次访问A网站时，这时加载了y.js的文件进行了缓存

- 当我们第二次访问A网站时，浏览器发现缓存中已经有y.js了

- - 缓存中有y.js那就用缓存的文件
  - 优点：减少了资源的请求
  - 缺点：当y.js的内容更新了，若不通过强制刷新浏览器的话则无法获取最新的y.js内容

- 我们加上标识符就可以解决这个问题了

- - 第一次访问时加载了y.123.js
  - 第二次访问发现有缓存就用缓存中的y.123.js
  - 这时服务器中的y文件内容改变了，同时也修改了名字为y.111.js
  - 第三次访问发现没有y.111.js文件，正确加载最新y.111.js

上诉都是比较简单解释，具体细节你可以不用知道，明白这个缓存机制的缓存方式即可

## webpack中使用指纹策略

使用：

```
filename: '[name].[hash].[ext]',    
```

- hash：以项目为单位，项目内容改变了，则会生成新的hash，内容不变则hash不变

- - 整个工程任何一个需要被打包的文件发生了改变，打包结果中的所有文件的hash值都会改变

- chunkhash：以chunk为单位，当一个文件内容改变，则整个chunk组的模块hash都会改变

- - 假设打包出口有a.123.js和c.123.js
  - a文件中引入了b文件
  - 修改了b文件的内容
  - 重新的打包结果为a.111.js和c.123.js
  - a的hash值会被影响，但是c的hash值不受影响

- contenthash：以自身内容为单位，依赖不算

- - 假设打包出口有a.123js和b.123.css
  - a文件引入了b文件
  - 修改了b文件的内容
  - 重新打包结果为a.123.js和b.111.css
  - a的hash值不受影响

# 热模块替换

你一定使用过这个功能，只是你不知道罢了！

场景：

- 启动项目本地服务
- 修改了一个.css文件的内容
- 浏览器没有刷新，但是修改的内容还是生效了

这就是热模块替换，提示：无论是css还是js都可以做热模块替换，但是个人建议只做css的热模块替换即可。因为js的热模块替换需要写代码进行替换，除非特定情况下，否则js的热模块替换用处不大。

我们来做一个css的热模块替换吧

**注：热模块替换不支持抽取出的css文件，只能放在style中，所以需要style-loader**

配置webpack.config.js

```
const path = require('path')  
const MiniCssExtractPlugin = require('mini-css-extract-plugin')  
const HtmlWebpackPlugin = require('html-webpack-plugin')  
const { CleanWebpackPlugin } = require('clean-webpack-plugin')  
const HtmlPlugin = require('./plugin/htmlPlugin')  
const Webpack = require('webpack')  
  
module.exports = {  
  entry: './src/index.js',  
  output: {  
    filename: 'main.js',  
    path: path.resolve(__dirname, './dist')  
  },  
  mode: 'development',  
  module: {  
    rules: [  
      {  
        test: /\.js$/,  
        use: path.resolve(__dirname, './loader/replaceLoader.js')  
      },  
      {  
        test: /\.css$/,  
        use: [  
          // MiniCssExtractPlugin.loader,  
          'style-loader',  
          'css-loader',  
          'postcss-loader'  
        ]  
      },  
      {  
        test: /\.(png|jpe?g|gif)$/,  
        use: {  
          loader: 'url-loader',  
          options: {  
            name: '[name].[ext]',  
            limit: 1024 * 3,  
            outputPath: "images/",  
            publicPath: "/images",  
          }  
        }  
      }  
    ]  
  },  
  devServer: {  
    open: true,  
    port: 8080,  
    hot: true  
  },  
  plugins: [  
    new MiniCssExtractPlugin({  
      filename: "css/[name].css",  
    }),  
    new HtmlWebpackPlugin({  
      template: './src/index.html'  
    }),  
    new CleanWebpackPlugin(),  
    new HtmlPlugin(),  
    new Webpack.HotModuleReplacementPlugin()  
  ]  
} 
```

思考了一下，还是附了一份完整的配置文件（有点担心这样太占页面位置了）

- 首先，将之前的MiniCssExtractPlugin.loader替换回style-loader
- 给devServer中加上`hot: true`
- 引入webpack: `const Webpack = require('webpack')`
- 在plugin中添加：`new Webpack.HotModuleReplacementPlugin()`

运行`webpack-dev-server`命令看看效果吧！

# webpack5都更新了什么

话不多说，直接上链接，**webpack5更新的内容**[15]

**提示：当你使用webpack4的时候，无论是装plugin（插件）还是loader都尽量看看该插件现在都有什么版本，如果发现版本从4.x一下子跳到了5.x，那么一定要去安装4.x的版本，否则在打包的时候会发生未知的错误**

### 参考资料

[1]https://github.com/postcss/autoprefixer[2]https://github.com/postcss/postcss[4]https://www.babeljs.cn/docs/plugins[6]https://github.com/browserslist/browserslist[8]https://webpack.docschina.org/plugins/mini-css-extract-plugin/[10]https://github.com/jantimon/html-webpack-plugin#options[12]https://webpack.docschina.org/contribute/writing-a-loader/[14]https://webpack.docschina.org/api/compiler-hooks/

