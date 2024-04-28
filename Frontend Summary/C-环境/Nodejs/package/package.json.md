## package.json

> npm是前端开发人员广泛使用的包管理工具，项目中通过package.json来管理项目中所依赖的npm包的配置。package.json就是一个json文件，除了能够描述项目的包依赖外，允许我们使用“语义化版本规则”指明你项目依赖包的版本，让你的构建更好地与其他开发者分享，便于重复使用。

### package.json

#### 1. package.json简介

```js
// 在nodejs项目中，package.json是管理其依赖的配置文件，通常我们在初始化一个nodejs项目的时候会通过：

npm init
```

```json
// 然后在你的目录下会生成3个目录/文件， node_modules, package.json和 package.lock.json。其中package.json的内容为：

{
    "name": "Your project name",
    "version": "1.0.0",
    "description": "Your project description",
    "main": "app.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
    },
    "author": "Author name",
    "license": "ISC",
    "dependencies": {
        "dependency1": "^1.4.0",
        "dependency2": "^1.5.2"
    }
}
// 上述可以看出，package.json中包含了项目本身的元数据,以及项目的子依赖信息(比如dependicies等)。
```

#### 2. package-lock.json

> 我们发现在npm init的时候，不仅生成了package.json文件，还生成了package-lock.json文件。那么为什么存在package.json的情况下，还需要生成package-lock.json文件呢。本质上package-lock.json文件是为了锁版本，在package.json中指定的子npm包比如：react: "^16.0.0"，在实际安装中，只要高于react的版本都满足package.json的要求。这样就使得根据同一个package.json文件，两次安装的子依赖版本不能保证一致。

```js
// 而package-lock文件如下所示，子依赖dependency1就详细的指定了其版本。起到lock版本的作用。

{
    "name": "Your project name",
    "version": "1.0.0",
    "lockfileVersion": 1,
    "requires": true,
    "dependencies": {
        "dependency1": {
            "version": "1.4.0",
            "resolved": 
"https://registry.npmjs.org/dependency1/-/dependency1-1.4.0.tgz",
            "integrity": 
"sha512-a+UqTh4kgZg/SlGvfbzDHpgRu7AAQOmmqRHJnxhRZICKFUT91brVhNNt58CMWU9PsBbv3PDCZUHbVxuDiH2mtA=="
        },
        "dependency2": {
            "version": "1.5.2",
            "resolved": 
"https://registry.npmjs.org/dependency2/-/dependency2-1.5.2.tgz",
            "integrity": 
"sha512-WOn21V8AhyE1QqVfPIVxe3tupJacq1xGkPTB4iagT6o+P2cAgEOOwIxMftr4+ZCTI6d551ij9j61DFr0nsP2uQ=="
        }
    }
}
```

### **必须属性**

package.json中最重要的两个字段就是name和version，它们都是必须的，如果没有，就无法正常执行npm install命令。npm规定package.json文件是由名称和版本号作为唯一标识符的。

#### 1. name

name很容易理解，就是项目的名称，它是一个字符串。在给name字段命名时，需要注意以下几点：

- 名称的长度必须小于或等于214个字符，不能以“.”和“_”开头，不能包含大写字母（这是因为当软件包在npm上发布时，会基于此属性获得自己的URL，所以不能包含非URL安全字符（non-url-safe））；
- 名称可以作为参数被传入require("")，用来导入模块，所以应当尽可能的简短、语义化；
- 名称不能和其他模块的名称重复，可以使用npm view命令查询模块明是否重复，如果不重复就会提示404：

![1712410167750](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712410167750.png)

如果npm包上有对应的包，则会显示包的详细信息：

![1712410191246](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712410191246.png)

实际上，我们平时开发的很多项目并不会发布在npm上，所以这个名称是否标准可能就不是那么重要，它不会影响项目的正常运行。如果需要发布在npm上，name字段一定要符合要求。

#### 2. version

version字段表示该项目包的版本号，它是一个字符串。在每次项目改动后，即将发布时，都要同步的去更改项目的版本号。版本号的使用规范如下：

- 版本号的命名遵循语义化版本2.0.0规范，格式为：**主版本号.次版本号.修订号**，通常情况下，修改主版本号是做了大的功能性的改动，修改次版本号是新增了新功能，修改修订号就是修复了一些bug；
- 如果某个版本的改动较大，并且不稳定，可能如法满足预期的兼容性需求，就需要发布先行版本，先行版本通过会加在版本号的后面，通过“-”号连接以点分隔的标识符和版本编译信息：内部版本（alpha）、公测版本（beta）和候选版本（rc，即release candiate）。

可以通过以下命令来查看npm包的版本信息，以react为例：

```js
// 查看最新版本
npm view react version
// 查看所有版本
npm view react versions
```

当执行第二条命令时，结果如下：

![1712410375057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712410375057.png)

### 描述信息

package.jaon中有五个和项目包描述信息相关的配置字段，下面就分别来看看这些字段的含义。

#### 1. description

description字段用来描述这个项目包，它是一个字符串，可以让其他开发者在 npm 的搜索中发现我们的项目包。

#### 2. keywords

keywords字段是一个字符串数组，表示这个项目包的关键词。和description一样，都是用来增加项目包的曝光率的。下面是eslint包的描述和关键词：

![1712410474558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712410474558.png)

#### 3. author

author顾名思义就是作者，表示该项目包的作者。它有两种形式，一种是字符串格式：

```js
"author": "CUGGZ <xxxxx@xx.com> (https://juejin.cn/user/3544481220801815)"
```

