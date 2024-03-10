### 创建项目

#### 1. **准备阶段**

##### (1) **安装node**

##### (2) **安装git**

##### (3) **安装vue**

#### 2. **创建项目**

创建一个文件夹 ，打开终端 

vue create xxx 

\> Manually select features 

(*) router ...

2.x 

n 

\> ESLint + Prettier 

 Lint on save 

 In dedicated config files 

 n 

#### 3. **创建项目后添加插件vue add xxx**

如果你想在一个已经被创建好的项目中安装一个插件，可以使用 vue add 命令   vue add eslint   

 	如果出于一些原因你的插件列在了该项目之外的其它 package.json 文件里，你可以在自己项目的 package.json 里设置 vuePlugins.resolveFrom 选项指向包含其它 package.json 的文件夹。 

例如，如果你有一个 .config/package.json 文件： 

{ 

  "vuePlugins": {  "resolveFrom": ".config"  } 

} 

##### (1) **下载依赖npm i xx 与 vue add xx区别**

1、vue add可能会改变现有的项目结构，但是npm install仅仅是安装包而不会改变项目的结构。

2、add如果你下载的库, 特别是 Ui 库, 希望对脚手架结构产生影响,那就选择vue add xxx

3、npm如果不希望对脚手架结构产生影响, 只是单纯的使用, 比如 axios 这个插件，那就选择npm install xxx

vue add 除了会 npm install 之外，还会帮你配置好一个范例文件。需要注意的是這个指令会更改你现有的文件內容。特別的是使用 vue add router 或是 vue add vuex，他们虽然不是插件，但Vue CLI会帮你配置好文件，例如 vue add router 会帮你配置 router.js 文件以及生成 About.vue 和 Home.vue 并在 App.vue 內建立了简单的路由范例，而 vue add vuex 会帮你配置好一个 store.js 文件。

#### 4. **项目目录解析**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps34.jpg) 

.git 链接的是尤雨溪的仓库，删除 

node_modules 存放第三方包的目录 

public 站点目录 

src 开发目录 

.gitignore git忽视的配置 

babel.config.js 高级语法降级的配置 

package.json 包管理文件 

package-lock.json 对包的版本进行锁定 

README.md  

vue.config.js 项目配置 

##### (1) **Public/asset**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps35.png)

##### (2) **Package.json项目依赖**

{

  "name": "ruoyi",

  "version": "3.8.1",

  "description": "东兴市智慧化管理平台",

  "author": "新助力",

  "license": "MIT",

  "scripts": {

​    "dev": "vue-cli-service serve",

​    "build:prod": "vue-cli-service build",

​    "build:stage": "vue-cli-service build --mode staging",

​    "preview": "node build/index.js --preview",

​    "lint": "eslint --ext .js,.vue src"

  },

  "husky": { },

  "lint-staged": { },

  "keywords": [  ],

  "repository": { },

  "dependencies": { },  //生产环境 -S --save

  "devDependencies": { }, //开发环境 -D --dev

  "engines": { },

  "browserslist": [ ]

}

 

Script-----脚本命令：

npm run xxx 运行指令xxx;

如npm run dev 运行的就是 "vue-cli-service serve"

--mode 指 模式

Build 打包 不同的模式 后文件内容是不同的，生产模式打包，没有注释等

**Ps：**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps36.png)

 "husky": {

​    "hooks": {

​      "pre-commit": "lint-staged"

​    }

  },

 

"lint-staged": {

​    "src/**/*.{js,vue}": [

​      "eslint --fix",

​      "git add"

​    ]

  },

"keywords": [

​    "vue",

​    "admin",

​    "dashboard",

​    "element-ui",

​    "boilerplate",

​    "admin-template",

​    "management-system"

  ],

"repository": {

​    "type": "git",

​    "url": "https://gitee.com/y_project/RuoYi-Vue.git"

  },

"engines": {

​    "node": ">=8.9",

​    "npm": ">= 3.0.0"

  },

"browserslist": [

​    "> 1%",

​    "last 2 versions"

  ]

 

##### (3) **package-lock.json 锁定版本**

锁定安装时的包的版本号，并且需要上传到git，以保证其他人在npm install时大家的依赖能保证一致。

package.json文件只能锁定大版本，也就是版本号的第一位，并不能锁定后面的小版本，你每次npm install都是拉取的该大版本下的最新的版本，为了稳定性考虑我们几乎是不敢随意升级依赖包的，这将导致多出来很多工作量，测试/适配等，所以package-lock.json文件出来了，当你每次安装一个依赖的时候就锁定在你安装的这个版本。

**那如果我们安装时的包有bug，后面需要更新怎么办？**

在以前可能就是直接改package.json里面的版本，然后再npm install了，但是5版本后就不支持这样做了，因为版本已经锁定在package-lock.json里了，所以我们只能npm install [xxx@x.x.x](mailto:xxx@x.x.x) 这样去更新我们的依赖，然后package-lock.json也能随之更新。

**假如我已经安装了jquery 2.1.4这个版本，从git更新了package.json和package-lock.json，我npm install能覆盖掉node_modules里面的依赖吗?**

在直接更新package.json和package-loc.json这两个文件后，npm install是可以直接覆盖掉原先的版本的，所以在协作开发时，这两个文件如果有更新，你的开发环境应该npm install一下才对。

##### (4) **package-lock.json与package.json区别**

package.json 

 

生成方式：执行 npm init 命令。

主要作用：描述项目及项目所依赖的模块信息。

 

 

package-lock.json  锁定版本，防止小版本熬制错误

 

生成方式：从 npm 5 版本之后只要使用 npm install 命令下载，就会自动生成 package-lock.json 文件。

 

主要作用：

1）描述 node_modules 文件中所有模块的版本信息，模块来源及依赖的小版本信息。

2）当版本升级，使用 npm install 命令时，会安装 package.json 中指定的大版本的最新版本。

如 package.json 中指定版本"dependencies": { "webpack": "^2.0.0" }，

则 package-lock.json 会按照 {"webpack": "2.7.0"} 版本升级。

在保证大版本号前提下的最新版本。

webpack "2.7.0" 是 "2.x.x" 的最高版本。

 

区别：

当项目中已有 package-lock.json 文件，

在安装项目依赖时，将以该文件为主进行解析安装指定版本依赖包，

而不是使用 package.json 来解析和安装模块。

因为 package-lock 为每个模块及其每个依赖项指定了版本，位置和完整性哈希，

所以它每次创建的安装都是相同的。

无论你使用什么设备，或者将来安装它都无关紧要，每次都应该给你相同的结果。

 

注意 cnpm 不支持 package-lock

使用 cnpm install 时候，并不会生成 package-lock.json 文件。

cnpm install 的时候，就算你项目中有 package-lock.json 文件，cnpm 也不会识别，

仍会根据 package.json 来安装。所以这就是为什么之前你用 npm 安装产生了 package-lock.json，

后面的人用 cnpm 来安装，可能会跟你安装的依赖包不一致。

 

因此，尽量不要直接使用 cnpm install 安装项目依赖包。

但是为了解决直接使用 npm install 速度慢的问题，**可以设置 npm 代理解决**。

 

https://blog.csdn.net/qianyu6200430/article/details

 

#### 5. **初期设置**

##### (1) **关闭eslint检测（会出现经常报错）**

在根目录下创建 vue.config.js 文件，注意文件名不能乱改，然后添加以下代码 

module.exports = { 

lintOnSave:false 

} 

![1709176985428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709176985428.png)

##### (1) **不同环境的指令配置**

用 参数 --mode 来区分不同环境指令 https://cli.vuejs.org/zh/guide/mode-and-env.html 

 "scripts": { 

​    "serve": "vue-cli-service serve --mode development", //开发环境 

​    "test":"vue-cli-service serve --mode test",//测试环境 

​    "build": "vue-cli-service build --mode production",//生产环境 

​    "lint": "vue-cli-service lint" 

  } 

##### (2) **不同环境api的配置**

vue-cli 中的指南  模式和环境变量 

\#表示注释  环境变量就是常量 

\#开发环境变量  在.env.development文件中写 

\#测试环境变量  .env.test 

\#生产环境变量  .env.production 

文件与src同级 

变量 以 VUE_APP_ 开头  

例如： 开发环境变量 VUE_APP_API = 'http://127.0.0.1:3000' 

​      测试环境变量 VUE_APP_API = 'http://127.0.0.1:5000' 

 

