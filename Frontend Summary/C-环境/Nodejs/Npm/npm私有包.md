# npm私有包

## **npm是什么**

npm是Node Package Manager的缩写，是一个用于JavaScript包管理的工具。它是Node.js生态系统中的一部分，用于帮助开发者在项目中管理和共享代码包。npm的主要功能包括：

1. **包管理：** npm允许开发者通过命令行工具安装、升级、删除和管理JavaScript包（也称为模块或库）。这些包可以包含代码、依赖关系、配置文件等，以便在项目中共享和重用。
2. **依赖管理：** npm能够管理项目所需的依赖关系，并确保这些依赖关系的正确版本被安装。这有助于确保项目的稳定性和可重复性。
3. **版本控制：** npm使用语义版本控制（Semantic Versioning）来管理包的版本。这有助于开发者了解何时可以安全地更新其项目中的依赖项。
4. **命令行工具：** npm提供了一个命令行界面，使开发者能够方便地执行各种与包管理相关的任务，如安装、升级、搜索和发布包等。
5. **包的发布和共享：** 开发者可以使用npm将他们的JavaScript包发布到npm注册表中，这是一个用于存储和共享JavaScript包的在线仓库。其他开发者可以通过npm安装并使用这些包。

## **什么是打包**

在软件开发中，打包（Packaging）是指将应用程序或项目的各个组成部分（例如源代码文件、依赖项、配置文件等）整合到一个或多个文件中，以便于分发、部署或交付给最终用户。前端常见的项目构建工具（如Webpack、Parcel、Rollup等），则打包通常包括构建输出物，这些文件包含了经过编译、压缩和优化的代码。在深入对比Webpack、Parcel、Rollup打包工具中，我们总结了，rollup相比于webpack更适合打包一些第三方的类库，因此本文主要通过rollup来进行打包。

## **什么是npm域级包**

npm的域级包（Scoped Packages）是一种npm包的命名约定，它允许开发者在包的名称前添加一个命名空间，通常使用“@”符号。这个命名空间通常与组织、公司或个人的名称相关联，以确保包名称的唯一性。

域级包的命名格式如下：

```
@scope/package-name
```

其中，`@scope`是域名，可以是组织、公司或个人的名称，`package-name`是包的实际名称。例如，一个名为`my-package`的公司或组织`example`的域级包可以命名为：

```
@example/my-package
```

使用域级包的优势包括：

1. **唯一性：** 通过使用域名作为前缀，可以确保包名称的唯一性。即使不同的组织、公司或个人使用相同的包名称，由于它们的域名不同，这些包也会被区分开来。
2. **组织和结构：** 域级包有助于将包组织成逻辑上相关的集合。这对于大型项目、公司内部项目或共享包的情况非常有用。

要发布和安装域级包，可以使用`npm publish`和`npm install`命令，并在包名称前加上域名前缀。例如：

```js
# 发布域级包
npm publish --access public

# 安装域级包
npm install @example/my-package
```

## **发布指定文件**

当你使用`npm publish`命令发布包时，默认情况下，npm会将整个项目目录发布到npm注册表。如果你只想发布项目的特定文件或目录，可以通过`.npmignore`文件或`.gitignore`文件来定义不需要发布的文件或目录，从而实现选择性发布。

以下是一些步骤，以及如何使用`.npmignore`或`.gitignore`来限制发布的文件：

### **使用`.npmignore`文件**

1. **创建 .npmignore 文件：** 在项目根目录下创建一个名为`.npmignore`的文件。
2. **编辑 .npmignore 文件：** 在`.npmignore`文件中列出你不想发布的文件或目录，就像编辑`.gitignore`文件一样。

```js
# .npmignore

# 不发布的文件或目录
node_modules/
test/
docs/
```

3. **运行 npm publish 命令：** 确保`.npmignore`文件包含你希望排除的文件和目录后，运行`npm publish`命令。

### **使用`.gitignore`文件**

如果项目中已经存在`.gitignore`文件，你也可以使用它来达到相同的目的。

1. **编辑 .gitignore 文件：** 在`.gitignore`文件中列出你不想发布的文件或目录。

```js
# .gitignore

# 不发布的文件或目录
node_modules/
test/
docs/
```

2. **在 package.json 文件中添加 "files" 字段：** 如果你使用`.gitignore`而不是`.npmignore`，你还需要在`package.json`文件中添加一个`"files"`字段，以确保只发布`.gitignore`中列出的文件。