另一种是对象形式：

```js
"author": {
  "name" : "CUGGZ",
  "email" : "xxxxx@xx.com",
  "url" : "https://juejin.cn/user/3544481220801815"
}
```

#### 4. contributors

contributors表示该项目包的贡献者，和author不同的是，该字段是一个数组，包含所有的贡献者，它同样有两种写法：

```js
"contributors": [
  "CUGGZ0 <xxxxx@xx.com> (https://juejin.cn/user/3544481220801815)",
  "CUGGZ1 <xxxxx@xx.com> (https://juejin.cn/user/3544481220801815)"
 ]
 
 
"contributors": [
  {
   "name" : "CUGGZ0",
   "email" : "xxxxx@xx.com",
   "url" : "https://juejin.cn/user/3544481220801815"
 },
  {
   "name" : "CUGGZ1",
   "email" : "xxxxx@xx.com",
   "url" : "https://juejin.cn/user/3544481220801815"
 }
 ]
```

#### 5. homepage

homepage就是项目的主页地址了，它是一个字符串。

#### 6. repository

repository表示代码的存放仓库地址，通常有两种书写形式。第一种是字符串形式：

```ja
"repository": "https://github.com/facebook/react.git"
```

除此之外，还可以显式地设置版本控制系统，这时就是对象的形式：

```js
"repository": {
  "type": "git",
  "url": "https://github.com/facebook/react.git"
}
```

#### 7. bugs

bugs表示项目提交问题的地址，该字段是一个对象，可以添加一个提交问题的地址和反馈的邮箱：

```js
"bugs": { 
  "url" : "https://github.com/facebook/react/issues",
  "email" : "xxxxx@xx.com"
}
```

最常见的bugs就是Github中的issues页面，如上就是react的issues页面地址。

### **依赖配置**

> package.json中跟依赖相关的配置属性包含了dependencies、devDependencies、peerDependencies和peerDependenciesMeta等。
>
> dependencies是项目的依赖，而devDependencies是开发所需要的模块，所以我们可以在开发过程中需要的安装上去，来提高我们的开发效率。这里需要注意的时，在自己的项目中尽量的规范使用，形如webpack、babel等是开发依赖，而不是项目本身的依赖，不要放在dependencies中。
>
> dependencies除了dependencies和devDependencies，本文重点介绍的是peerDependencies和peerDependenciesMeta。

通常情况下，我们的项目会依赖一个或者多个外部的依赖包，根据依赖包的不同用途，可以将他们配置在下面的五个属性下：dependencies、devDependencies、peerDependencies、bundledDependencies、optionalDependencies 。下面就来看看每个属性的含义。

#### 1. dependencies

dependencies字段中声明的是项目的生产环境中所必须的依赖包。当使用 npm 或 yarn 安装npm包时，该npm包会被自动插入到此配置项中：

```js
npm install <PACKAGENAME>
yarn add <PACKAGENAME>
```

当在安装依赖时使用--save参数，也会将新安装的npm包写入dependencies属性。

```js
npm install --save <PACKAGENAME>
```

该字段的值是一个对象，该对象的各个成员，分别由模块名和对应的版本要求组成，表示依赖的模块及其版本范围。

```js
"dependencies": {
   "react": "^17.0.2",
   "react-dom": "^17.0.2",
   "react-scripts": "4.0.3",
},
```

这里每一项配置都是一个键值对（key-value）， key表示模块名称，value表示模块的版本号。版本号遵循**主版本号.次版本号.修订号**的格式规定：

- **固定版本：** 上面的react-scripts的版本4.0.3就是固定版本，安装时只安装这个指定的版本；
- **波浪号：** 比如~4.0.3，表示安装4.0.x的最新版本（不低于4.0.3），也就是说安装时不会改变主版本号和次版本号；
- **插入号：** 比如上面 react 的版本^17.0.2，表示安装17.x.x的最新版本（不低于17.0.2），也就是说安装时不会改变主版本号。如果主版本号为0，那么插入号和波浪号的行为是一致的；
- latest：安装最新的版本。

需要注意，不要把测试或者过渡性的依赖放在dependencies，避免生产环境出现意外的问题。

#### 2. devDependencies

devDependencies中声明的是开发阶段需要的依赖包，如Webpack、Eslint、Babel等，用于辅助开发。它们不同于 dependencies，因为它们只需安装在开发设备上，而无需在生产环境中运行代码。当打包上线时并不需要这些包，所以可以把这些依赖添加到 devDependencies 中，这些依赖依然会在本地指定 npm install 时被安装和管理，但是不会被安装到生产环境中。

当使用 npm 或 yarn 安装软件包时，指定以下参数后，新安装的npm包会被自动插入到此列表中：

```js
npm install --save-dev <PACKAGENAME>
yarn add --dev <PACKAGENAME>


"devDependencies": {
  "autoprefixer": "^7.1.2",
  "babel-core": "^6.22.1"
}
```

##### devDependencies 和 dependencies

本文的核心：理清 `devDependencies` 中的 dev ，到底指的是什么，搞清楚这个 dev 的概念。如此一来，`devDependencies` 和 `dependencies` 之间的区别就清晰可见了。

**一、走出 “dev” 的误区**

关于 “`devDependencies` 和 `dependencies` 有什么区别？” 这样的问题，我们随便百度、google一下都能出来很多个答案，其中**最广为流传的说法**大概就是：“ `devDependencies` 是开发环境下需要用到的依赖， `dependencies` 是生产环境下需要用到的依赖” 这样的话术，这也就是很容易让人走进误区的开端。至于为什么这么说，我们接着往下看。