在vue.config.js中 console.log(process.env) (可查看NODE_ENV 运行环境） 

​                   console.log(process.env.VUE_APP_API) (可查看变量的值） 

 devServer:{ 

​    proxy:{ 

​      '/api':{ 

​        target:process.env.VUE_APP_API, 

​        changeOrigin:true, 

​        pathRewrite:{ 

​          '/api':"" 

​        } 

​      } 

​    } 

  }   

 运行这两服务器，axios.get('/api') 

 然后 npm run serve  /  npm run test  就会打印出来 

### style

#### 1. **样式穿透**

样式穿透主要应用场景是在项目中运用UI组件库（element, vant, vuetify）后要改动其内在样式，因此采用深度选择器 

三大样式穿透 >>> , ::v-deep , /deep/ 

 

 \>>> 只作用于css 

::v-deep 只作用于sass 

/deep/ 只作用于less 

#### 2. **css预处理**

##### (1) **Less**

优雅减级 

npm i less less-loader@7.3.0 

在组件样式中添加 lang="less" 就能使用less 写法了 

##### (2) **Sass** 

npm i node-sass sass-loader style-loader -D 

#### 3. **Reset style** 

//assets/style/index.css  在main.js 中引入 

html, body, div, span, applet, object, iframe, h1, h2, h3, h4, h5, h6, p, blockquote, pre, a, abbr, acronym, address, big, cite, code, del, dfn, em, img, ins, kbd, q, s, samp, small, strike, strong, sub, sup, tt, var, b, u, i, center, dl, dt, dd, ol, ul, li, fieldset, form, label, legend, table, caption, tbody, tfoot, thead, tr, th, td, article, aside, canvas, details, embed, figure, figcaption, footer, header, hgroup, menu, nav, output, ruby, section, summary, time, mark, audio, video{ margin: 0; padding: 0; border: 0; font-size: 100%; font: inherit; font-weight: normal; vertical-align: baseline; } /* HTML5 display-role reset for older browsers */ article, aside, details, figcaption, figure, footer, header, hgroup, menu, nav, section{ display: block; } ol, ul, li{ list-style: none; } blockquote, q{ quotes: none; } blockquote:before, blockquote:after, q:before, q:after{ content: ''; content: none; } table{ border-collapse: collapse; border-spacing: 0; } /* custom */ a{ color: #7e8c8d; text-decoration: none; -webkit-backface-visibility: hidden; } ::-webkit-scrollbar{ width: 5px; height: 5px; } ::-webkit-scrollbar-track-piece{ background-color: rgba(0, 0, 0, 0.2); -webkit-border-radius: 6px; } ::-webkit-scrollbar-thumb:vertical{ height: 5px; background-color: rgba(125, 125, 125, 0.7); -webkit-border-radius: 6px; } ::-webkit-scrollbar-thumb:horizontal{ width: 5px; background-color: rgba(125, 125, 125, 0.7); -webkit-border-radius: 6px; } html, body{ width: 100%; font-family: "Arial", "Microsoft YaHei", "黑体", "宋体", "微软雅黑", sans-serif; } body{ line-height: 1; -webkit-text-size-adjust: none; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); } html{ overflow-y: scroll; } /*清除浮动*/ .clearfix:before, .clearfix:after{ content: " "; display: inline-block; height: 0; clear: both; visibility: hidden; } .clearfix{ *zoom: 1; } /*隐藏*/ .dn{ display: none; }

### 图片、图标引入

#### 1. **图片引入**

第一种 

​    data(){ 

​        return{ 

​          imgsrc:require('../../../assets/huawei_logo.png') 

​        } 

​      } 

<img :src="imgsrc" alt="" class="logo">

 

第二种 

​    import uawei_logo from '../../../assets/huawei_logo.png'; 

​    data(){ 

​        return{ 

​          imgsrc:uawei_logo 

​        } 

​      } 

   <img :src="imgsrc" alt="" class="logo">

 

 第三种 @/ + src 

     <img src="@/assets/hwcloud-promotion-04-p.jpg" alt="">

 

#### 2. **图标引入**

在阿里巴巴矢量图标库中选好项目需要的图标：

第一种 ：网上地址url :  在css中引入； @import url(‘url’)   

第二种 : 复制网页上的代码→创建.css →黏贴→ 在mian.js中引入   

第三种 ：下载到本地→复制文件（除了demo.css、demo.html）→ 放到项目中→在main.js中引入

### 开发问题

#### 1. **解决vue路由重复导航的错误**

// 获取原型对象上的push函数 

const originalPush=VueRouter.prototype.push 

// 修改原型对象中的push方法 

VueRouter.prototype.push=function(location){ 

  return originalPush.call(this,location).catch(err=>err) 

} 

#### 2. **vue前端** **跨域解决方案** 

旧版 ：建一个 vue.config.js 

module.exports={ 

... 

devServer:{ 

host:"localhost",//本地服务 

port:8080, //本地服务 

proxy:{ //代理 

"/api":{ 

target:"http://127.0.0.1:3000/",//后台的接口 

changeOrigin:true, //允许跨域 

pathRewrite:{ //重写路径 

"^/api":"/" 

} 

} 

} 

}

}

@vue/cli 5.0  vue.config.js 

const { defineConfig } = require("@vue/cli-service"); 

module.exports = defineConfig({ 

  transpileDependencies: true, 

  lintOnSave:false,//关闭lint 检查 

  devServer: { 

​    proxy: {//代理 

​      "/api":{//代理后台接口的别名 

​        target:"http://127.0.0.1:2000/",//访问的后台接口 

​        changeOrigin:true,//允许跨域 

​        pathRewrite:{//重写路径 

​          '/api':"" 

​        } 

​      } 

​    } 

  } 

}); 

请求参数  axios.get('/api') 

### Mock

#### 1. **Mock**

mock 的意思是模拟，也就是模拟接口返回的信息，用已有的信息替换它需要返回的信息，从实现对上级模块的测试。

#### 2. **使用**

npm i mockjs 

 

// 使用 Mock 

**var Mock = require('mockjs')** 

 

**Mock.mock()** 

**Mock.mock( rurl?, rtype?, template|function( options ) )** 

\1. **Mock.mock( template )** 根据数据模板生成模拟数据。 

​    Mock.mock({ 

​      name:"zhangsan", 

​      age:18 

​    }) 

**2.Mock.mock( rurl, template )**当拦截到匹配rurl 的 Ajax 请求时，将根据数据模板template 生成模拟数据，并作为响应数据返回。 

​    //需要配置环境条件(在 main.js中) 

​    if(process.env.NODE_ENV==='development'){ 

​      //如果 是 开发环境，那么 就会拦截ajax请求 

​        require('./mock/index')//mock/index.js  是统一管理所有的mock 

​    }     

​    //home页面的请求 

​    axios.get('/api/home').then(res=>console.log(res)) 

​    //home页面的mock 

​    let Mock = require('mockjs'); 

​    Mock.mock('/api/about',['z','l','w']) 

注意：请求路径 与 拦截路径 要一致 

**3.Mock.mock( rurl, rtype, template )** 

**4. Mock.mock( rurl, rtype, fonction(config){return config})** 对数据进行操作 

​    axios.delete('/api/delete/'+id)    

​    delete 适合用 正则 

​    Mock.mock(/\/api\/delete\/\d/,'delete',function(obj){} 

#### 3. **引入mockjs 模拟接口**

npm i mockjs 

//src/mock/index.js 

let Mock = require('mockjs') 

let arrs = ['王赛军','小全'] 

Mock.mock('/get','get',arrs) 

//main.js 

if(process.env.NODE_ENV==='development'){ 

  require('./mock') 

} 

#### 4. **数据模板定义规范**

**网址**：[https://github.com/nuysoft/Mock/wiki/Syntax-Specification#数据模板定义规范-dtd"](https://github.com/nuysoft/Mock/wiki/Syntax-Specification#数据模板定义规范-dtd\)

数据模板中的每个属性由 3 部分构成：属性名、生成规则、属性值：

// **属性名   name**

// **生成规则 rule**

// **属性值   value**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

· 属性名 和 生成规则 之间用**竖线 | 分隔**。

· 生成规则 是可选的。

· 生成规则 有 7 种格式： 

\1. 'name|min-max': value

\2. 'name|count': value

\3. 'name|min-max.dmin-dmax': value

\4. 'name|min-max.dcount': value

\5. 'name|count.dmin-dmax': value

\6. 'name|count.dcount': value

\7. 'name|+step': value

**·** **生成规则 的 含义 需要依赖 属性值的类型 才能确定。** **属性值value决定了有哪些生成规则**

· 属性值 中可以含有 @占位符。

· 属性值 还指定了最终值的初始值和类型。

##### (1) **属性值是字符串 String**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name|min-max': string**

通过重复 string 生成一个字符串，重复次数大于等于 min，小于等于 max。

**'name|count': string**

通过重复 string 生成一个字符串，重复次数等于 count。

##### (2) **属性值是数字 Number**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name|+1': number**

属性值自动加 1，初始值为 number。

**'name|min-max': number**

生成一个大于等于 min、小于等于 max 的整数，属性值 number 只是用来确定类型。

**'name|min-max.dmin-dmax': number**

生成一个浮点数，整数部分大于等于 min、小于等于 max，小数部分保留 dmin 到 dmax 位。

##### (3) **属性值是布尔型 Boolean**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name|1': boolean**

随机生成一个布尔值，值为 true 的概率是 1/2，值为 false 的概率同样是 1/2。

**'name|min-max': value**

随机生成一个布尔值，值为 value 的概率是 min / (min + max)，值为 !value 的概率是 max / (min + max)。

##### (4) **属性值是对象 Object**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name|count': object**

从属性值 object 中随机选取 count 个属性。

**'name|min-max': object**

从属性值 object 中随机选取 min 到 max 个属性。

##### (5) **属性值是数组 Array**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name|1': array**

从属性值 array 中随机选取 1 个元素，作为最终值。

**'name|+1': array**

从属性值 array 中顺序选取 1 个元素，作为最终值。

**'name|min-max': array**

通过重复属性值 array 生成一个新数组，重复次数大于等于 min，小于等于 max。

**'name|count': array**

通过重复属性值 array 生成一个新数组，重复次数为 count。

##### (6) **属性值是函数 Function**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name': function**

执行函数 function，取其返回值作为最终的属性值，函数的上下文为属性 'name' 所在的对象。

##### (7) **属性值是正则表达式 RegExp**

**'name****属性名** **|** **rule****生成规则****': value****属性值****（****rule****）**

**'name': regexp**

根据正则表达式 regexp 反向生成可以匹配它的字符串。用于生成自定义格式的字符串。

Mock.mock({

​    'regexp1': /[a-z][A-Z][0-9]/,

​    'regexp2': /\w\W\s\S\d\D/,

'regexp3': /\d{5,10}/

})

// =>{

​    "regexp1": "pJ7",

​    "regexp2": "F)\fp1G",

"regexp3": "561659409"

}

### B（business）端项目

#### 1. **组件库**

##### (1) **Element-UI**

###### ①　**引入**

**1)** **全局引入**

安装 npm i element-ui  

main.js 

​    import ElementUI from 'element-ui' 

​    import 'element-ui/lib/theme-chalk/index.css' 

​     

​    Vue.use(ElementUI)  全局引入后可随意使用

**2)** **按需引入 (也可以全部引入)**

**A.** **Vue add xx**

vue add element  --> 生成一个plugin文件：element.js

element.js:  只有一个 Button 组件，想要全部引入，在官网>快速上手，复制 ‘引入方式’中的代码

→→放在 src/plugins/element.js 中

→→把不使用的删掉

→→main.js  引入  import ‘./plugins/element.js’

**B.** **npm i xx**

npm i element-ui 

借助 [babel-plugin-component](https://github.com/QingWei-Li/babel-plugin-component)，我们可以只引入需要的组件，以达到减小项目体积的目的。 

首先，安装 babel-plugin-component： 

npm install babel-plugin-component -D 

我们最新Vue-cli生成的项目文件已经不叫.babelrc了 ->>>>>>> 生成的文件叫babel.config.js 

在官网提供的配置项中 "presets": [["es2015", { "modules": false }]], 没有及时的更新 

在老版的脚手架中 确实应该写["es2015", { "modules": false }] 

但是在新版的脚手架中应该写为["@babel/preset-env", { modules: false }] 

然后，将babel.config.js文件修改为： 

 

module.exports = { 

presets: [ 

'@vue/cli-plugin-babel/preset', 

[ 

"@babel/preset-env", { modules: false } 

] 

], 

"plugins": [ 

[ 

"component", 

{ 

"libraryName": "element-ui", 

"styleLibraryName": "theme-chalk" 

} 

] 

] 

}

**通常我们会在src目录下新建一个element-ui文件夹 里面创建一个index.js文件** 

在文件中导入你需要的组件并使用 

import Vue from 'vue' 

import { Button, Select } from 'element-ui' 

// Vue.component(Button.name, Button) 

// Vue.component(Select.name, Select) 

// 或写为一下形式 

Vue.use(Button) 

Vue.use(Select) 

在mian.js中引入文件就好了 

import './element-ui/index.js' 

用哪个组件就看哪个组件的说明文档。

 

易错点

[Vue warn]: Unknown custom element: ＜el-carousel-item＞ - did you register the component correctly? 

结果仔细一看，原来使用走马灯时不仅carousel 这个要注册，carousel-item 这个也要注册，注册完就能正常显示了。

![1709177213958](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709177213958.png)

###### ①　**组件的运用**

**1)** **Form 表单**

form 组件的的数据使用 model 属性绑定，（不要单独的绑定在input 等上） 

rules 对form表单进行正则校验的 

#### 1. **真实项目：外汇项目（内网开发）**

##### (1) **项目**

主项目--protolUI

子项目

##### (2) **从gitlab上拉项目**

确定任务，从那个分支上拉取代码，克隆到本地开发

##### (3) **依赖/包管理**

更换镜像源：

更换镜像源为公司地址：npm config set registry http://nexus.safewx.cn/repository/npm_group/

**若项目有下载好的依赖，那么把依赖复制到项目中去；而不必更换镜像源，在执行npm i**

查看镜像源地址：npm config get registry

上传文件到npm

更换上传地址：npm config set registry [http://nexus.safewx.cn/repository/npm_hosted/](http://nexus.safewx.cn/repository/npm_group/)

登录用户：npm login xxx

上传文件到npm：npm publish

##### (4) **本地运行项目**

**克隆父项目到本地，npm i**

**1.** **index.html**

<!-- 生产文件线上打开-->

<script src=”/js/jsonfile.js”></script> ----->注释掉

<!-- 开发测试文件上线--> 去注释

<script type=”systemjs-importmap” src=”http://10.10.100.163:8083/testuse.json”></script> 将ip改为本地ip 

 

**2.** **public/nginx/testuse.json**

找到对应的子项目名，如IBOR

“IBOR”:”http://10.10.100.163:7781/app.js”   **将ip改为本地ip**

 

**3.** **Public/nginx/url_config.js**

“PORTAL_URL”:”http://10.10.100.200:5001”,

“JSONSRC”:”http://10.10.100.200:5001/api/v1/webmodule/common”

**修改ip 为服务器 ip**

 

**4.** **克隆子项目本地** 

npm i

npm run spa-serve

**终端查看ip ：ipconfig；父子项目启动后凡是public/nginx/testuse.json 的url报错，都可以考虑换成本地ip**

##### (5) **代码提交**

**代码提交时，注意是否有提交规范：**

本次项目提交规范：在提交注释时，需要  git commit -m ‘fix:修复bug’  不同的前缀 意义不同

##### (6) **Axios封装**

import axios from ‘axios’

import { Message } from ‘element-ui

/**

\* @description: 错误提示

\* @param {msg} String 错误提示文字

\* @return {*}

*/

const tip = (msg) => {

  Message.error({

  showClose: true,

  message: msg,

  type: ‘error’

})

}

/**

\* @description: 请求失败后的统一处理

\* @param {Number} status 请求失败码 other 其他错误

\* @return {*}

*/

const errorHandle = (status, other) => {

  switch (status) {

  case 500:

​    tip(‘服务器错误’)

​    break;

  case 404:

​    tip(‘网络请求不存在’)

​    break;

  case 401:

​    localStorage.clear()

​    window.location.herf = ‘/’

​    break;

  default:

​    tip(‘其他错误’)

​    break;

}

}

 

let pending = [] //声明一个数组用于存储每个ajax请求的取消函数和ajax标识

 

/**

\* @description: 如果pending中纯在当前请求，就终止上一次请求 执行本次请求

\* @param {object} ever 请求配置参数

\* @return {*}

*/

 

let removePending = (ever) => {

  pending.forEach((element, index) => {

  if(element.u === ever.url + ‘&’+ever.method) {

  //当 当前请求在数组中存在时执行函数

  element.f() //执行取消操作

  pending.splice(index, 1)

}

})

}

 

// 创建axios 实例

const _axios = axios.creat({

  Timeout: 60*10000,

})

 

// 设置post 请求头

_axios.defaults.headers.post[‘Content-Type’] = ‘application/x-www-form-urlencoded;charset=UTF-8’

 

//请求拦截器

_axios.incerceptors.request.use(

(config) => {

​      //token处理

​      if(localStorage.LoginData) {

​      config.headers.Authorization = `Bearer ${ JSON.parse(localStorage.LoginData).accessToken }`

}

// 取消重复点击的请求

removePending(config)

config.cancelToken = new axios.CancelToken(c => {

​      pending.push({ u: config.url+’&’+config.method, f: c })

})

return config

},

(error) => {

  return Promise.reject(error)

}

)

 

//响应拦截器

_axios.incerceptors.response.use(

(response) => {

​          if(response.data.code && response.data.code === 401 && response.data.data) {

  // 需要重新登录

  localStorage.clear()

  sessionStorage.clear()

  window.location.href = ‘/’

} else {

  return response.data

}

},

(error) => {

  const { response } = error

  if(response) {

  // 请求已发出但是服务器状态码不是200

  errorHandle(response.status, response.data.message)

  return Promise.reject(response)

}

// 断网处理

// 断网是 app 显示提示

if(!window.navigator.online) {

  // store 设置一个值显示断网 的显示隐藏 并且重新加载请求

  console.log(‘断网’)

} else {

  // 其他错误超时等

  return Promise.reject(error)

}

}

)

 

export default _axios

 

**// 要用到 axios 的地方 引入 上边的 文件 （**每一个menu 都有多个请求**）**

import _axios from ‘xxxx’

const account = {

  getAccount(params) {

​    return _axios.post(..........) // get ...

}

}

export default account

 

**// 汇合多个 menu** 

import account from ‘xxx/account’

import account1 from ‘xxx/account1’

...

 

export default {

  account,

  account1,

  ...

}

 

// main.js

import Vue from ‘vue’

// 引入 所有 menu 中的接口

import api from ‘../xx/xx’

 

// 挂载 到 vue实例上

Vue.prototype.$basisApi = api

 

##### (7) **开发技术总结**

###### ①　**时间比较**

Let a = ‘2022-11-1’, b = ‘2022-11-4’

( new Date(a) ).getTime() 比较 ( new Date(b) ).getTime()

( new Date(a) ).getTime() --> 转为number类型，多少毫秒；进行比较

###### ②　**设置时间**

Let date = new Date();

date.setTime( data.getTime(date) + 3600 * 1000 * 24 )

###### ③　**加载动画**

\1. 全局：

axios 拦截器

\1. 单个：

Let loading = this.$loading({ }) 

当请求成功或失败时 loading.close() 动画关闭

axios.post().then().catch()

async await ---> let loading=this.$loading({}) 开启 ；await axios.post();  loading.close()

 

###### ④　**初始化data中的数据**

this.$options.data() 与 this.$nextTick(()=>{}) 结合；初始化data中的数据

###### ⑤　**重置表单**

This.$nextTick( () => { this.form = this.$options.data().form } )

###### ⑥　**引入多个文件**

**引用webpack中的require.context()**	

const requestComponent = require.context({ 

‘@/components/Transaction/TransactionForm’, 

false,

/\w+\.vue$/

 })

const componentsObj = {}

requestComponent .keys().forEach(fileName => {

  let name = fileName.split(‘/’).pop().replace(/\.\w+$/, ‘’)

  const componentConfig = requestComponent(name)

  componentsObj[name] = componentConfig .default || componentConfig 

})

 

export default {

  components: Object.assign(

  {},

  componentsObj,

  { ImportMessage: ImportMessage },

),

}

 

**vuex之多个module引入.js文件**

 

import Vue from 'vue';

import Vuex from 'vuex';

import getters from './getters';

 

Vue.use( Vuex );

 

// https://webpack.js.org/guides/dependency-management/#requirecontext

const modulesFiles = require.context( './modules', true, /\.js$/ );

 

// you do not need `import app from './modules/app'`

// it will auto require all vuex module from modules file

const modules = modulesFiles.keys().reduce( ( modules, modulePath ) => {

  // set './app.js' => 'app'

  const moduleName = modulePath.replace( /^\.\/(.*)\.\w+$/, '$1' );

  const value = modulesFiles( modulePath );

  modules[moduleName] = value.default;

  return modules;

}, {} );

 

const store = new Vuex.Store( {

  modules,

  getters

} );

 

export default store

 

**页面需要导入多个组件时** [**require.context()** ](https://www.jianshu.com/p/f918617cde4e)

 

import aaa from '@/components/login/aaa'

import bbb from '@/components/login/bbb'

import ccc from '@/components/login/ccc'

import ddd from '@/components/login/ddd'

components:{

​    aaa,

​    bbb,

​    ccc,

​    ddd,

}

\-------------------------------------

const path = require('path')

const files = require.context('@/components/login', false, /\.vue$/)

const modules = {}

files.keys().forEach(key => {

  const name = path.basename(key, '.vue') // 提取出用 '/' 隔开的path的最后一部分,path.basename(p, [ext])。 p要处理的path,ext要过滤的字符

  modules[name] = files(key).default || files(key)

})

components:modules

 

###### ⑦　**History模式的跳转**

window.open(`${window.location.origin}/ibor/dealquery/${DealObkectType[typeMapping[prodType]]}?params=${JSON.stringify(params)}`)

###### ⑧　**signalR 与 进度条 结合使用**

npm i @microsoft/signalr

 

import * as signalR from ‘@microsoft/signalr’

import URL form '....'

 

import { isThisTypeNode } from "typescript"

 

export default {

  SR: {},

  // 初始化连接

  initSR: function() {

​    // let that = this

​    if(!localStorage.LoginData) return

​    let userInfo = JSON.parse(localStorage.LoginData)

​    let url = URL

​    // 1. 初始化连接

​    this.SR = new signalR.HubConnectionBuilder()

​      .withUrl(`${url}`,{

​        accessTokenFactory: () => encodeURI(`${JSON.parse(localStorage.LoginData).accessToken}`)

​      })

​      .withAutomaticReconnect()

​      .configureLogging(signalR.LogLevel.Information)

​      .build()

​    // 2. 携带参数

​    this.SR.qs = { enc_auth_token: encodeURL(userInfo.accessToken) }

​    console.log('init', this.SR)

  },

  // 发送消息

  sendMsg: function({ mode, type, content}) {

​    let ID = JSON.parse(sesstionStorage.getItem('jurisdictionData')).ID

​    console.log('ID', ID)

​    let data = {

​      PageNo: ID,

​      FunctionSubModule: mode,

​      FunctionSubType: type,

​    }

​    if(!!content) {

​      data.DataContent = content

​    }

​    let that = this

​    try {

​      console.log('current state: ', that,SR.state)

​      if(that.SR.state === 'Connected') {

​        that.SR.invoke('RcvRequest', data)

​      } else {

​        that.SR.start().then(() => {

​          that.SR.invoke('RcvRequest', data)

​        })

​      }

​    } catch (error) {

​      console.log('send error: ', error)

​    }

  },

  // 接收信息

  receiveMsg: function(cb) {

​    let that = this

​    that.SR.on('PublishNotification', cb)

  },

  // 停止连接

  stopSR: function() {

​    let that = this

​    try {

​      that.SR.stop()

​      console.log('signalR退出成功')

​    } catch (error) {

​      console.log('stop', error)

​    }

  }

}

 

使用

<el-dialog>

  <el-progress :percentage=”parcent” :show-text=”true”></el-progress>

​      <span>{{ progressMsg }}</span>

​      <ImportMessage :data=”fileInfoData” :msgFlag=”msgFlag”></ImportMessage >

</el-dialog>

 

import ImportMessage from ‘xxxxx’ 

import signal from ‘@/utils/signalR’

 

//触发signal的事件

fn(){

  this.$confirm('Delete These Data?', 'Notification', {

​    cancelButtonText: 'Cancel',

​    cancelButtonClass: 'cancelBGC',

​    confirmButtonText: 'OK',

​    confirmButtonClass: 'confirmBGC',

​    type: 'warning',

  })

​    .then(()=>{

​      // ... 触发

​      this.jumpValue ='Delete'

​      this.msgFlag = false

​      let param = {

​        Deals: row.Deals,

​        ReceiptType: 'DealGroupManagement',

​      }

​      this.handleUpdate({

​        content: param,

​        mode: 'FileOpration',

​        type: 'Delete',

​      })

​    })

​    .catch(()=>{

​      console.log('取消')

​    })

},

async initSR(data) {

  //连接

  signal.initSR()

  this.sendMsg(data)

},

// 发送消息

sendMsg(data) {

  signal.sengMsg(data)

},

// 接收消息

receiveMsg(data) {

  signal.receiveMsg( res => {

​    if(res.errorCode === 20000 || res.errorCode === 20001) {

​      switch (data.type) {

​        case 'ContinueProcess':

​          if(res.errorCode === 20001) {

​            this.$notify.success({

​              title:'success',

​              dangeroudlyUseHTMLString: true,

​              message: res.dataObj && res.dataObj !== '' ? `<p>${res.dataObj}</p>` : `<p>${this.jumpValue} Success</p>`,

​              duration: 3000,

​              customClass: 'notify-success',

​            })

​            this.saveFlag = true // 表示数据提交成功，可以直接关闭页面

​            // ...其他操作

​          }

​          if(res.currentStatus === 'Error') {

​            // ...其他操作

​          }

​          break;

​        case 'ProcessFileContent':

​        case 'DealGroup':

​        case 'Jump':

​        case 'Delete':

​          this.percent = res.currentValue

​          this.progressMsg = res.message

​          this.progressData = res

​          if(res.dataObj) {

​            this.fileInfoData = JSON.parse(JSON.stringify(res.res.dataObj))

​          }

​          if( (res.errorCode === 20000 && res.currentStatus === 'Done')|| res.currentStatus === 'Error') {

​            // ...

​          }

​          break;      

​        default:

​          this.$notify({

​            type: res.currentStatus === 'Error' ? 'error' : 'Success',

​            message: res.message,

​            duration: res.currentStatus === 'Error' ? 0 : 3000,

​            onClose: function () {

​              // ...

​            }

​          })

​          break;

​      }

​    } else {

​      this.jumpValue = ''

​      this.$notify.error({

​        title: 'Error',

​        message: res.message,

​        duration: 0,

​        customClass: 'notify-error',

​        onClose: function () {

​          // ...

​        }

​      })

​    }

  })

},

// 停止连接

stopSignal() {

  signal.stopSR()

},

handleUpdate(data) {

  //...显示进度条弹窗

  this.initSR(data)

  this.receiveMsg(data)

},

// 弹窗关闭时

handleImportClose(){

  this.stopSignal()

}

###### ⑨　**将table数据导出 excel**

插件 xlsx xlsx-style

 

import xlsx from ‘xlsx

 

downXlsx(selectdata) {

  const loading = this.$loading({

​    lock: true,

​    text: 'Loading',

​    spinner: 'el-icon-loading',

​    background: 'rgba(0, 0, 0, 0.7)',

  })

 // 创建表格

  let sheet = xlsx.utils.json_to_sheet(selectdata),

   book = xlsx.utils.book_new()

  xlsx.utils.book_append_sheet(book, sheet, 'sheet1')

  xlsx.writeFile(book, `export${new Date().getTime()}.xlsx`)

  loading.close()

}

 

###### ⑩　**数据较多，使用自定义指令，上划加载**

<el-select

  v-if=”f.type==’select’&&f.label===’Inventory Date’”

  v-model=”dataForm[f.key]”

  v-elSelectLoadmore=”loadmore”

  :loading=”true”

  clearable

  filterable

\>

  <el-option

​    v-for=”(o,index) in f.option”

​    :key=”f.optionKey?o[f.optionKey]:0+index”

​    :label=”f.optionKey?o[f.optionKey]:o”

​    :value=”f.optionKey?o.[f.optionKey]:o”

   \></el-option>

</el-select>

 

data() {

  return {

​    pageIndex:1,//当前第几页

​    pageSize:20,//每次获取几条数据

​    TotalCount:0,//总数据数量

​    selectLoading:false,//加载

}

}

 

directives: {

  ‘elSelectLoadmore’: {

  bind(el, binding) {

  const SELECTWRAP_DOM = el.querySelector(“.el-select-dropdown .el-select-dropdown__wrap”)

  SELECTWRAP_DOM.addEventListener(“scroll”, function() {

  const condition = this.scrollHeight - this.scrollTop <= this.clientHeight

  if(condition) {

  console.log(‘=======触底加载=========’)

  binding.value()

}

}

}

}

}

 

mounted() {

  //初始化 inventoryDate

  this.getInventoryDate()

}

 

methods: {

  // 下拉 懒加载

  async loadmore() {

​    if(this.TotalCount>this.formInput.InventoryDate.option.length) {

  await this.getInventoryDate()

}

},

getInvemtoryDate() {

  this.selectLoading = true

  ...请求数据

}

}

}

###### ⑪　**自定义指令：弹窗宽度拖大拖小**

// v-dialogDragWidth 拖大拖小

Vue.directive('dialogDragWidth', {

  bind(el, binding, vnode, oldVnode) {

​    const dragDom = binding.value.$el.querySelector('.el-dialog')

​    el.onmousedown = e => {

​      // 鼠标按下，计算当前元素距离可视区的距离

​      const disX = e.clientX - el.offsetLeft

​      document.onmousemove = e => {

​        e.prevwntDefault() // 移动时禁止默认事件

 

​        // 通过事件委托，计算移动距离

​        const l = e.clientX - disX

​        dragDom.style.width = `${l}px`

​      }

​      document.onmouseup = e => {

​        document.onmousemove = null

​        document.onmouseup = null

​      }

​    }

  }

})

###### ⑫　**自定义指令：拖拽**

// v-dialogDrag 拖拽

Vue.directive('dialogDrag', {

  bind(el, binding, vnode, oldVnode) {

​    const dialogHeaderEl = el.querySelector('.el-dialog__header')

​    const dragDom = el.querySelector('.el-dialog')

​    dialogHeaderEl.style.cursor = 'move'

​    // 获取原有属性 ie dom元素.currentStyle 

​    // 火狐 谷歌 window.getComputedStyle(dom元素, null)

​    const sty = dragDom.currentStyle || window.getComputedStyle(dragDom, null)

​    dialogHeaderEl.onmousedown = e => {

​      const disX = e.clientX - dialogHeaderEl.offsetLeft

​      const disY = e.clientY - dialogHeaderEl.offsetTop

​      // 获取到的值 带 px 正则匹配替换

​      let styL, styT

​      // 注意在ie中 第一次获取到的值为组件自带 50% 移动之后赋值为px

​      if(sty.left.inCludes('%')) {

​        styL = +document.body.clientWidth * (+sty.left.replace(/\%/g, '') / 100)

​        styT = +document.body.clientHeight * (+sty.top.replace(/\%/g, '') / 100)

​      } else {

​        styL = +sty.left.replace(/\%/g, '')

​        styT = +sty.top.replace(/\%/g, '')

​      }

​      document.onmousemove = e => {

​        // 通过事件委托，计算移动的距离

​        const l = e.clientX - disX

​        const t = e.clientY - disY

​        // 移动当前元素

​        dragDom.style.left = `${l + styL}px`

​        dragDom.style.top = `${t + styT}px`

​        // 将此时的位置传出去

​        // binding.value({x:e.pageX,y:e.pageY})

​      }

​      document.onmouseup = e => {

​        document.onmousemove = null

​        document.onmouseup = null

​      }

​    }

  }

})

###### ⑬　**el-table 禁用某一项数据的勾选**

<el-table>

  <el-table-column

​    type="selection"

​    width="55"

​    align="center"

​    **:selectable="selectabled"**

  />

  <el-table-column

​    v-for="(item,index) in xx"

​    :prop="item.prop"

​    :label="item.label"

​    :key="index"

  \>

​    **<template slot-scope="scope">****{{ scope.row }}****</template>**

  </el-table-column>

</el-table>

 

**selectabled**(row, index) {

  if(row.attr===0) {

​    return false

  } else {

​    return true

  }

}

###### ⑭　**接口请求，避免从缓存中拿数据**

// 添加时间戳

‘url_t=`${new Date().getTime()}`’

###### ⑮　**获取元素的宽高**

Element.getBoundingClientRect()

**元素有误滚动条都无影响，获取的是元素占页面的空间**

Element.getBoundingClientRect().left		/*距视口左上角*/	左边框到视口

Element.getBoundingClientRect().right		/*距视口左上角*/	右边框到视口 left + width

Element.getBoundingClientRect().top		/*距视口左上角*/	上边框到视口

Element.getBoundingClientRect().bottom	/*距视口左上角*/	下边框到视口 top + height

Element.getBoundingClientRect().x		/*距视口左上角*/	左上角到视口左侧

Element.getBoundingClientRect().y		/*距视口左上角*/	左上角到视口顶部

Element.getBoundingClientRect().width	/*自身占据的空间*/

Element.getBoundingClientRect().height	/*自身占据的空间*/

##### (8) **Form表单常见问题**

###### ①　**表单清空（重置）**

this.$refs[refsName].resetFields()

###### ②　**select 选中但不显示**

// change 时 ，使用强制刷新

this.$forceUpdate()

###### ③　**select 要获取数据过多少时**

前边的 ‘数据较多，使用自定义指令，上划加载’ 

###### ④　**select 的label与 value**

<el-select v-model=”formdata.ex”>

   <el-option

​     v-for=”item in options”

​     :key=”item.value”

​     :label=”item.key”   ---------change时，我们看到的是label

​     :value=”item.value” ------------change时，值为item.value

  \></el-option>

</el-select>

 

options: [

  {

  key: ‘Y’,

  value: ‘Trading form’,

},

  {

  key: ‘N’,

  value: ‘N’,

}

]

###### ⑤　**当一个 form 元素中只有一个输入框时，在该输入框中按下回车应提交该表单**

如果希望阻止这一默认行为，可以在 <form> 标签上添加 @submit.native.prevent。

###### ⑥　**某些输入框（选择框）必填未填时,单独调用该框的校验规则**

if( !this.formData.tradeDate ) {

  // 调用该框的校验规则

  this.$refs.ruleForm.validateField(‘TradeDate’)

}

###### ⑦　**关闭input框的自动填充功能**

浏览器有历史记录功能，形成历史输入记录下拉框

// 获取input 元素

const input = document.querySelector(‘xxx’)

// 设置属性

// ie ：autocomplete = ‘off’

//FF ：disableautocomplete

Input.setAttribute(‘autocomplete’, ‘off’)

###### ⑧　**校验功能没有触发**

###### ⑨　**输入框无法输入**

###### ⑩　**select选中了值，但是提示不能为空**

###### ⑪　**选择框选不中选项**

**组件初始化时，该属性未在formData中定义属性**

 

###### ⑫　**调用表单的整体校验**

this.$refs[formName][0].ruleFormRefs()

 

// 需要在表单组件内定义该方法

ruleFormRefs() {

  let flag = null

  this.$refs[componentName].validate( valid => {

  if(valid) {

  flag = true

} else {

  flag = false

}

})

Return flag

}

#### 2. **开发难点**

##### (1) **需要项目打包时生成一个文件，然后引入**

"scripts": {

​    "commit": "czg",

​    "bootstrap": "pnpm install",

​    "serve": "cd ./node_modules/pdfmake && node build-vfs.js ../../src/assets/fonts && cd ../../ && npm run dev",

​    "dev": "vite",

​    "build": "cd ./node_modules/pdfmake && node build-vfs.js ../../src/assets/fonts && cd ../../ && cross-env NODE_ENV=production vite build && esno ./build/script/postBuild.ts",

}

##### (2) **vuex数据持久化**

###### ①　**方案****1****：本地存储 localStorage  session****S****torage**

**用了localStorage为什么还要用vuex**

在使用vuex的时候，vuex的数据不能持久化存储，将vuex的数据存到本地，

localStorage即可实现数据持久化存储的问题，

 

**你既然用来本地存储，为什么还用vuex，有什么区别吗**

1.vuex他的数据是响应式的，而本地存储的数据不是响应式的

eg:假如ab两个组件都在用本地存储，你改变了a组件里的数据，a页面数据虽然会同步到本地存储，

但是由于数据不是响应式的，所以b页面的数据不会变，

2.localstorage，sessionstorage只能保存string类型的值，还涉及到数据转化，这也是它的一个缺点

3.一个是数据容器，一个是数据管理

###### ②　**方案****2****：vuex-persistedstate**

yarn add vuex-persistedstate

// 或

npm install --save vuex-persistedstate

 

import Vuex from "vuex";

// 引入插件

import createPersistedState from "vuex-persistedstate";

 

Vue.use(Vuex);

 

const state = {};

const mutations = {};

const actions = {};

 

const store = new Vuex.Store({

​	state,

​	mutations,

​	actions,

  /* vuex数据持久化配置 */

​	plugins: [

​		createPersistedState({

​      // 存储方式：localStorage、sessionStorage、cookies

​			storage: window.sessionStorage,

​      // 存储的 key 的key值

​			key: "store",

​			render(state) {

​        // 要存储的数据：本项目采用es6扩展运算符的方式存储了state中所有的数据

​				return { ...state };

​			}

​		})

​	]

});

 

export default store;

 

vuex中module数据的持久化存储

 

/* module.js */

export const dataStore = {

  state: {

​    data: []

  }

}

 

/* store.js */

import { dataStore } from './module'

 

const dataState = createPersistedState({

  paths: ['data']

});

 

export new Vuex.Store({

  modules: {

​    dataStore

  },

  plugins: [dataState]

}); 

 

注意事项：

 

storage为存储方式，可选值为localStorage、sessionStorage和cookies；

localStorage和sessionStorage两种存储方式可以采用上述代码中的写法，若想采用cookies坐位数据存储方式，则需要另外一种写法；

render接收一个函数，返回值为一个对象；返回的对象中的键值对既是要持久化存储的数据；

若想持久化存储部分数据，请在return的对象中采用key：value键值对的方式进行数据存储，render函数中的参数

![1709177297149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709177297149.png)

###### ①　**方案****3****：vuex-persist**

yarn add vuex-persist

// 或

npm install --save vuex-persist

 

import Vuex from "vuex";

// 引入插件

import VuexPersistence from "vuex-persist";

 

Vue.use(Vuex);

//  初始化

const state = {

​	userName:'admin'

};

const mutations = {};

const actions = {};

// 创建实例

const vuexPersisted = new VuexPersistence({

​	storage: window.sessionStorage,

  render:state=>({

  	userName:state.userName

​    // 或

​    ...state

  })

});

 

const store = new Vuex.Store({

​	state,

  actions,

  mutations,

  // 数据持久化设置

  plugins:[vuexPersisted]

});

 

export default store;

### C（consumer）端项目

#### 1. **组件库**

##### (1) **Vant****-UI**

###### ①　**引入**

**1)** **全局引入**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps37.png)

**2)** **按需引入**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps38.png)