```js
// package.json

{
  "name": "your-package",
  "version": "1.0.0",
  "files": [
    "dist/",
    "src/",
    "README.md"
  ],
  // 其他 package.json 配置...
}
```

3. **运行 npm publish 命令：** 确保`.gitignore`文件包含你希望排除的文件和目录，然后运行`npm publish`命令。

## **npm包项目开发**

### **创建项目**

创建名为hello-npm文件夹，然后在该目录下，执行npm init -y初始化一下

```js
/hello-npm
├─ .babelrc.json    //babel配置文件
├─ .gitignore       
├─ LICENSE
├─ package-lock.json
├─ package.json
├─ README.md
├─ rollup.config.js  //rollup构建配置文件
├─ src
│  ├─ deepClone.ts
│  ├─ index.ts      //主入口文件
│  ...
└─ tsconfig.json    //ts配置文件 可以用tsc命令生成
```

### **安装依赖**

```js
//ts相关包
npm i -D typescript tslib 

//rollup相关包
npm i -D rollup @rollup/plugin-node-resolve rollup-plugin-commonjs rollup-plugin-typescript

//babel相关
npm i -D @rollup/plugin-babel 
npm i -D @babel/core @babel/preset-env
```

### **配置tscofig.json**

全局安装typescript，用tsc命令生成tscofig.json文件，你也可以自己手动新建

```js
npm i -g typescript
tsc --init


// tsconfig.json
{
  "compilerOptions": {
    "target": "es5",  // 要改成es5，不然babel转换es6的时候有些转换不了
    "module": "commonjs",
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitAny": false,
    "noImplicitThis": false,
    "skipLibCheck": true
  }
}
```

### **配置babel**

```js
// .babelrc.json
{
  "presets": [
    "@babel/env"
  ]
}
```

### **配置rollup**

```js
// rollup.config.js
import resolve from "@rollup/plugin-node-resolve";
import babel from "@rollup/plugin-babel";
import commonjs from "rollup-plugin-commonjs";
import typescript from "rollup-plugin-typescript";

export default {
  input: "src/index.ts", // 打包入口
  output: {
    // 打包出口
    file: "dist/index.js",
    format: "umd", // umd是兼容amd/cjs/iife的通用打包格式，适合浏览器
    name: "utilibs", // cdn方式引入时挂载在window上面用的就是这个名字
    sourcemap: true,
  },
  plugins: [
    // 打包插件
    resolve(), // 查找和打包node_modules中的第三方模块
    commonjs(), // 将 CommonJS 转换成 ES2015 模块供 Rollup 处理
    typescript(), // 解析TypeScript
    babel({ babelHelpers: "bundled" }), // babel配置,编译es6
  ],
};
```

### **实现deepClone方法**

这里我们故意用es6的箭头函数来写，后面测试babel有没有把他编译成es5的语法

```js
// src/deepClone.ts
/**
 * 深拷贝
 * @param obj 需要深拷贝的对象
 * @returns
 */
const deepClone = (obj: Object) => {
  // 不是引用类型或者是null的话直接返回
  if (typeof obj !== "object" || typeof obj == null) {
    return obj;
  }
  // 初始化结果
  let result: object;
  if (obj instanceof Array) {
    result = [];
  } else {
    result = {};
  }

  for (let key in obj) {
    // 保证不是原型上的属性
    if (obj.hasOwnProperty(key)) {
      // 递归调用
      result[key] = deepClone(obj[key]);
    }
  }
  return result;
};

export default deepClone;
```

在主入口文件处引入刚刚写的深拷贝

```js
// src/index.ts

import deepClone from "./deepClone";

export {
  deepClone
};

```

### **配置package.json**

1. 增加一个main属性，到时候打包发布到npm以后在项目里import引入时的主入口文件，和rollup的打包出口文件相对应
2. 增加script打包命令

```js
{
  "name": "ldp-hello-npm",
  "version": "1.0.0",
  "description": "js通用方法库",
  "main": "index.js",    //主入口文件
  "type": "module",
  "scripts": {
    "build": "rollup --config",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "toolkit",
    "rollup",
    "typescript"
  ],
  "author": "lh",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.23.7",
    "@babel/preset-env": "^7.23.7",
    "@rollup/plugin-babel": "^6.0.4",
    "@rollup/plugin-node-resolve": "^15.2.3",
    "rollup": "^4.9.2",
    "rollup-plugin-commonjs": "^10.1.0",
    "rollup-plugin-typescript": "^1.0.1",
    "tslib": "^2.6.2",
    "typescript": "^5.3.3"
  }
}
```