**1. devDependencies 的 dev 理解误区**

存在即合理，即然这是个容易让人产生误区的话术，为什么它是搜索引擎中最容易搜索到的答案呢？其实，从某种角度来看，这个说法并没有什么毛病，只是很容易让人走进误区。如果没有实践中体会过他们的区别，就**很难真正的理解这句话**，这也是让我们掉进误区的原因。

其中最大的误区便是对 “`dev`” 的理解。这么说可能不够清晰，笔者把它转述成一个问题：

- 安装在 `devDependencies` 中的依赖，在项目执行 `build` 的时候会不会被打包进 **dist** 产物中？

上面这个问题其实很简单，大家**不要掉到笔者这个提问的坑里**。我们从正常的项目打包流程分析（不管是 webpack 还是 vite，打包的核心步骤都类似），**这里从最简化的进行分析，只为了针对上述问题。**

1. 初始化配置
2. **项目入口**
3. **依赖解析**
4. loader处理
5. ... ...

好了，看到这样的打包流程（集中关注**第2、3**点），大家应该也意识到一点：**项目打包跟 devDependencies 这个字段并没什么关系**。这样一来，上述问题的答案也就很清晰了。只要是项目中用到的依赖（且安装到 `node_modules` 中），不管这个依赖是放在 `devDependencies` 还是放在 `dependencies` ，都会被打包工具解析、构建，最后都打进 dist 产物中。

总结：**生产打包 与 devDependencies 字段无关**。`devDependencies` 中的 `dev` **并不是**指我们 dev server 时候的 `dev` ，不能简单的把 dev 理解成当前项目的 “开发环境” 。接着往下，我们通过真实的装包来验证一下这个结论。

**2. 验证 devDependencies**

为了加深大家对上述 “dev误区” 的理解，笔者这里做一个小实验。

1. 随便用vue-cli生成一个 vue2 项目，目录如下。![1714194396465](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194396465.png)
2. 当前项目的依赖情况如下图：![1714194418543](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194418543.png)可以看到，目前的 vue 是放在 `dependencies` 字段中。
3. `install` 一下依赖，然后到 `node_modules` 中找到 vue 的依赖包，并且找到对应的入口文件。![1714194438575](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194438575.png)
4. 在 vue.runtime.esm.js 中，加入一行代码。看看打包后的情况如何。![1714194450766](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194450766.png)
5. 执行 `build`。并对 dist 文件夹搜索。![1714194467497](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194467497.png)没有意外，vue2 的包给打进了该项目的 dist 包中。

大家对于上述的结果当然不会感觉到有何不妥，那接下来，笔者把 vue 的依赖信息移动到 `devDependencies` 中，然后删除掉之前的 `node_modules` 目录后重新执行 `install` ，结果如图所示。![1714194483005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194483005.png)![1714194499511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194499511.png)

这时，再重复上述步骤 3、4、5 ，对 vue2 项目进行打包，再去 dist目录中**搜索**手动添加到 vue2 源码中的 `console.log` ，结果如图所示。![1714194513490](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194513490.png)

结果就是，放在 `devDependencies` 的 vue2 在 build 时候（mode 为 production）依然会被打包进单页应用的项目中。所以，通过这个实践，就为了搞清楚一个点，`devDependencies` 的 dev 并不是指我们在业务项目开发中的 dev 和 prod，它甚至跟打包时候的 `mode` 扯不上关系。

那他们到底的区别在哪里，为什么会存在这2个字段？我们接着往下看。

**二、『npm包』的 devDependencies**

这里提到了 **npm包** ，敏感的同学可能就猜到 `devDependencies` 和 `dependencies` 的真正区别了。其实 `devDependencies` 这个字段的 **dev** 的真正含义，更多是指 **npm包** 的开发阶段所需要的依赖。

**1. npm 包的 dev**

怎么理解前面提到的 npm包 开发阶段所需要的依赖？我们大概回忆一下npm包从 开发 - 发包 的流程。

1. npm**初始化**——`package.json`。想要开发一个 npm包，最先一定是要进行初始化，执行命令 `npm init`，然后填写一些信息比如 name 、 version 、 description ...此时便会生成一个 `pakcage.json` 文件。
2. npm包的**开发**。这个阶段，也就是对 npm包 功能实现的阶段，我们会开始编写代码。然而，我们在编写npm包的时候，可能需要用到其他的库，这个时候我们就需要去**安装其他的库**。
3. npm包的**打包、发布**。npm包开发完成后，当然就是要对我们的项目进行打包，然后通过 `npm publish` 命令去发布我们的npm包。

整个 npm包的实现 大概就是这么一个流程。其中第二点，笔者提到了：**如果开发过程中需要用到其他的工具库，就要把依赖安装到当前项目里**！这就涉及到本文的重点了，要怎么安装呢？-D、还是-S？不同的命令会带来怎么样不同的后果呢？

现在，我门来通过一个具体案例开探讨这个问题的答案。

场景描述：现在要开发一个基于 element-plus 的二次封装的组件库，所以在开发调试阶段，笔者需要安装 `vue3` 、 `element-plus` ... 等等依赖，以辅助我们开发组件。

对比实验：

1. 将 `vue3` 、 `element-plus` 都放在组件库package.json的 `devDependencies` 中，然后将组件库发包。最后，在业务项目中安装该组件库，看依赖情况。![1714194539967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194539967.png)
2. 将 `vue3` 放在组件库package.json的 `devDependencies` 中，`element-plus` 放在组件库package.json的 `dependencies` 中，然后将组件库发包。最后，在业务项目中安装该组件库，看依赖情况。![1714194551868](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194551868.png)