###### ②　**适配--rem...**

**1)** **Rem适配（在vant 文档中）**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps39.png)

![1709177343746](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709177343746.png)

#### 1. **开发难点**

##### (1) **项目中重定向报错的问题** 

在拼多多 项目中，在跳转到 聊天和个人中心页面时，需要判断 本地存储中是否有token ，有token就跳转到该页面，没有就跳转到login页面。需要在 路由守卫中判断 beforeEnter 

beforeEnter(to,from,next){ 

​      console.log(to,from); 

​      let token = localStorage.getItem('token') 

​      console.log(token); 

​      if(token){ 

​        next() 

​      }else{ 

​          next({path:'/login'}) 

​      } 

​    } 

**问题：没有token ，跳转 login时，会报错 重定向问题** 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps40.jpg) 

**解决办法1：** 

这个报错是是路由中点击路径重复 ，也有可能是安装的vue-router还是之前出错的那个版本，在项目目录下运行 npm i vue-router@3.0 -S 即可。 

 

**解决办法2：** 

 在 main.js里添加一段代码。  

import Router from 'vue-router' 

const routerPush = Router.prototype.push 

Router.prototype.push = function push(location) { 

  return routerPush.call(this, location).catch(error=> error) 

} 

##### (2) **解决出现滚动条的问题**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps41.png)

