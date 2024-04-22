# NPM 与 NPX

## 一、介绍

在开发中，时常在安装依赖的命令中会看到npx，但是又不一定知道 npx 到底是什么，和 npm 有什么关系和区别。今天就来探究一下！

你要说NPM，就不能只讲npm-cli，大部分开发人员只是用了它，但是并没有深入了解。需要说整个NPM。

## 二、NPM是什么

npm包含三个部分：

### 1、npm website（NPM 网站）

![1713687847476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687847476.png)用来搜索npm包和管理自己包的权限等。

### 2、npm CLI（命令行工具）

![1713687859467](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687859467.png)平时开发用得最多的和npm 交互的方式。

### 3、npm Registry（NPM 注册表）

> ❝
>
> npm Registry是一个大型的 JavaScript 软件公共数据库。因为它的注册表是全球性的，这意味着任何人都可以在任何地方搜索和使用npm的资源。这种开放性和可访问性使得npm成为了世界上最大的数据库之一。
>
> ❞

也就是说，npm提供了全球共用的注册表。想要使用npm与包进行交互，都必须在npm注册表中登记。

至于包的存放位置，也就是各自的数据库。标准语言来说，就是npm的源。

开放包：存在http://registry.npmjs.org/数据库。

私有包：存在私有的服务器。

例如淘宝镜像：http://registry.npm.taobao.org

## 三、npm源查看与设置

### 1、查看npm源（数据库）信息

```
npm config get registry
```

### 2、设置npm源（数据库）信息

```
npm config set registry npm源地址
```

总结一下，npm总的来说，大概就是下面这张图。这里是私服包示例![1713687884259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687884259.png)

## 四、npm vs. npx

这个时候，我们再来看npx和npm。

> ❝
>
> npm，是指Node.js的包管理器。
>
> ❞

这里是不是和上面说法有一些冲突？并不是，因为npm主要是为Node.js开发而设计的包管理器，上面说的三点也都是npm围绕着Node.js包管理器而必要的产物。

> ❝
>
> NPX 是一个 Node 包执行器，该 Node 包可以是本地也可以是远程的。允许开发者在无需安装的情况下执行任意 Node 包。
>
> ❞

不理解？

## 五、示例

先安装npx

```
npm install -g npx
```

### 1、调用项目安装的模块

npx 想要解决的主要问题，就是调用项目内部安装的模块。比如，项目内部安装了snowpack

```
npm install -D snowpack
```

一般来说，调用 snowpack ，只能在项目脚本和 package.json 的scripts字段里面， 如果想在命令行下调用，必须像下面这样。

```
node_modules/.bin/snowpack --version
```

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687906519.png)npx 就是想解决这个问题，让项目内部安装的模块用起来更方便，只要像下面这样调用就行了。

```
npx snowpack --version
```

![1713687925123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713687925123.png)

为什么npx可以这样，这样就需要了解npx的原理啦。

### 2、npx原理

npx 的原理很简单，就是运行命令的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在。如果已经安装了，它将直接执行该命令，否则将临时下载这个命令，并在运行完后删除它。

也就是说，如果安装了，那它就去直接执行命令，如果没有安装，npx会临时下载这个包，然后执行命令，用完以后就删除掉，你可能会说我要是执行10次就需要下载10次了？是的，这样可以保证每次执行命令时候，都是最新的版本。不过，npx会缓存一些东西，后面执行时候快一点点。

这样就好理解，为什么说npx是 Node 包执行器啦。它做的事情就是执行命令，只不过如果没有安装，它会自动去下载下来执行。

### 3、系统命令调用

由于 npx 会检查环境变量$PATH，所以系统命令也可以调用。

```js
# 等同于 ls
$ npx ls

# 等同于 mkdir components
$ npx mkdir components
```

注意，Bash 内置的命令不在$PATH里面，所以不能用。比如，cd是 Bash 命令，因此就不能用npx cd。

### 4、避免全局安装模块

我们始终记住npx的特点，它会临时下载包，所以 npx 还能避免全局安装的模块。比如，create-react-app这个模块是全局安装，npx 可以运行它，而且不进行全局安装。

```
npx create-react-app my-react-app
```

上面代码运行时，npx 将create-react-app下载到一个临时目录，使用以后再删除。

想一想，这样是不是还给你节约电脑存储空间啦 哈哈。