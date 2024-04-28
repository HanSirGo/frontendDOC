### 一、Vite
#### 1. mode
##### import.meta.env
```javascript
import.meta.env.MODE: {string} 应用运行的模式。
import.meta.env.BASE_URL: {string} 部署应用时的基本 URL。他由base 配置项决定。
import.meta.env.PROD: {boolean} 应用是否运行在生产环境。
import.meta.env.DEV: {boolean} 应用是否运行在开发环境 (永远与import.meta.env.PROD相反)。
import.meta.env.SSR: {boolean} 应用是否运行在 server 上。
```
![1713773201346](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713773201346.png)
#### 2. 环境变量
> https://cn.vitejs.dev/config/#configuring-vite
##### .env 文件 设置 环境变量
我们可以在 .env 文件中编写自己需要的环境变量。

> .env                # 所有情况下都会加载 
> .env.local          # 所有情况下都会加载，但会被git 忽略 
> .env.[mode]         # 只在指定模式下加载 
> .env.[mode].local   #只在指定模式下加载，但会被 git 忽略

自己编写的环境变量必须以 VITE_ 为前缀，比如：
```javascript
VITE_SOME_KEY=123
DB_PASSWORD=foobar // 不合法

console.log(import.meta.env.VITE_SOME_KEY) // 123
console.log(import.meta.env.DB_PASSWORD) // undefined
```
####  3. 批量导入
##### import.meta.glob 为过动态导入，构建时，会分离为独立的 chunk
```javascript
const files = import.meta.glob('./module/*.js')

const modules = {}
for (const key in files) {
    files[key]().then(res=>{
        modules[key.replace(/(\.\/module\/|\.js)/g, '')] = res.default
    })
}

Object.keys(modules).forEach(item => {
    modules[item]['namespaced'] = true
})
```
##### import.meta.globEager 为直接引入
```javascript
const files = import.meta.globEager('./module/*.js')

const modules = {}
for (const key in files) {
    modules[key.replace(/(\.\/module\/|\.js)/g, '')] = files[key].default
}

Object.keys(modules).forEach(item => {
    modules[item]['namespaced'] = true
})
```
### 二、webpack
#### 1. mode
##### process.env

> process.env.NODE_ENV  应用运行的模式
> ![1713773187152](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713773187152.png)
```javascript
{
  "name": "",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "vue-cli-service serve", //本地开发运行，会把process.env.NODE_ENV设置为'development'
    "build": "vue-cli-service build", //默认打包模式，会把process.env.NODE_ENV设置为'production'
  },
  "dependencies": {
  }
}
```
```javascript
{
  "scripts": {
  	"serve":"vue-cli-service serve --mode development"
    "build": "vue-cli-service build --mode production"
  }
}
```
##### cross-env

> cross-env：运行跨平台设置和使用环境变量的脚本


> 您使用NODE_ENV =production, 来设置环境变量时，大多数Windows命令提示将会阻塞(报错)。 （异常是Windows上的Bash，它使用本机Bash。）同样，Windows和POSIX命令如何使用环境变量也有区别。 使用POSIX，您可以使用：$ ENV_VAR和使用％ENV_VAR％的Windows。
> 说人话：windows不支持NODE_ENV=development的设置方式。

```javascript
npm install --save-dev cross-env
```

```javascript
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```
#### 2. 环境变量设置 
![1713773174522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713773174522.png)
#### 3. 批量导入
##### require.context

> require.context(directory,useSubdirectories,regExp)
>
> directory:表示检索的目录 
> useSubdirectories：表示是否检索子文件夹
> regExp:匹配文件的正则表达式,一般是文件名

```javascript
const files = require.context("@/views/00-99/requireContext/components", false, /\.vue$/)
const modules = {}
files.keys().forEach((key) => {
  const name = path.basename(key, ".vue")
  modules[name] = files(key).default || files(key)
})
console.log(modules)
export default {
  components: modules,
}
```