##### (3) **设置滚动条** 

overflow:scroll /* x y 方向都会*/ 

或者 

overflow-x:scroll /*只是x方向*/ 

或者 

overflow-y:scroll  /*只是y方向*/ 

使用scrollbar属性设置滚动条样式 

::-webkit-scrollbar 滚动条整体部分 

::-webkit-scrollbar-button 滚动条两端的按钮 

::-webkit-scrollbar-track 外层轨道 

::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去） 

::-webkit-scrollbar-thumb 滚动条里面可以拖动的那个 

::-webkit-scrollbar-corner 边角 

::-webkit-resizer 定义右下角拖动块的样式 

##### (4) **右侧下拉了一段距离，左侧点击，右侧要回到起始点**

var xx = document.getElementsByClassName("van-tree-select__content"); 

xx[0].scrollTop = 0; 

console.log(xx[0].scrollTop); 

##### (5) **vue单个路由如何获取 store 中的 state**

路由配置文件中引入一下 store 

然后访问store 就能访问到 state 

##### (6) **图片懒加载** 

1.vantUI  Lazyload 懒加载 

//main.js 

import { Lazyload } from 'vant'; 

Vue.use(Lazyload); 

// 注册时可以配置额外的选项 

Vue.use(Lazyload, { 

  lazyComponent: true, 

}); 