**2. 对比实验1**

1. 首先在业务项目中安装组件库 vc-element-plus。依赖如下图：![1714194567403](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194567403.png)

   执行安装，操作如下：

   ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194575835.png)

2. 看下安装的 vc-element-plus 内部的依赖安装情况。如图：
   ![1714194588676](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194588676.png)

回顾实验1条件：

```js
"dependencies": {
  "@element-plus/icons-vue": "^2.0.6",
  "@xxx/vc-shared": "workspace:*",
  "lodash": "^4.17.21"
},
"devDependencies": {
  "@vitejs/plugin-vue": "^2.3.3",
  "element-plus": "^2.2.8",
  "vue": "^3.2.36",
  "vue-router": "4",
  "less": "^4.1.3"
}
```

根据步骤2中的图可以清晰看出，放在 `dependencies` 字段中的三个依赖包：`@element-plus/icons-vue` 、 `@xxx/vc-shared` 、 `lodash` 都被安装到组件库的 node_modules 中，而组件库位于**当前的业务项目**的 node_modules 中。换句话说，业务项目中拥有了组件库 `dependencies` 中的依赖包。

这里，笔者进行猜想，实验2中，`element-plus` 将被装到组件库的内部依赖中。紧接着，我们进行实验2验证一下猜想。

**3. 对比实验2**

1. 首先在业务项目中安装组件库 vc-element-plus（版本号对比**实验1**已经不同）。依赖如下图：![1714194622531](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194622531.png)

   同样的，删除node_module后进行装包，操作如下：

   ![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194633746.png)

2. 看下安装的 vc-element-plus 内部的依赖安装情况。如图：
   ![1714194644123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714194644123.png)

ok，这样一对比，应该就很清晰了。很明显，实验2中安装了4个依赖，其中多出来的就是我们实验二中放进 `dependencies` 中 `element-plus`。（注意，`@element-plus`是图标那些的，跟`element-plus`不是同一个依赖源）。再次回顾 `dependencies` 字段验证一下：

```js
"dependencies": {
  "@element-plus/icons-vue": "^2.0.6",
  "@xxx/vc-shared": "workspace:*",
  "lodash": "^4.17.21",
  "element-plus": "^2.2.8",
},
"devDependencies": {
  "@vitejs/plugin-vue": "^2.3.3",
  "vue": "^3.2.36",
  "vue-router": "4",
  "less": "^4.1.3"
}
```

到这里，大家应该对 `devDependencies` 和 `dependencies` 之间的区别有一个清晰的认识了。至于项目装包中什么时候使用 `-D`，什么时候使用 `-S` 也有自己的理解了～

**4. 总结 `devDependencies` 和 `dependencies` 的区别**

结论：`devDependencies` 和 `dependencies`的区别核心体现在 **npm包** 中。只要开发的项目是**发npm包**提供给外部、其他业务项目使用的，需要非常注意依赖的安装地方，因为搞不好很容易在业务使用中会出现bug。而如果只是自己项目用，**不需要发npm包**的话，把依赖安装到 `devDependencies` 或者 `dependencies` 中，实质上是没有任何区别的。

为什么在开发 npm包 的时候 不严格区分 `devDependencies` 、 `dependencies` 进行装包可能会导致业务项目的使用中出现bug呢？笔者举一个例子来加深理解：

- 假设npm包开发者不小心把 vue3 的依赖写到了 `dependencies` 中（用于开发调试的），版本是 `3.0.36`。
- 业务项目自身用了 `vue@3.0.0` 的情况下，安装了这个 npm包 ，由于 npm包 中的 `dependencies` 有 `vue@3.0.36` 这个依赖，此时会在装 npm包 的同时安装36版本的vue。
- 由于 npm包中会用到vue，代码是这样引入的：`import { onMount } from 'vue'`，此时，npm包会在自己内部的 `node_modules` 中找到 `vue@3.0.36` 的包并使用，此时就会产生 2 个 vue3 实例，就很容易出现一些奇怪的bug。（业务项目的`vue@3.0.0` 和 npm包的`vue@3.0.36`）
- **这里还要注意一点就是 externals** 。有同学可能会说，npm包打包的时候会 `externals` 掉第三方的库，比如上述中的 vue3 ，`externals` 只是保证 vue3 的代码不打包进 npm包 的代码中而已。

如果开发 npm包 中不严格区分 `devDependencies` 、 `dependencies` 的依赖安装，可能会导致用户处在使用 npm包 的时候出现问题。

#### 3. peerDependencies

有些情况下，我们的项目和所依赖的模块，都会同时依赖另一个模块，但是所依赖的版本不一样。比如，我们的项目依赖A模块和B模块的1.0版，而A模块本身又依赖B模块的2.0版。大多数情况下，这不是问题，B模块的两个版本可以并存，同时运行。但是，有一种情况，会出现问题，就是这种依赖关系将暴露给用户。

最典型的场景就是插件，比如A模块是B模块的插件。用户安装的B模块是1.0版本，但是A插件只能和2.0版本的B模块一起使用。这时，用户要是将1.0版本的B的实例传给A，就会出现问题。因此，需要一种机制，在模板安装的时候提醒用户，如果A和B一起安装，那么B必须是2.0模块。

peerDependencies字段就是用来供插件指定其所需要的主工具的版本。

```js
"name": "chai-as-promised",
"peerDependencies": {
   "chai": "1.x"
}
```

