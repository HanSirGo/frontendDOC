## 使用rollup打包ts文件

**安装依赖**

```
# 全局安装rollup
npm install rollup -g

# 安装TypeScript
npm i typescript -D

# 安装TypeScript转换器
npm i rollup-plugin-typescript2 -D

# 安装代码压缩插件
npm i rollup-plugin-terser -D

# 安装rollupweb服务
npm i rollup-plugin-serve -D

# 安装热更新
npm i rollup-plugin-livereload -D

# 安装配置环境变量用来区分本地和生产
npm i cross-env -D
```

**步骤**

1. 安装依赖
2. npm init -y 创建配置package.json文件
3. tsc --init 创建tsconfig.json文件
4. 创建src public 文件夹 rollup.config.js 文件
5. 配置 rollup.config.js

```json
# package.json文件
{
	"name":"rollupTs",
    ...
    "script" : {
        "build": "rollup -c",
        "dev": "rollup -c -w"
    }
}
```

```
node_modules
public
	- index.html
src
	- index.ts
package.json
package-lock.json
rollup.config.js
tsconfig.json
```

```js
# rollup.config.js

import path from 'path'
import ts2 from 'rollup-plugin-typescript2'
import serve from 'rollup-plugin-serve'
import livereload from 'rollup-plugin-livereload'
import { terser } from 'rollup-plugin-terser'
export default {
    input: "./src/index.ts",
    output: {
        file: path.resolve(__dirname,"./dist/index.js"),
        format: "umd",
        sourcemap: true// 在tsconfig.json中将 sourceMap也设置为true
    },
    plugin: {
        ts2(),
        livereload(),
    	terser(),
        serve({
            open: true,
            port: 8080,
            openPage: "/plugin/index.html"
        })
    }
}
// 环境变量
console.log(process.env)
// 安装cross-env插件之前，找不到NODE_ENV这个属性。
```

```
# 安装配置环境变量用来区分本地和生产
npm i cross-env -D

# package.json文件
{
	"name":"rollupTs",
    ...
    "script" : {
        "build": "cross-env NODE_ENV=production rollup -c",
        "dev": "cross-env NODE_ENV=development  rollup -c -w"
    }
}
```



## 使用webpack打包ts文件

**安装依赖**

```
# 安装webpack环境
npm install webpack webpack-cli webpack-dev-server -D

# 安装TypeScript
npm i typescript -D

# 编译TypeScript
npm i ts-loader -D

# 安装热更新服务
npm i webpack-dev-server -D

# HTML模板
npm i html-webpack-plugin -D
```

**步骤**

1. 安装依赖
2. npm init -y 创建配置package.json文件
3. tsc --init 创建tsconfig.json文件
4. 创建初始化文件
5. 进行配置

```json
# package.json文件
{    
    "name":"rollupTs",
 ...    
    "script" : {
        "build": "webpack",
        "dev": "webpack serve --open"
    }
}
```

```
node_modules
public
	- index.html
src
	- index.ts
package.json
package-lock.json
webpack.config.js
tsconfig.json
```

```js
# webpack.config.js
const path = require('path')
const htmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    entry: "./src/index.ts",
    
    output: {
        path: path.resolve(__dirname,'dist'),
        filename:'index.js'
    },
    
    module: {
        rules: [
            {
                test: /\.ts$/,
                use: 'ts-loader',
                exclude: /node_modules/
            }
        ]
    },
    
    mode: "development",
    
    resolve: { // 解决导入模块化时报错
        extensions: [".ts",".js"]
    },
    
    plugin: [
        new htmlWebpackPlugin({
            template: "./public/index.html"
        })
    ]
}
```