##### (7) **vue使用element-resize-detector监听元素宽度变化**

如图，当我们切换左侧菜单展示效果的时候，右侧内容会对应变宽，但此时的echarts并不能执行自适应效果，这是因为切换菜单展示效果并没有触发window.onresize，所以为解决类似此问题，我们可使用element-resize-detector

![1709177378744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709177378744.png)

1、引入element-resize-detector，npm install element-resize-detector --save

2、在对应位置上引入即可

 

let elementResizeDetectorMaker = require("element-resize-detector");

 

mounted(){

this.bar()

//监听元素变化

let erd = elementResizeDetectorMaker();

let that = this;

erd.listenTo(document.getElementById("bar"), function (element) {

​    that.$nextTick(function () {

​        //使echarts尺寸重置

​        that.myEcharts.resize();

​    })

})

}

methods(){

  bar(){

​    this.myEcharts = this.echarts.init(document.getElementById(“bar”),this.echartsTheme.theme)

}

}

//监听元素变化

PS：如果在改变宽度过程中存在动画效果，此时我们可以使用防抖，使在动画结束后再resize，这样做的好处是避免在动画过程中不断进行resize，造成界面卡顿，影响性能

节流与防抖代码见：https://blog.csdn.net/Ag_wenbi/article/details/106879625

<template>

    <div class="page">

        <div id="bar" class="echarts"></div>

​    </div>

</template>

 

<script>

​    let elementResizeDetectorMaker = require("element-resize-detector");

​    import {debounce} from 'utils.js';

​    export default {

​        name:'page',

​        mounted(){

​            let erd = elementResizeDetectorMaker();

​            let that = this;

​            erd.listenTo(document.getElementById("bar"), debounce(this.resizeFunc))

​        },

​        methods:{

​            resizeFunc(element){

​                console.log(element);//element元素信息

​                that.$nextTick(function () {

​                    //使echarts尺寸重置

​                    that.myEcharts.resize();

​                })

​            }

​        }

​    }

</script>

 

<style lang="scss" scoped>

.page{

​    width:100%;

​    height:100%;

​    .echarts{

​        width:100%;

​        height:100%;

​    }

}

</style>

 

##### (1) **滚动插件 better-scroll** 

**better-scroll**  

npm i better-scroll -S 

https://better-scroll.github.io/docs/zh-CN/ 

https://github.com/ustbhuangyi/better-scroll/blob/master/README_zh-CN.md 

//组件中 

import BScroll from 'better-scroll' 

 

mounted () {  this.$nextTick(() => { this.scroll = new BScroll(this.$refs.wrapper, {}) }) } 

// template 

<div  class="wrapper"
 style="height: 200px; border: 1px solid #f00;overflow:hidden"
 ref="wrapper"
 >

  <div class="content">

​    <ul> 

​      <li>1</li> 

​      ... 

​      <li>10</li> 

​    </ul> 

  </div> 

</div> 

###### ①　**解决vue v-for渲染的异步请求数据无法使用better-scroll插件滚动问题**

https://blog.csdn.net/cyj5201314/article/details/122821444  

我们可以用watch监听v-for渲染的列表数据，一旦该数据发生改变，说明异步请求更新了该数据，再使用this.$nextTick()函数执行回调，执行插件实例的refresh()方法即可  

watch: { 

​    cities () {  this.$nextTick(function () {  this.scroll.refresh()  })  } 

  } 

##### (2) **不能直接修改 通过props 传递过来的值**

不能直接修改 通过props 传递过来的值  会 报错 报错的 

![1709177404235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709177404235.png)

<v-tabber v-model=”activeName” @change=”onChange”></v-tabber>

new Vue({

props:[‘activename’],

data(){

return { activeName:this.activename }

}

})

##### (1) **重复key检测，可能造成更新错误**

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps42.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps43.jpg) 

### 实现效果