上面代码指定在安装chai-as-promised模块时，主程序chai必须一起安装，而且chai的版本必须是1.x。如果项目指定的依赖是chai的2.0版本，就会报错。

需要注意，从npm 3.0版开始，peerDependencies不再会默认安装了。

> peerDependencies是package.json中的依赖项,可以解决核心库被下载多次，以及统一核心库版本的问题。

```json
//package/pkg
----- node_modules
   |-- npm-a -> 依赖了react,react-dom
   |-- npm-b -> 依赖了react,react-dom
   |-- index.js
```

> 比如上述的例子中如果子npm包a,b都以来了react和react-dom,此时如果我们在子npm包a,b的package.json中声明了PeerDependicies后，相应的依赖就不会重新安装。需要注意的有两点：
>
> - 对于子npm包a,在npm7中，如果单独安装子npm a,其peerDependicies中的包，会被安装下来。但是npm7之前是不会的。
> - 请规范和详细的指定PeerDependicies的配置，笔者在看到有些react组件库，不在PeerDependicies中指定react和react-dom，或者将react和react-dom放到了dependicies中，这两种不规范的指定都会存在一些问题。
> - 其二，正确的指定PeerDependicies中npm包的版本，**react-focus-lock\@2.8.1**[1],peerDependicies指定的是："react": "^16.8.0 || ^17.0.0 || ^18.0.0"，但实际上，这个react-focus-lock并不支持18.x的react

#### 4. peerDependenciesMeta

> 看到“Meta”就有元数据的意思，这里的peerDependenciesMeta就是详细修饰了peerDependicies，比如在react-redux这个npm包中的package.json中有这么一段：

```json
 "peerDependencies": {
    "react": "^16.8.3 || ^17 || ^18"
  },
 "peerDependenciesMeta": {
    "react-dom": {
      "optional": true
    },
    "react-native": {
      "optional": true
    }
  }
```

> 这里指定了"react-dom","react-native"在peerDependenciesMeta中，且为可选项，因此如果项目中检测没有安装"react-dom"和"react-native"都不会报错。
>
> 值得注意的是，通过peerDependenciesMeta我们确实是取消了限制，但是这里经常存在非A即B的场景，比如上述例子中，我们需要的是“react-dom”和"react-native"需要安装一个，但是实际上通过上述的声明，我们实现不了这种提示。

#### 5. optionalDependencies

如果需要在找不到包或者安装包失败时，npm仍然能够继续运行，则可以将该包放在optionalDependencies对象中，optionalDependencies对象中的包会覆盖dependencies中同名的包，所以只需在一个地方进行设置即可。

需要注意，由于optionalDependencies中的依赖可能并为安装成功，所以一定要做异常处理，否则当获取这个依赖时，如果获取不到就会报错。

#### 6. bundledDependencies

上面的几个依赖相关的配置项都是一个对象，而bundledDependencies配置项是一个数组，数组里可以指定一些模块，这些模块将在这个包发布时被一起打包。

需要注意，这个字段数组中的值必须是在dependencies, devDependencies两个里面声明过的包才行。

#### 7. engines

当我们维护一些旧项目时，可能对npm包的版本或者Node版本有特殊要求，如果不满足条件就可能无法将项目跑起来。为了让项目开箱即用，可以在engines字段中说明具体的版本号：

```js
"engines": {
 "node": ">=8.10.3 <12.13.0",
  "npm": ">=6.9.0"
}
```

需要注意，engines只是起一个说明的作用，即使用户安装的版本不符合要求，也不影响依赖包的安装。

### 脚本配置

#### 1. script

> 在npm中使用script标签来定义脚本，每当制定npm run的时候，就会自动创建一个shell脚本，这里需要注意的是，npm run新建的这个 Shell，会将本地目录的node_modules/.bin子目录加入PATH变量。
>
> 这意味着，当前目录的node_modules/.bin子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 esbuild，只要直接写esbuild xxx 就可以了。

```js
{
  // ...
  "scripts": {
    "build": "esbuild index.js",
  }
}
{
  // ...
  "scripts": {
    "build": "./node_modules/.bin/esbuild index.js" 
  }
}
// 上面两种写法是等价的。
```

scripts 是 package.json中内置的脚本入口，是key-value键值对配置，key为可运行的命令，可以通过 npm run 来执行命令。除了运行基本的scripts命令，还可以结合pre和post完成前置和后续操作。先来看一组scripts：

```js
"scripts": {
 "dev": "node index.js",
  "predev": "node beforeIndex.js",
  "postdev": "node afterIndex.js"
}
```

这三个js文件中都有一句console：

```js
// index.js
console.log("scripts: index.js")
// beforeIndex.js
console.log("scripts: before index.js")
// afterIndex.js
console.log("scripts: after index.js")
```

当我们执行npm run dev命令时，输出结果如下：

```js
scripts: before index.js
scripts: index.js
scripts: after index.js
```

可以看到，三个命令都执行了，执行顺序是predev→dev→postdev。如果scripts命令存在一定的先后关系，则可以使用这三个配置项，分别配置执行命令。

通过配置scripts属性，可以定义一些常见的操作命令：

```js
"scripts": {
  "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
  "start": "npm run dev",
  "unit": "jest --config test/unit/jest.conf.js --coverage",
  "test": "npm run unit",
  "lint": "eslint --ext .js,.vue src test/unit",
  "build": "node build/build.js"
}
```

这些脚本是命令行应用程序。可以通过调用 npm run XXX 或 yarn XXX 来运行它们，其中 XXX 是命令的名称。例如：npm run dev。我们可以为命令使用任何的名称，脚本也可以是任何操作。