### **打包编译**

```
npm run build
```

可以看到打包成功了

![1713680005744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680005744.png)

看看dist目录下的产物

1. 可以看到确实生成了一个index.js对应我们rollup配置的打包出口文件,这也是我们package.json中配置的main属性到时候导入要用到的入口文件
2. 刚才故意用箭头函数写的深拷贝也成功被转成了es5的语法

![1713680017200](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680017200.png)

### **本地调试模拟npm包安装导入验证**

准备一个正常的项目，我们这里使用vite工具搭建一个简易的名为vite-project项目，如要搭建完善的项目脚手架请看我之前发的公众号

```js
npm create vue@latest
cd <your-project-name>
npm install
npm run dev
```

![1713680040847](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680040847.png)

把我们hello-npm中的package.josn和README.md拷贝到dist目录下

![1713680052203](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680052203.png)

用npm link链接到全局

```
cd dist
npm link
```

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680065923.png)

提示成功，可以去自己全局安装的npm的node_modules下找一找是不是有

![1713680074363](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680074363.png)

接下里去vite-project项目里link这个包， 这里注意要带上包名（package.json里面配置的name）

```
 npm link ldp-hello-npm
```

取消link

```
npm unlink ldp-hello-npm
```

![1713680092007](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680092007.png)

这里用npm link引入以后有eslint的报错的话加个.eslintignore文件忽略链接到的这个地址的eslint检查

在vite-project的main.ts里面引入写上这样一段代码测试一下有没有生效

```js
// src/main.ts
import { deepClone } from "ldp-hello-npm";

const obj: any = { a: 100, b: { c: 200 } };
const objCopy = deepClone(obj);
obj.b.c = 300;
console.log("obj:", obj);
console.log("objCopy", objCopy);
```

这里import引入utilibs时没有代码补全提示，并且启动有找不到模块的报错

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680108551.png)

我们回到hello-npm项目中：

1. tsconfig.json配置修改

```js
{
  "compilerOptions": {
    "target": "es5",  // 要改成es5，不然babel转换es6的时候有些转换不了
    "module": "commonjs",
    "declaration": true, // 根据ts文件自动生成.d.ts声明文件和js文件
    "emitDeclarationOnly": true, // 只输出.d.ts声明文件，不生成js文件
    "outDir": "./dist", // 输出目录
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitAny": false,
    "noImplicitThis": false,
    "skipLibCheck": true
  }
}
```

2. 修改package.json,增加scripts脚本和types属性

```js
  "types": "index.d.ts",  // 声明文件的主入口
  "scripts": {
    "build:types": "tsc",
    "build": "npm run build:types && rollup --config",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

3. 重新去执行npm run build，就可以生成声明文件了

![1713680162898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680162898.png)

1. 切回vite-project项目看看，就有引入提示了

![1713680173297](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680173297.png)

## **发布到npm**

如果以前改过npm的镜像地址，比如使用了淘宝的镜像，就先改回来

```
npm config set registry https://registry.npmjs.org/
```

本地验证结束，自然是要发布到npm上去了，先去npm官网上面注册一个npm的账号

- 注意dist目录下要有package.json和README.md, npm官网上显示的信息都是这里面读取；
- package.json里的version每次发布都要比之前的大

```js
cd dist
npm addUser or npm login // 添加账户输入账号密码邮箱和邮箱验证码
```

打开链接验证（由于我添加过之后再次执行上边命令就会验证账号）![1713680200247](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680200247.png)

如果不知道当前登录的账号可以用who命令查看身份：

```
npm who am i
npm profile get // 查看绑定的账号信息
```

还可以用下面命令退出当前账号

```
npm logout
```

登录成功就可以将我们的包推送到npm上去了

```
npm publish
```

![1713680223755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680223755.png)

![1713680246192](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713680246192.png)

## **发布npm包时可能会遇到的一些坑**

1. 邮箱未验证

```js
npm ERR! publish Failed PUT 403
npm ERR! code E403
npm ERR! you must verify your email before publishing a new package: https://www.npmjs.com/email-edit : your-package
```

2. 没有权限发布

```js
npm ERR! publish Failed PUT 403
npm ERR! code E403
npm ERR! You do not have permission to publish "your-package". Are you logged in as the correct user? : your-package
```

包和别人的包重名了。修改包名 3. 源设置成第三方源，比如设置了淘宝镜像。只要把源设为默认的即可

```
npm config set registry http://registry.npmjs.org
```

npm包开发模板源码：https://github.com/LHNB521/hello-npm.git