```js
1.Vuex带你手把手实现Vue后台管理中的角色鉴权
1.静态的
通俗点将就是前端将 所有的导航数据以及所有的路由定死在前端，每个导航和路由新增roles属性代表当前路由或当前导航可以访问的角色有哪些，登录时接口返回用户角色 role，进行判断和过滤，实现鉴权
2.动态的
前端定定义基础路由，用户的导航数据和 路由数据 存储在数据库中，用户登录时，请求接口获取当前用户可以访问的路由和菜单，前端拿到数据 动态渲染导航，通过 vue路由 addRoute方法动态添加到路由中
vuex中定义user模块，存储用户信息以及用户侧边导航数据
// 引入封装好的 登录的model 函数
import doLogin from '@/api'

export default {
    namespaced: true,
    state: {
         // 用户信息利用缓存备份防止刷新 取值时判断缓存获取
         // 用户的基础信息 如nickName昵称和avatar用户头像
        userInfo: localStorate.getItem('userInfo') ? JSON.parse(localStorate.getItem('userInfo')) : {} ,
        // 登录返回的token 秘钥
        token: localStorage.getItem('token') || '', 
        // 当前用户的角色
        role: localStorage.getItem('role') || '', 
        // 所有的导航 以下是示例
        menus: [
          // 每个导航 新增roles属性 代表可以访问当前用户的所有的角色
          {  label: '仪表盘', path: '/dashBoard', roles: ['admin', 'a', 'b', 'superAdmin'], icon: 'el-icon-s-data' },
          {  label: '商品管理', path: 'el-icon-s-goods',  icon: 'el-icon-s-data', roles: ['admin', 'a', 'b', 'superAdmin'] },
          {  label: '个人中心', path: '/user', roles: ['admin', 'a', 'b', 'superAdmin'], icon: 'el-icon-user-solid' },
          {  label: '设置', path: '/setting', roles: ['a', 'b', 'superAdmin'],  icon: 'el-icon-s-tools' }
        ]
},
  }
}
    getters: {
        authMenus (state) {
            // 定义getters 过滤当前用户的role不能访问的导航
            // 这就是当前用户role可以访问的导航
            return state.menus.filter(menu=> menu.roles.includes(state.role))
        }
    },
    mutations: {
        INIT_LOGIN (state, {userInfo, token, role}) {
           // 登录成功 存储 用户信息
            state.userInfo = userInfo
            state.token = token
            state.role = role
            // 缓存起来防止刷新 vuex状态丢失
            localStorage.setItem('userInfo', JSON.stringify(userInfo))
            localStorage.setItem('token', token)
            localStorage.setItem('role', role)
        }
    },
    actions: {
        DO_LOGIN ({commit}, params) {  // action中发送请求进行登录
            doLogin(params).then(res => {
                if(res.data.code === 200) {  // 请求成功触发mutation存储用户信息 包括role
                    commit('INIT_LOGIN', {
                        userInfo: res.data.data.userInfo,
                        token: res.data.data.token,
                        role: res.data.data.role
                    })
                }
            })
        }
router中路由meta中新增roles 定义当前路由可以访问的所有的角色
const routes = [
  { path: '/', component: Admin, meta: { roles: '*' // * 所有角色都可以访问 },
      children: [
        { path: '/', redirect: '/dashBoard', meta: { roles: ['admin', 'a', 'b', 'superAdmin'] } },
        { path: '/dashBoard', name: '仪表盘', component: () => import('_views/DashBoard'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/itemLists', name: '商品管理', component: () => import('_views/Items'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/itemAdd', name: '增加商品', component: () => import('_views/Items/components/ItemAdd'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/itemUpdate', name: '修改商品', component: () => import('_views/Items/components/ItemUpdate'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/cateLists', name: '分类管理', component: () => import('_views/Cate'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/user', name: '个人中心', component: () => import('_views/SetUser'), meta: { roles: ['admin', 'a', 'b', 'superAdmin'] }  },
        { path: '/setting', name: '设置', component: () => import('_views/Setting'), meta: { roles: ['a', 'b', 'superAdmin'] }  }
      ]
    },
    { path: '/login', component: () => import('_views/Login'),  meta: { roles: '*' } },
    { path: '*', component: () => import('_views/NotFound'), meta: { roles: '*' } },
    { path: '/noAuth', component: () => import('_views/Nopermission'), meta: { roles: '*' } }
]
router新增路由前置首位 做权限拦截
router.beforeEach((to, from, next) => {
   //登录鉴权 
  if (to.path !== '/login') {
    if (isLogin()) {
      /* 登录成功后，判断当前用户的角色 能否访问当前路由， 可以的话 放行；不能 到没有权限这个页面去*/
      if (to.meta.roles === '*') {  // 所有用户都可以访问 直接放行
        next()
      } else {  // 判断 roles中是否包含 用户的role
        const role = localStorage.getItem('role') || ''
        if (to.meta.roles.includes(role)) {
          next()
        } else {  // 去没有权限这个页面 这是没有权限路由需要自己创建一个
          next('/noAuth')
        }
      }
    } else {
      next('/login')
    }
  } else {
    next()
  }
}
侧边导航页面 使用 getters中的 authMenus 循环侧边导航
    <el-menu  @select="choseMenu">
          <div v-for="nav in $store.getters['user/authMenus ']" :key="nav.label">
            <el-menu-item :index="nav.label" @click="switchNav(nav.path, nav.label)" v-if="!nav.children">
              <i :class="nav.icon"></i>
              <span slot="title">{{nav.label}}</span>
            </el-menu-item>
            <el-submenu :index="nav.label" v-if="nav.children">
              <template slot="title">
                <i :class="nav.icon"></i>
                <span>{{nav.label}}</span>
              </template>
                <el-menu-item v-for="subNav in nav.children" :key="subNav.path" :index="subNav.label"
                  @click="switchNav(subNav.path, subNav.label)">{{subNav.label}}</el-menu-item>
            </el-submenu>
          </div>
    </el-menu>
最后一步 登录页登录时调用 请求登录的action即可大功告成
this.$store.dispatch('user/DO_LOGIN',{
    userName: 'xxx',
    pwd: 'xxxx'
})
2.使用vditor做的markdown编辑器
(1)使用vditor做的markdown编辑器
// 下载vditor 插件
npm i vditor --save

//markdown.vue
<template>
  <div ref="wrapRef"></div>
</template>
<script lang="ts">
  import type { Ref } from 'vue';
  import {
    defineComponent,
    ref,
    unref,
    nextTick,
    computed,
    watch,
    onBeforeUnmount,
    onDeactivated,
    // watchEffect,
  } from 'vue';
  import {
    postInfoList,
  } from '/@/api/performance/personal'
  import Vditor from 'vditor';
  import 'vditor/dist/index.css';
  import { useLocale } from '/@/locales/useLocale';
  import { useModalContext } from '../../Modal';
  import { useRootSetting } from '/@/hooks/setting/useRootSetting';
  import { onMountedOrActivated } from '/@/hooks/core/onMountedOrActivated';
  import { getTheme } from './getTheme';
  // import { $HTTP } from '/@/api/$http';
  type Lang = 'zh_CN' | 'en_US' | 'ja_JP' | 'ko_KR' | undefined;

  export default defineComponent({
    inheritAttrs: false,
    props: {
      height: { type: Number, default: 'auto' },
      value: { type: String, default: '' },
    },
    emits: ['change', 'get', 'update:value', 'closeLoading'],
    setup(props, { attrs, emit }) {
      const wrapRef = ref<ElRef>(null);
      const vditorRef = ref(null) as Ref<Nullable<Vditor>>;
      const initedRef = ref(false);
      const modalFn = useModalContext();

      const { getLocale } = useLocale();
      const { getDarkMode } = useRootSetting();
      const valueRef = ref(props.value || '');

      watch(
        [() => getDarkMode.value, () => initedRef.value],
        ([val, inited]) => {
          if (!inited) {
            return;
          }
          instance
            .getVditor()
            ?.setTheme(getTheme(val) as any, getTheme(val, 'content'), getTheme(val, 'code'));
        },
        {
          immediate: true,
          flush: 'post',
        },
      );

      watch(
        () => props.value,
        (v) => {
          if (v !== valueRef.value) {
            instance.getVditor()?.setValue(v);
          }
          valueRef.value = v;
        },
      );

      const getCurrentLang = computed((): 'zh_CN' | 'en_US' | 'ja_JP' | 'ko_KR' => {
        let lang: Lang;
        switch (unref(getLocale)) {
          case 'en':
            lang = 'en_US';
            break;
          case 'ja':
            lang = 'ja_JP';
            break;
          case 'ko':
            lang = 'ko_KR';
            break;
          default:
            lang = 'zh_CN';
        }
        return lang;
      });
      function init() {
        const wrapEl = unref(wrapRef) as HTMLElement;
        if (!wrapEl) return;
        const bindValue = { ...attrs, ...props };
        const insEditor = new Vditor(wrapEl, {
          // 设置外观主题
          theme: getTheme(getDarkMode.value) as any,
          lang: unref(getCurrentLang),
          mode: 'sv',
          minHeight:200,
          fullscreen: {
            index: 520,
          },
          preview: {
            theme: {
              // 设置内容主题
              current: getTheme(getDarkMode.value, 'content'),
            },
            hljs: {
              // 设置代码块主题
              style: getTheme(getDarkMode.value, 'code'),
            },
            actions: [],
          },
          hint: {
            parse: true,
          },
          toolbar: [//工具栏
            "emoji",
            "headings",
            "bold",
            "italic",
            "strike",
            "link",
            "|",
            "list",
            "ordered-list",
            "check",
            "outdent",
            "indent",
            "|",
            "quote",
            "line",
            "code",
            "inline-code",
            "insert-before",
            "insert-after",
            "|",
            "upload",
            // "record",//录音
            "table",
            "|",
            "undo",
            "redo",
            "|",
            "fullscreen",
            "edit-mode",
            {
              name: "more",
              toolbar: [
                  "both",
                  "code-theme",
                  "content-theme",
                  "export",
                  "outline",
                  "preview",
                  "devtools",
                  "info",
                  "help",
              ],
            },
          ],
          upload: {
            // url: 'http://localhost:3000/postimg',
            multiple: true,//上传文件是否为多个
            accept: 'image/*',//文件上传类型，同 input accept
            fieldName: 'formFiles',
            error(msg: string){
              console.log(msg)
            },
            // success( editor: HTMLPreElement, msg: any ){//上传成功回调
              // console.log(editor,msg)
    //           // valueRef.value = JSON.parse(msg).data.url
    //           let succFileText=''
    //           const res = JSON.parse(msg)
    //           res.data.forEach(item => {
    //             const path = item.url;
    //             const name = item.name;
    //             const lastIndex = name.lastIndexOf(".");
    //             let type = name.substr(lastIndex);
    //             type = type.toLowerCase();
    //             if (type.indexOf(".wav") === 0 || type.indexOf(".mp3") === 0 || type.indexOf(".ogg") === 0) {
    //               if (instance.getVditor()?.currentMode === "wysiwyg") {
    //                 succFileText += `<div class="vditor-wysiwyg__block" data-type="html-block"
    // data-block="0"><pre><code><audio controls="controls" src="${path}"></audio></code></pre>`;
    //               } else if (instance.getVditor()?.currentMode === "ir") {
    //                 succFileText += `<audio controls="controls" src="${path}"></audio>\n`;
    //               } else {
    //                 succFileText += `[${name}](${path})\n`;
    //               }
    //             } else if (type.indexOf(".apng") === 0
    //               || type.indexOf(".bmp") === 0
    //               || type.indexOf(".gif") === 0
    //               || type.indexOf(".ico") === 0 || type.indexOf(".cur") === 0
    //               || type.indexOf(".jpg") === 0 || type.indexOf(".jpeg") === 0 || type.indexOf(".jfif") === 0 || type.indexOf(".pjp") === 0 || type.indexOf(".pjpeg") === 0
    //               || type.indexOf(".png") === 0
    //               || type.indexOf(".svg") === 0
    //               || type.indexOf(".webp") === 0) {
    //               if (instance.getVditor()?.currentMode === "wysiwyg") {
    //                 succFileText += `<img alt="${name}" src="${path}">`;
    //               } else {
    //                 succFileText += `![${name}](${path})\n`;
    //               }
    //             } else {
    //               if (instance.getVditor()?.currentMode === "wysiwyg") {
    //                 succFileText += `<a href="${path}">${name}</a>`;
    //               } else {
    //                 succFileText += `[${name}](${path})\n`;
    //               }
    //             }
    //           });
    //           console.log("succFileText",succFileText)
    //           instance.getVditor()?.insertValue(`${succFileText}`)
            // },
            // format(fileLists: File[], responseText: string): string  {
            //   //对服务端返回的数据进行转换，以满足内置的数据结构
            //   // 上传图片显示文件地址的原因是succMap 的 key 要用文件名
            //   console.log(fileLists,responseText)
            //   const { files } = JSON.parse(responseText)
            //   let obj = {}
            //   console.log(fileLists,files)
            //   files.forEach((item)=>{
            //     // console.log(item.split('/')[item.split('/').length-1])
            //     // console.log(`${item.split('/')[item.split('/').length-1]}`)
            //     obj[`${item.split('/')[item.split('/').length-1]}`] = $HTTP.ADDRESS + '/' + item.split('/')[item.split('/').length-2] + '/' + item.split('/')[item.split('/').length-1];
            //   })
            //   console.log(obj)
            //   return JSON.stringify({
            //     "msg": "",
            //     "code": 0,
            //     "data": {
            //       "errFiles": [],
            //       "succMap": obj,
            //     }
            //   })
            // },
            handler(files: File[]):any {
              // console.log(files)
              // console.log(fileList._rawValue[0].originFileObj)
              files.forEach(item=>{
                // console.log(item)
                const formData = new FormData()
                formData.append('formFiles', item)
                postInfoList(formData)
                  .then(res=>{
                    // console.log(res)
                    // {files: ['Summaries/微信图片_20210716093831.jpg']}
                    // const path = $HTTP.ADDRESS + '/' + res.files[0];
                    const name = res.files[0].split('/')[res.files[0].split('/').length-1];
                    // console.log(path,name)
                    /*
                    * 第一种 FileReader 生成 base64  
                    * 缺点：生成的pdf 图片大小不合适
                    */
                    var reader=new FileReader();
                    reader.readAsDataURL(item);
                    reader.onload=function(){
                      // console.log('图片',reader.result)
                    let path = reader.result

                    let succFileText='';
                    // return JSON.stringify(res)
                    if (instance.getVditor()?.currentMode === "wysiwyg") {
                      succFileText += `<img alt="${name}" src="${path}">`;
                    } else {
                      succFileText += `![${name}](${path})\n`;
                    }
                    // console.log("succFileText",succFileText)
                    instance.getVditor()?.insertValue(`${succFileText}`)
                    };
                  })
                  .catch(err=>{
                    console.log(err)
                  })
              })
              
            },
          },
          input: (v) => {
            valueRef.value = v;
            // console.log('context',v)
            emit('update:value', v);
            emit('change', v);
          },
          after: () => {
            nextTick(() => {
              modalFn?.redoModalHeight?.();
              insEditor.setValue(valueRef.value);
              vditorRef.value = insEditor;
              initedRef.value = true;
              // 初始状态，不让markdown自动获取焦点,选中后在获取焦点
              vditorRef.value.blur()
              emit('get', instance);
            });
          },
          blur: () => {
            // 这个是控制是否有鼠标可以点击工具栏
            // unref(vditorRef)?.setValue(props.value);
          },
          ...bindValue,
          cache: {
            enable: false,
          },
        });
      }

      const instance = {
        getVditor: (): Vditor => vditorRef.value!,
        ondisabled: () => {
          let setinterval = setInterval(()=>{
            // console.log(vditorRef.value)
            if(vditorRef.value) {
              vditorRef.value.disabled()
              clearInterval(setinterval)
              emit('closeLoading')
            }
          },1000)
        },
        undisabled: () => {
          let setinterval = setInterval(()=>{
            // console.log(vditorRef.value)
            if(vditorRef.value) {
              vditorRef.value.enable()
              clearInterval(setinterval)
              emit('closeLoading')
            }
          },1000)
        },
      };

      function destroy() {
        const vditorInstance = unref(vditorRef);
        if (!vditorInstance) return;
        try {
          vditorInstance?.destroy?.();
        } catch (error) {}
        vditorRef.value = null;
        initedRef.value = false;
      }

      onMountedOrActivated(init);

      onBeforeUnmount(destroy);
      onDeactivated(destroy);
      return {
        wrapRef,
        ...instance,
      };
    },
  });
</script>
(2)前端markdown数据 →导出pdf文件
1.下载模块
"pdfmake": "^0.2.7",
   	"html-to-pdfmake": "^2.4.11",
   	"marked": "^4.2.5",
2.引入模块
import {marked} from 'marked';
import pdfMake from "pdfmake/build/pdfmake";
import htmlToPdfmake from "html-to-pdfmake"
3.导出
async function convertMarkdownToPdf(){
        loading.value = true
// markdown的内容
        const markdown = valueRef.value
        const html = marked.parse(markdown); //转成html串
        // 异步加载 字体js文件
        // console.log(html)
        const pdfFonts = await import ("pdfmake/build/vfs_fonts")
        // 本地获取字体 pdfFonts.pdfMake?.vfs 线上获取字体 pdfMake.vfs
        pdfMake.vfs = pdfFonts.pdfMake ? pdfFonts.pdfMake.vfs : pdfMake.vfs;
        pdfMake.fonts = {
          NotoSerifSC: {
            normal: 'NotoSerifSC-Regular.otf',
            bold: 'NotoSerifSC-Regular.otf',
            italics: 'NotoSerifSC-Regular.otf',
            bolditalics: 'NotoSerifSC-Regular.otf'
          }
        };
        const val = htmlToPdfmake(html);
        // console.log(val)
        val.forEach(el => {
          // console.log(el?.stack[0].positions)
          // 图片
          if(el.stack){
            el.stack.forEach(item => {
              if(item.nodeName==='IMG') {
                item.width = 100
                // 只能是数字，不让会报错
                // item.width = 'auto'
              }
            });
          }
          // table 文字太长，或 图片太长 就会超出 a4 宽度，解决不了
          if(el.nodeName==='TABLE') {
            // el.width = 640;
            // console.log(el.table)
            // console.log(el.table?.widths)
            // console.log(el.table?.body)
            // 取不到 widths 最好不要在table中放图片
            // el.table?.body?.forEach(item => {
              // console.log(item.length)
              // item.forEach( i => {
                // i.width = 600/item.length + 'px'
                // i['word-break'] = 'break-all'
                // i['white-space'] = 'break-spaces'
                // console.log(i)
              // });
            // });
          }
        });
        // console.log(val)
        const dd = {
          content: val, 
          defaultStyle: {
            font: 'NotoSerifSC'
          },
          pageSize: "A4",
          pageOrientation: "portrait",
          // footer: footer,
          // header: header,
          styles: {},
        };
        pdfMake.createPdf(dd).download(`${props.value.userName}_${props.value.periodName}_个人总结.pdf`);
        loading.value = false;
      }