使用好该字段可以大大的提升开发效率。

#### 2. config

config字段用来配置scripts运行时的配置参数，如下所示：

```ja
"config": { "port": 3000}
```

如果运行npm run start，则port字段会映射到`npm_package_config_port`环境变量中：

```js
console.log(process.env.npm_package_config_port) // 3000
```

用户可以通过`npm config set foo:port 3001` 命令来重写port的值。

#### 3. bin

> bin属性用来将可执行文件加载到全局环境中，指定了bin字段的npm包，一旦在全局安装，就会被加载到全局环境中，可以通过别名来执行该文件。

```js
// 比如\@bytepack/cli的npm包：
"bin": {
    "bytepack": "./bin/index.js"
 },
```

```js
// 一旦在全局安装了@bytepack/cli，就可以直接通过bytepack来执行相应的命令，比如

bytepack -v
//显示1.11.0

//如果非全局安装，那么会自动连接到项目的node_module/.bin目录中。与前面介绍的script标签中所说的一致，可以直接用别名来使用。
```

#### 4. workspaces

> 在项目过大的时候，最近越来越流行monorepo。提到monorepo就绕不看workspaces，早期我们会用yarn workspaces，现在npm官方也支持了workspaces.     workspaces解决了本地文件系统中如何在一个顶层root package下管理多个子packages的问题，在workspaces声明目录下的package会软链到最上层root package的node_modules中。

```js
// 直接以官网的例子来说明：

{
  "name": "my-project",
  "workspaces": [
    "packages/a"
  ]
}
```

```js
// 在一个npm包名为my-project的npm包中，存在workspaces配置的目录。

.
+-- package.json
+-- index.js
`-- packages
   +-- a
   |  `-- package.json
```

```js
// 并且该最上层的名为my-project的root包，有packages/a子包。此时，我们如果npm install,那么在root package中node_modules中安装的npm包a，指向的是本地的package/a.

.
+-- node_modules
|  `-- packages/a -> ../packages/a
+-- package-lock.json
+-- package.json
`-- packages
   +-- a
   |   `-- package.json
```

```js
// 上述的

-- packages/a -> ../packages/a
// 指的就是从node_modules中a链接到本地npm包的软链
```

### **文件&目录**

下面来看看package.json中和文件以及目录相关的属性。

#### 1. main

main 字段用来指定加载的入口文件，在 browser 和 Node 环境中都可以使用。如果我们将项目发布为npm包，那么当使用 require 导入npm包时，返回的就是main字段所列出的文件的module.exports 属性。如果不指定该字段，默认是项目根目录下的index.js。如果没找到，就会报错。

该字段的值是一个字符串：

```ja
"main": "./src/index.js",
```

#### 2. browser

browser字段可以定义 npm 包在 browser 环境下的入口文件。如果 npm 包只在 web 端使用，并且严禁在 server 端使用，使用 browser 来定义入口文件。

```js
"browser": "./src/index.js" 
```

#### 3. module

module字段可以定义 npm 包的 ESM 规范的入口文件，browser 环境和 node 环境均可使用。如果 npm 包导出的是 ESM 规范的包，使用 module 来定义入口文件。

```js
"module": "./src/index.mjs",
```

需要注意，*.js 文件是使用 commonJS 规范的语法(require('xxx'))，*.mjs 是用 ESM 规范的语法(import 'xxx')。

上面三个的入口入口文件相关的配置是有差别的，特别是在不同的使用场景下。在Web环境中，如果使用loader加载ESM（ES module），那么这三个配置的加载顺序是browser→module→main，如果使用require加载CommonJS模块，则加载的顺序为main→module→browser。

Webpack在进行项目构建时，有一个target选项，默认为Web，即构建Web应用。如果需要编译一些同构项目，如node项目，则只需将webpack.config.js的target选项设置为node进行构建即可。如果再Node环境中加载CommonJS模块，或者ESM，则只有main字段有效。

#### 4. bin

bin字段用来指定各个内部命令对应的可执行文件的位置：

```js
"bin": {
  "someTool": "./bin/someTool.js"
}
```

这里，someTool 命令对应的可执行文件为 bin 目录下的 someTool.js，someTool.js会建立符号链接node_modules/.bin/someTool。由于node_modules/.bin/目录会在运行时加入系统的PATH变量，因此在运行npm时，就可以不带路径，直接通过命令来调用这些脚本。因此，下面的写法可以简写：

```js
scripts: {  
  start: './node_modules/bin/someTool.js build'
}

// 简写
scripts: {  
  start: 'someTool build'
}
```

所有node_modules/.bin/目录下的命令，都可以用npm run [命令]的格式运行。

上面的配置在package.json包中提供了一个映射到本地文件名的bin字段，之后npm包将链接这个文件到prefix/fix里面，以便全局引入。或者链接到本地的node_modules/.bin/文件中，以便在本项目中使用。

#### 5. files

files配置是一个数组，用来描述当把npm包作为依赖包安装时需要说明的文件列表。当npm包发布时，files指定的文件会被推送到npm服务器中，如果指定的是文件夹，那么该文件夹下面所有的文件都会被提交。

```js
"files": [
    "LICENSE",
    "Readme.md",
    "index.js",
    "lib/"
 ]
```

如果有不想提交的文件，可以在项目根目录中新建一个.npmignore文件，并在其中说明不需要提交的文件，防止垃圾文件推送到npm上。这个文件的形式和.gitignore类似。写在这个文件中的文件即便被写在files属性里也会被排除在外。比如可以在该文件中这样写：

```js
node_modules
.vscode

