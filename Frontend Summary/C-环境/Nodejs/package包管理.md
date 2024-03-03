#### npm

```js
npm  
	全称 node package manager node包管理器  
    npmjs.com
配置淘宝镜像:
	npm config set registry https://registry.npm.taobao.org （c盘、用户中有一个.npmrc 文件）
安装包:
	npm install jquery / npm i jquery

```

##### 在指令中带参数 -x 表示参数

```js

npm root -g 来进行查询，就可以知道全局配置的包下载的位置了 

npm i jquery -g 指全局安装 （C:\Users\Administrator\AppData\Roaming\npm）

-S,--save安装包信息将加入到dependencies (生产阶段的依赖)

-D, --save--dev安装包信息将加入到devDependencies(开发阶段的依赖)所以开发阶段

	npm i jquery  本地安装 安装在当前指令目录
	npm i jquery --save 生产环境安装 / npm i jquery -S
	npm i jquery --development 开发环境安装 /npm i jquery -D
```

```js
npm init  	生成package.json文件,存放的是项目信息 
npm init -y -y指令，直接生成简化版package.json文件
{
  "name": "day04",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": { //指令
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies":{ //上线依赖 生产环境
      "jquery": "^3.6.0"
  },
  "devDependencies":{//开发依赖  工具类
      "jquery": "^3.6.0"
  }
}

npm i @vue/cli -g 安装脚手架

vue -V 查看版本
```

##### npm 清理缓存

```
npm cache clean -f

有时候npm下载资源出错,再次下载时可能因为之前的缓存造成一直下载不成功.
此时可以清一下缓存,然后尝试重新下载.
```

##### npm i 与 npm install

```

```

#### pnpm

#### cnpm

#### yarn

```js
yarn 使用
全局安装 Yarn 的最新版本:
npm install -g yarn
如果以后要将 Yarn 更新到最新版本，请运行:
yarn set version latest
显示命令列表
yarn help
初始化一个新项目
yarn init
安装所有依赖项
yarn
yarn install
添加依赖项
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
将依赖项添加到不同的依赖类别中
yarn add [package] --dev  # dev dependencies
yarn add [package] --peer # peer dependencies
更新依赖项
yarn up [package]
yarn up [package]@[version]
yarn up [package]@[tag]
删除依赖项
yarn remove [package]
更新 Yarn 本体
yarn set version latest
yarn set version from sources
```