(3)难点：打包时生成文件
想要使用中文，就需要把字体转成 vfs_fonts.js 在引入
但是，上传到git上的项目，运行没有我们本地的vfs_fonts.js文件，所以需要我们这样：
在package.json中配置：（因为生成vfs_fonts.js必须在node_modules/pdfmake中成功，字体在../../src/assets/fonts静态文件中）
 	"serve": "cd ./node_modules/pdfmake && node build-vfs.js ../../src/assets/fonts && cd ../../ && npm run dev",
     "dev": "vite",
"build": "cd ./node_modules/pdfmake && node build-vfs.js ../../src/assets/fonts && cd ../../ && cross-env NODE_ENV=production vite build && esno ./build/script/postBuild.ts",
导出的字体：粗体 斜体 没效果是因为没有配置相对应的中文字体
pdfMake.fonts = {
          NotoSerifSC: {
            normal: 'NotoSerifSC-Regular.otf',
            bold: 'NotoSerifSC-Regular.otf',
            italics: 'NotoSerifSC-Regular.otf',
            bolditalics: 'NotoSerifSC-Regular.otf'
          }
        };

3.tab切换+内容滚动
实现功能：所有内容都在同一个页面，点击tab，内容滚动到相应页面；滚动内容，tab切换到相应tab项上
(1)c端：可以使用 vant-ui中的 Tab标签页中 滚动导航
https://vant-contrib.gitee.io/vant/#/zh-CN/tab

(2)b端：element-ui中是没有这个功能的，只有切换功能；只能手动
1.未验证是否所有浏览器都兼容
主要是这两个api的配合使用.在tab切换时,用scrollIntoView触发了scroll事件,在scroll事件中监听getBoundingClientRect().top,发现值小于或等于tab的高度了,说明已经进入到此区域了,修改active的值,tab的样式就改变了.反过来,如果手动滚动页面时,会直接触发scroll事件,从而也使改变active的值,控制tab样式的改变.
<template>
  <div class="box">
    <div class="tab" ref="tab">
      <div v-for="(item, index) in tabs" :key="index">
        <div :class="{ active: active === index }" @click="switchTab(index)">
          {{ item }}
        </div>
      </div>
    </div>
    <div class="cont" ref="cont">
      <div class="cont_1" ref="cont_1">内容一</div>
      <div class="cont_2" ref="cont_2">内容二</div>
      <div class="cont_3" ref="cont_3">内容三</div>
    </div>
    <div class="back-top" @click="backTop"></div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      tabs: ["tab1", "tab2", "tab3"],
      active: 0,
      cont1: null,
      cont2: null,
      cont3: null,
      isClickTab: false
    };
  },
  methods: {
    backTop() {
      this.cont1.scrollIntoView({
        block: "start",
        behavior: "smooth"
      });
    },
    switchTab(index) {
//scrollIntoView滚动到指定区域,并且可以设置动画效果,大多数浏览器都支持,在移动端更应该没问题了.
      if (index === 0) {
        this.cont1.scrollIntoView({
          block: "start",
          behavior: "smooth"
        });
      } else if (index === 1) {
        this.cont2.scrollIntoView({
          block: "start",
          behavior: "smooth"
        });
      } else {
        this.cont3.scrollIntoView({
          block: "start",
          behavior: "smooth"
        });
      }
    }
  },
  mounted() {
    this.cont1 = this.$refs["cont_1"];
    this.cont2 = this.$refs["cont_2"];
    this.cont3 = this.$refs["cont_3"];
    const tabH = this.$refs["tab"].offsetHeight;
    this.$refs["cont"].addEventListener("scroll", () => {
 // getBoundingClientRect 这个方法可以获取到当前元素距离窗口的上下左右的距离
      if (this.cont3.getBoundingClientRect().top <= tabH) {
        this.active = 2;
        return false;
      }
      if (this.cont2.getBoundingClientRect().top <= tabH) {
        this.active = 1;
        return false;
      }
      if (this.cont1.getBoundingClientRect().top <= tabH) {
        this.active = 0;
      }
    });
  }
};
</script>
<style lang="scss" scoped>
.box {
  font-size: 28px;
  overflow-x: auto;
  height: 100vh;
  display: -webkit-flex;
  display: flex;
  flex-direction: column;
  overflow-y: hidden;
  .tab {
    height: 88px;
    background: #fff;
    line-height: 88px;
    color: #666;
    display: -webkit-flex;
    display: flex;
    justify-content: space-around;
    .active {
      font-size: 32px;
      color: #333;
      &::after {
        display: block;
        content: "";
        width: 36px;
        height: 6px;
        margin: auto;
        margin-top: -10px;
        background: rgba(255, 51, 0, 1);
        border-radius: 3px;
      }
    }
  }
  .cont {
    height: 300px;
    flex-grow: 1;
    overflow: auto;
    .cont_1 {
      height: 400px;
      background: pink;
    }
    .cont_2 {
      height: 800px;
      background: yellow;
    }
    .cont_3 {
      height: 100%;
      background: lightgreen;
    }
  }
  .back-top {
    width: 80px;
    height: 80px;
    // background: url(../../assets/back-top.png) center / 100% 100% no-repeat;
    border-radius: 50%;
    position: fixed;
    bottom: 120px;
    right: 32px;
  }
}
</style>

4.业务需要在前端进行数据的缓存，到期就删除再进行获取新数据

前端设置数据定时失效的可以有下面2种方法：
1、当数据较大时，可以利用localstorage，存数据时候写入一个时间，获取的时候再与当前时间进行比较
2、如果数据不超过cookie的限制大小，可以利用cookie比较方便，直接设置有效期即可。
(1)利用localstorage实现
1.存储数据时加上时间戳
在项目开发中，我们可以写一个公用的方法来进行存储的时候加上时间戳

//export抛出
export function setLocalStorageAndTime (key, value) {
 window.localStorage.setItem(key, JSON.stringify({ data: value, time: new Date().getTime() }))
}
项目中：
存储、

// 有数据再进行存储
  setLocalStorageAndTime('XXX', {name: 'XXX'})
读取、

// 判断是否返回为null或者失效
getLocalStorageAndTime('XXX', 86400000)
获取数据时与当前时间进行比较、

export function getLocalStorageAndTime (key, exp = 86400000) {
 // 获取数据
 let data = window.localStorage.getItem(key)
 if (!data) return null
 let dataObj = JSON.parse(data)
 // 与过期时间比较
 if (new Date().getTime() - dataObj.time > exp) {
  // 过期删除返回null
  removeLocalStorage(key)
  console.log('信息已过期')
  return null
 } else {
  return dataObj.data
 }
}
(2)利用js-cookie实现
js-cookie
https://www.cnblogs.com/NanKe-Studying/p/13952558.html
安装：

//以下几种都可以用：
1、引入js-cookie.js

1.直接饮用cdn：<script src="https://cdn.jsdelivr.net/npm/js-cookie@2/src/js.cookie.min.js"></script>

2.本地下载下来后：<script src="/path/to/js.cookie.js"></script>

3.模块化开发时: import Cookies from 'js-cookie'

一、下载cookie
npm install js-cookie

二、当前页面引用cookie
import Cookies from "js-cookie";

三、cookie在当前的使用(方法一)
// 组件中使用
// 写入cookie
Cookies.set('name', 'value')
// 读取
Cookies.get('name') // => 'value'
Cookies.get('nothing') // => undefined
// 读取所有可见的cookie
Cookies.get()
// 删除某项cookie值
Cookies.remove('name')

四、cookie在全局使用（方法二）
main.js
import Cookies from 'js-cookie'

五、cookie设置过期时间
//1、存cookie  set方法支持的属性有 ：  expires->过期时间    path->设置为指定页面创建cookie   domain-》设置对指定域名及指定域名的子域名可见  secure->值有false和true ,表示设置是否只支持https,默认是false
Cookies.set('key', 'value');  
//创建简单的cookie
Cookies.set('key', 'value', { expires: 27 });
//创建有效期为27天的cookie
Cookies.set('key', 'value', { expires: 17, path: ''  }); 
//可以通过配置path,为当前页创建有效期7天的cookie

//2、取cookie
Cookies.get('key'); // 获取指定key 对应的value
Cookies.get(); //获取所有value

//3、删除cookie
Cookies.remove('key');//删除普通的cookie
Cookies.remove('name', { path: '' }); // 删除存了指定页面path的cookie
注意：如果存的是对象，如： userInfo = {age:111,score:90};  Cookie.set('userInfo',userInfo)

取出来的userInfo需要进行JSON的解析,解析为对象：let res = JSON.parse( Cookie.get('userInfo') );

当然你也可以使用：Cookie.getJSON('userInfo');
Cookies.get('name'); // => '{"foo":"bar"}'
Cookies.get(); // => { name: '{"foo":"bar"}' }
//-------------------------------------------------------//
Cookies.getJSON('name'); // => { foo: 'bar' }
Cookies.getJSON(); // => { name: { foo: 'bar' } }

js-cookie 的示例中只有以天为单位的有效期：

Cookies.set('name', 'value', { expires: 7 }); // 7 天后失效
官方文档只要设置天数，没有时分秒，这样我们想设置更小单位的时候无法下手，其实也可以设置时间戳来处理时间的，下面这种方式可以设置任意单位的有效期：

let seconds = 10;
let expires = new Date(new Date() * 1 + seconds * 1000);
Cookies.set('username', 'tanggaowei', { expires: expires }); // 10 秒后失效
贴上利用js-cookie的封装, 记得 npm i js-cookie

import Cookies from 'js-cookie'
 
/*
* 设置cookies
* */
export function getCookies (key) {
 return Cookies.get(key)
}
/*
* 设置Cookies
* */
export function setCookies (key, value, expiresTime) {
 let seconds = expiresTime
 let expires = new Date(new Date() * 1 + seconds * 1000)
 return Cookies.set(key, value, { expires: expires })
}
/*
* 移除Cookies
* */
export function removeCookies (key) {
 return Cookies.remove(key)
}
域domain与路径path
默认值：
path: ‘/’
前言：

domain表示的是cookie所在的域，默认为请求的地址，如网址为www.jb51.net/test/test.aspx，那么domain默认为www.jb51.net。而跨域访问，如域A为t1.test.com，域B为t2.test.com，那么在域A生产一个令域A和域B都能访问的cookie就要将该cookie的domain设置为.test.com；如果要在域A生产一个令域A不能访问而域B能访问的cookie就要将该cookie的domain设置为t2.test.com。

path表示cookie所在的目录，asp.net默认为/，就是根目录。在同一个服务器上有目录如下：/test/,/test/cd/,/test/dd/，现设一个cookie1的path为/test/，cookie2的path为/test/cd/，那么test下的所有页面都可以访问到cookie1，而/test/和/test/dd/的子页面不能访问cookie2。这是因为cookie能让其path路径下的页面访问。
cookie.set()更多参数
语法：
cookies.set（名称，[值]，[options]）
更多options的参数配置：

maxAge：一个数字，表示自Date.now()到期起的毫秒数