build

.DS_Store
```

#### 6. man

man 命令是 Linux 中的帮助指令，通过该指令可以查看 Linux 中的指令帮助、配置文件帮助和编程帮助等信息。如果 node.js 模块是一个全局的命令行工具，在 package.json 通过 man 属性可以指定 man 命令查找的文档地址：

```js
"man": [
 "./man/npm-access.1",
 "./man/npm-audit.1"
]
```

man 字段可以指定一个或多个文件, 当执行man {包名}时, 会展现给用户文档内容。

需要注意：

- man文件必须以数字结尾，如果经过压缩，还可以使用.gz后缀。这个数字表示文件安装到哪个 man 节中；
- 如果 man 文件名称不是以模块名称开头的，安装的时候会加上模块名称前缀。

对于上面的配置，可以使用以下命令来执行查看文档：

```js
man npm-access
man npm-audit
```

#### 7. directories

directories字段用来规范项目的目录。node.js 模块是基于 CommonJS 模块化规范实现的，需要严格遵循 CommonJS 规范。模块目录下除了必须包含包项目描述文件 package.json 以外，还需要包含以下目录：

- bin ：存放可执行二进制文件的目录
- lib ：存放js代码的目录
- doc ：存放文档的目录
- test ：存放单元测试用例代码的目录
- ...

在实际的项目目录中，我们可能没有按照这个规范进行命名，那么就可以在directories字段指定每个目录对应的文件路径：

```js
"directories": {
    "bin": "./bin",
    "lib": "./lib",
    "doc": "./doc",
    "test" "./test",
    "man": "./man"
},
```

这个属性实际上没有什么实际的作用，当然不排除未来会有什么比较有意义的用处。

### package.json环境相关属性

> 常见的环境，基本上分为浏览器browser和node环境两大类，接下来我们来看看package.json中，跟环境相关的配置属性。环境的定义可以简单理解如下：
>
> - browser环境：比如存在一些只有在浏览器中才会存在的全局变量等，比如window，Document等
> - node环境: npm包的源文件中存在只有在node环境中才会有的一些变量和内置包，内置函数等。

#### 1 type

> js的模块化规范包含了commonjs、CMD、UMD、AMD和ES module等，最早先在node中支持的仅仅是commonjs字段，但是从node13.2.0开始后，node正式支持了ES module规范，在package.json中可以通过type字段来声明npm包遵循的模块化规范。

```json
//package.json
{
   name: "some package",
   type: "module"||"commonjs" 
}
```

> 需要注意的是：
>
> - 不指定type的时候，type的默认值是commonjs，不过建议npm包都指定一下type
> - 当type字段指定值为module则采用ESModule规范
> - 当type字段指定时，目录下的所有.js后缀结尾的文件，都遵循type所指定的模块化规范
> - 除了type可以指定模块化规范外，通过文件的后缀来指定文件所遵循的模块化规范，以.mjs结尾的文件就是使用的ESModule规范，以.cjs结尾的遵循的是commonjs规范

#### 2 main & module & browser

> 除了type外，package.json中还有main,module和browser 3个字段来定义npm包的入口文件。
>
> - main : 定义了 npm 包的入口文件，browser 环境和 node 环境均可使用
> - module : 定义 npm 包的 ESM 规范的入口文件，browser 环境和 node - 环境均可使用
> - browser : 定义 npm 包在 browser 环境下的入口文件

```json
// 我们来看一下这3个字段的使用场景，以及同时存在这3个字段时的优先级。我们假设有一个npm包为demo1,

----- dist
   |-- index.browser.js
   |-- index.browser.mjs
   |-- index.js
   |-- index.mjs
```

```json
// 其package.json中同时指定了main,module和browser这3个字段，

  "main": "dist/index.js",  // main 
  "module": "dist/index.mjs", // module

  // browser 可定义成和 main/module 字段一一对应的映射对象，也可以直接定义为字符串
  "browser": {
    "./dist/index.js": "./dist/index.browser.js", // browser+cjs
    "./dist/index.mjs": "./dist/index.browser.mjs"  // browser+mjs
  },

  // "browser": "./dist/index.browser.js" // browser
```

```js
// 默认构建和使用，比如我们在项目中引用这个npm包：

import demo from 'demo'
```

> 通过构建工具构建上述代码后，模块的加载循序为：_**browser+mjs > module > browser+cjs > main**_这个加载顺序是大部分构建工具默认的加载顺序，比如webapck、esbuild等等。可以通过相应的配置修改这个加载顺序，不过大部分场景，我们还是会遵循默认的加载顺序。

#### 3 exports

> 如果在package.json中定义了exports字段，那么这个字段所定义的内容就是该npm包的真实和全部的导出，优先级会高于main和file等字段。

```json
// 举例来说：

{
  "name": "pkg",
  "exports": {
    ".": "./main.mjs",
    "./foo": "./foo.js"
  }
}
```

```js
import { something } from "pkg"; // from "pkg/main.mjs"
```

```js
const { something } = require("pkg/foo"); // require("pkg/foo.js")
```

> 从上述的例子来看，exports可以定义不同path的导出。如果存在exports后，以前正常生效的file目录到处会失效，比如require('pkg/package.json')，因为在exports中没有指定，就会报错。    exports还有一个最大的特点，就是条件引用，比如我们可以根据不同的引用方式或者模块化类型，来指定npm包引用不同的入口文件。

```json
// package.json
{ 
  "name":"pkg",
  "main": "./main-require.cjs",
  "exports": {
    "import": "./main-module.js",
    "require": "./main-require.cjs"
  },
  "type": "module"
}
```

```js
// 上述的例子中，如果我们通过

const p = require('pkg')


//引用的就是"./main-require.cjs"。如果通过：

import p from 'pkg'
```

> 引用的就是"./main-module.js"最后需要注意的是 ：***如果存在exports属性，exports属性不仅优先级高于main，同时也高于module和browser字段。***

### 发布配置

下面来看看和npm项目包发布相关的配置。

#### 1. private

private字段可以防止我们意外地将私有库发布到npm服务器。只需要将该字段设置为true：

```js
"private": true
```

#### 2. preferGlobal

preferGlobal字段表示当用户不把该模块安装为全局模块时，如果设置为true就会显示警告。它并不会真正的防止用户进行局部的安装，只是对用户进行提示，防止产生误解：

```js
"preferGlobal": true
```

#### 3. publishConfig

publishConfig配置会在模块发布时生效，用于设置发布时一些配置项的集合。如果不想模块被默认标记为最新，或者不想发布到公共仓库，可以在这里配置tag或仓库地址。更详细的配置可以参考 npm-config。

通常情况下，publishConfig会配合private来使用，如果只想让模块发布到特定npm仓库，就可以这样来配置：

```js
"private": true,
"publishConfig": {
  "tag": "1.1.0",
  "registry": "https://registry.npmjs.org/",
  "access": "public"
}
```

#### 4. os

os字段可以让我们设置该npm包可以在什么操作系统使用，不能再什么操作系统使用。如果我们希望开发的npm包只运行在linux，为了避免出现不必要的异常，建议使用Windows系统的用户不要安装它，这时就可以使用os配置：

```js
"os" ["linux"]   // 适用的操作系统
"os" ["!win32"]  // 禁用的操作系统
```

#### 5. cpu

该配置和OS配置类似，用CPU可以更准确的限制用户的安装环境：

```js
"cpu" ["x64", "AMD64"]   // 适用的cpu
"cpu" ["!arm", "!mips"]  // 禁用的cpu
```

可以看到，黑名单和白名单的区别就是，黑名单在前面加了一个“!”。

#### 6. license

license 字段用于指定软件的开源协议，开源协议表述了其他人获得代码后拥有的权利，可以对代码进行何种操作，何种操作又是被禁止的。常见的协议如下：

- MIT ：只要用户在项目副本中包含了版权声明和许可声明，他们就可以拿你的代码做任何想做的事情，你也无需承担任何责任。
- Apache ：类似于 MIT ，同时还包含了贡献者向用户提供专利授权相关的条款。
- GPL ：修改项目代码的用户再次分发源码或二进制代码时，必须公布他的相关修改。

可以这样来声明该字段：

```js
"license": "MIT"
```

### 第三方配置

package.json 文件还可以承载命令特有的配置，例如 Babel、ESLint 等。它们每个都有特有的属性，例如 eslintConfig、babel 等。它们是命令特有的，可以在相应的命令/项目文档中找到如何使用它们。下面来看几个常用的第三方配置项。

#### 1. typings

typings字段用来指定TypeScript的入口文件：

```js
"typings": "types/index.d.ts",
```

该字段的作用和main配置相同。

#### 2. eslintConfig

eslint的配置可以写在单独的配置文件.eslintrc.json 中，也可以写在package.json文件的eslintConfig配置项中。

```js
"eslintConfig": {
      "root": true,
      "env": {
        "node": true
      },
      "extends": [
        "plugin:vue/essential",
        "eslint:recommended"
      ],
      "rules": {},
      "parserOptions": {
        "parser": "babel-eslint"
     },
}
```

#### 3. babel

babel用来指定Babel的编译配置，代码如下：

```js
"babel": {
 "presets": ["@babel/preset-env"],
 "plugins": [...]
}
```

#### 4. unpkg

使用该字段可以让 npm 上所有的文件都开启 cdn 服务，该CND服务由unpkg提供：

```js
"unpkg": "dist/vue.js"
```

#### 5. lint-staged

lint-staged是一个在Git暂存文件上运行linters的工具，配置后每次修改一个文件即可给所有文件执行一次lint检查，通常配合gitHooks一起使用。

```js
"lint-staged": {
 "*.js": [
   "eslint --fix",
    "git add"
  ]
}
```

使用lint-staged时，每次提交代码只会检查当前改动的文件。

#### 6. gitHooks

gitHooks用来定义一个钩子，在提交（commit）之前执行ESlint检查。在执行lint命令后，会自动修复暂存区的文件。修复之后的文件并不会存储在暂存区，所以需要用git add命令将修复后的文件重新加入暂存区。在执行pre-commit命令之后，如果没有错误，就会执行git commit命令：

```js
"gitHooks": {
 "pre-commit": "lint-staged"
}
```

这里就是配合上面的lint-staged来进行代码的检查操作。

#### 7. browserslist

browserslist字段用来告知支持哪些浏览器及版本。Babel、Autoprefixer 和其他工具会用到它，以将所需的 polyfill 和 fallback 添加到目标浏览器。比如最上面的例子中的该字段值：

```js
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
```

这里指定了一个对象，里面定义了生产环境和开发环境的浏览器要求。上面的development就是指定开发环境中支持最后一个版本的chrome、Firefox、safari浏览器。这个属性是不同的前端工具之间共用目标浏览器和 node 版本的配置工具，被很多前端工具使用，比如Babel、Autoprefixer等。