expires：一个Date对象，指示cookie的过期日期（默认在会话结束时过期）。默认：天

path：一个字符串，指示cookie的路径（/默认情况下）。

domain：一个字符串，指示cookie的域（无默认值）。

secure：一个布尔值，指示cookie是否仅通过HTTPS发送（false默认情况下，对于HTTP，true默认情况下，对于HTTPS）。在下面阅读有关此选项的更多信息。
httpOnly：一个布尔值，指示cookie是否仅通过HTTP（S）发送，并且不提供给客户端JavaScript（true默认情况下）。

sameSite：布尔值或字符串，指示cookie是“相同站点” cookie（false默认情况下）。可以将其设置为’strict’，‘lax’或true（映射到’strict’）。

signed：一个布尔值，指示是否要对cookie进行签名（false默认情况下）。如果为真，.sig则还将发送另一个具有后缀的同名Cookie，其27字节的url安全base64 SHA1值表示针对第一个Keygrip密钥的cookie-name = cookie-value的哈希值。此签名密钥用于检测下次接收cookie时的篡改。

overwrite：一个布尔值，指示是否覆盖以前设置的同名Cookie（false默认情况下）。如果是这样，则在设置此Cookie时，将从相同名称的同一个请求中设置的所

5.实现下拉刷新、上拉加载
(1)vue使用better-scroll实现下拉刷新、上拉加载
用好这个库，需要理解下面说明
必须包含两个大的div，外层和内层div
外层div设置可视的大小(宽或者高)-有限制宽或高
内层div，包裹整个可以滚动的部分
内层div高度一定大于外层div的宽或高，才能滚动
1、先开始写一个简单demo，最基本的代码架构（template）
<div ref="wrapper" class="wrapper"> 
<ul class="content"> 
<li>...</li> <li>...</li> 
</ul>
</div>

css

import BScroll from 'better-scroll'
this.scroll = new BScroll(new BScroll(this.$refs.wrapper))

 mounted () {
     this.scroll = new BScroll(this.$refs.wrapper, { 
// 上拉加载
         pullUpLoad: {  threshold: -30 // 当上拉距离超过30px时触发 pullingUp 事件 },
         pullDownRefresh: {   // 下拉刷新          
             threshold: 30,// 下拉距离超过30px触发pullingDown事件
             stop: 20 // 回弹停留在距离顶部20px的位置
         }
     })
     this.scroll.on('pullingUp', () => {
         console.log('处理上拉加载操作')
         setTimeout(() => {// 事情做完，需要调用此方法告诉 better-scroll 数据已加载，否则上拉事件只会执行一次
             this.scroll.finishPullUp()
         }, 2000)
     })
     this.scroll.on('pullingDown', () => {
         console.log('处理下拉刷新操作')
         setTimeout(() => {// 事情做完，需要调用此方法告诉 better-scroll 数据已加载，否则下拉事件只会执行一次
         		console.log('asfsaf')            
this.scroll.finishPullDown()
         }, 2000)
     })
 }
mounted(){
        // 获取 ref 为wrapper的dom
        let wrapper = this.$refs.wrapper
        let scroll = new BScroll(wrapper,{
                //配置项
                //0:默认值，不触发事件
                //1:屏幕滑动超过一定时间后触发scroll事件
                //2:屏幕在滑动过程中触发scroll事件
                //3:屏幕滑动过程中和momentum滚动动画都会触发scroll事件
                //momentum滚动动画：我们用力滑动时，手放开后屏幕还在滑动
                probeType:1,
                pullUpLoad:true,  //上拉页面，请求更多数据时，配置
click:true  //解决 每项的点击事件不起作用的问题
        })
        scroll.on('scroll',eve=>{})
        scroll.on('pullingUp',()=>{  //'上拉加载' ,但只能执行一次 //想要可以多次上拉加载
                setTimeout(()=>{ scroll.finisPullUp() },2000)
         })
}
```

### 开启服务器

#### 1. **本地服务器--json-server**

https://www.npmjs.com/package/json-server  

npm i json-server -g 

json-server -v 

 

创建 data文件夹 -> 建 文件 db.json  

{ "user": [ 

​    {  "id": 0, "name": "张三" }, 

​    { "id": 1,  "name": "李四" } } 

命令：  json-server db.json 监听数据 json-server --watch db.json 

#### 2. **开启一个后台服务器（接口）**

const http = require('http'); 

const hostname = '127.0.0.1'; 

const port = 5000; 

 

const server = http.createServer((req, res) => { 

  res.statusCode = 200; 

  res.setHeader('Content-Type', 'text/plain'); 

  res.end('Hello World asd'); 

}); 

 

server.listen(port, hostname, () => { 

  console.log(`Server running at http://${hostname}:${port}/`); 

});

express

npm init

npm i express

// server.js

const express = require(‘express’)

const app =express()

app.use((request,response,next)=>{

  console.log(‘有人请求服务器’)

  console.log(‘请求来自于’,request.get(‘Host’))

  console.log(‘请求地址’,request.url)

  next()

})

app.get(‘/students’,(request,response)=>{

  const students = [

  {id: ‘001’, name: ‘tom’, age: 18},

]

Response.send(students)

})

app.listen(5000,err=>{

  if(!err) console.log(‘服务器启动成功了，请求地址为http://localhost:5000/students’)

})

### 协同开发

#### 1. **协同开发**

使用工具：SourceTree   Git 

1.去远程仓库创建仓库 （GitHub gitee gitlab） gitee  

注册 

新建 仓库 （名称、个人创建 私有 （管理中可以改成 开源）、 ） 

管理  仓库管理者  邀请 开发者 一起开发 

 

创建一个项目 

git bash here  

git init 初始化一个本地项目  出现一个 .git 文件夹 

git add . 把所有的文件都添加到本地仓库暂存区  

git commit -m "first commit"  把暂存区的提交到 历史记录区 -m 参数 

 

（历史记录区 和 远程仓库 先链接 在） 

git remote add origin git@gitee.com:yongshengss/douyin.git 

建立本地仓库 和 远程仓库的链接，同时给这个远程仓库起了个别名 origin 

origin 可以修改 

 

git push -u origin master 

git push -u origin master --force 强制推送 

master 主分支，开发者不能在主分支开发，创建自己的分支 

 

三个区：工作区 暂存区 历史记录区 

 

创建 新分支 点新分支 克隆地址 在sourcetree中clone 一动操作  提  拉 推 点 master    拉   右键 合并 

 

#### 2. **流程**

创建 自己的分支 切换到自己的分支 

克隆项目 

切换到自己的分支上开发 

提交项目 到自己的分支 

切换到master 把自己的分支合并到master上 

### 联调

```
postman（工具） 测试接口：
https://gitee.com/hlmd/PostmanCn/releases汉化网址

下载app.zip（88MB）之后把他扔到postman的resources文件夹中解压即可

地址：C:\Users\你的用户名\AppData\Local\Postman\app-9.6.1\resources

重启软件汉化完成



新建 -> HTTP -> get/post... -> 地址（用json-server xx 监听数据得到的地址） -> 发送  有数据就是成功

 要是在db.json本身上修改了数据，必须重启

```

![1709185151138](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709185151138.png)

#### 1. **400:客户端错误**

1.一般是前端传递的参数错误

#### 2. **接口通了200，还走catch**

##### (1) **彻底没有then**

资源分配不均，多个异步请求，可以做成同步请求或者不在同一个地方发送多个请求

##### (2) **走then过程出现错误**

1.当axios请求完成后走的时then的代码块，如果then代码块中存在错误代码信息，这时就会进入catch中抛出异常（注意：此时控制台并不会报错，因为错误被catch捕获了）

2.axios是异步发起，若发起后页面刷新，那么就会丢失当前进程，导致接收不到。例如 form表单，点击按钮提交后，表单会刷新

3.后端返回的数据格式不对

例子1：

![1709185172055](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709185172055.png)

所以 ：200 了 还是走catch；

前端解决：修改then里的逻辑，（因为数据格式不对，then中的逻辑会报错，加一个判断）

 

 

http.post(API_URL.selectSaleStock(this.ApiServerEnv), params)

​                .then(res => {

​                    if (res.data.code === 0) {

​                        if (res.data.data) {

​                            this.tableData = res.data.data.itemList || []

​                            this.total = res.data.data.total || 0

​                            if (this.tableData.length === 0) {

​                                setTimeout(() => {

​                                    this.$Message.warning('未查询到相关数据!')

​                                }, 30)

​                            }

​                        } else {

​                            this.tableData = []

​                            if (this.tableData.length === 0) {

​                                setTimeout(() => {

​                                    this.$Message.warning('未查询到相关数据!')

​                                }, 30)

​                            }

​                        }

​                    } else {

​                        this.$Message.error(res.data.msg)

​                    }

​                })

​                .catch(() => {

​                    // this.tableData = mockTableData

​                })

后端解决：返回数据 data:[] 而不是 null

4.封装的axios,添加了响应拦截器

例子2：

axios.interceptors.response.use(

  response => {

if (res.code !== 'ok') {

​      if (response.headers['content-type'].search('octet-stream') > -1) {

​        return response;

​      }

​      Message({

​        message: res.message || 'Error',

​        type: 'error',

​        duration: 4 * 1000

​      });

​      return Promise.reject(new Error(res.message || 'Error'));

​    } else {

​      return response;

​    }

}

原因：

经过排查，同事axios会判断返回的 res数据 是否包含 code === ‘ok’

若包含，则会返回resolve() （进入 .then）

若不包含。则会进入上述if 语句

因为二进制数据流，直接返回的就是二进制数据流，不包含res.code。所以会进入 上面代码的if

if (response.headers[‘content-type’].search(‘octet-stream’) > -1) {

return response;

}

这句话会判断后端返回的http中的header ： ‘content-type’ 信息 ，是否包含特殊标识，

包含则不进入catch，不包含则进入。

解决方案：

后端添加 content-type

#### 1. **500: 原因**

1.向后端传递的数据格式不对

后端通常会希望获取到json格式的数据,

所以通常请求的headers里要设置content-type:application/json

 

headers: {

​    "Content-Type": "application/json",

}

 

一般来说这么做是没有问题的,但是就怕忘了设置header,又多此一举的改变了body中数据的类型:

 

fetch('url', {

​    method: 'POST',

​    body: JSON.stringify({

​        xxx:xxx,

​    }),

})

 

就是这么多此一举,

 

后端希望得到json, 于是我自己将post数据的body变成了一个json字符串,同时我又忘记设置了header

的content-type,会出现什么呢?从开发者工具看到请求的header里的content-type变成了text/plain,

所以服务器拿不到数据json,和期望的数据不同,自然就很有可能跑挂了,报500的可能性就

解决办法：

1.直接传原始数据，不对数据进行转化，让浏览器自动识别

其实浏览器是很智能的.

如果没有设置header的conten-type, 在body中传递了对象

 

fetch('url', {

​    method: 'POST',

​    body: {

​        xxx:xxx,

​    },

})

 

浏览器会自动将content-type改成application/json,

 

如果是用了formdata

 

const body = new FormData()

body.append('xxx', 'xxx')

fetch('url', {

​    method: 'POST',

​    body,

})

 

浏览器也可以很好的识别出请求的content-type,

所以如果可以的话, 尽量不要自己设置这个字段了,

用正确的数据类型去告诉浏览器要用什么content-type就可以了

 

2.对数据进行了转化，那么就要在header中用 content-type 告诉浏览器数据进行了 什

### 部署

#### 1. **Vercel 部署**

vercel.com   部署  

github 账号 

依赖vercel平台（为vue项目提供部署）前端部署 Jenkins后端平台 

登录 vercel login 与github关联 （点 红框中的 sign in） 在继续关联 github 进入 

如果想要让自己的mock 可以使用，那么 在项目的 package.json中  

​    "build": "vue-cli-service build --mode development" 

 将项目添加到 github中 （与vercel 关联的 仓库）  

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps44.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps45.jpg) 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps46.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps47.jpg)![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps48.jpg) 

#### 2. **Jikens部署**

登录jikens，选择需要部署的环境，构建就可以了。

查看部署项目相关的分支，在设置：（与git相关的分支）

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml10700\wps49.jpg) 