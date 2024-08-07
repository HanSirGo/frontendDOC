# 团队如何巧妙统一Node版本？

在现代前端开发中，`Node.js` 作为构建工具和服务端运行环境的基石，版本不一致可能导致各种问题，如依赖库兼容性问题、开发环境与生产环境不匹配等。

在不同项目中因为创建项目时期不同等原因，可能使用的 `Node.js` 版本不同，我们基本都会安装 `nvm` 等 `Node.js` 版本管理工具，方便在不同项目下切换不同的版本。但来回切换很容易导致混淆，因此引发的一些 `bug` 还难以排查。

为了确保团队协作项目的稳定性和一致性，我们需要采取一些措施来保证项目中的 `Node` 版本一致。

以下是一些常用的方法：

## **1.package.json的engines字段**

在项目的`package.json`文件中，可以使用`engines`字段来指定所需的 `Node` 版本。在该字段中，我们可以定义一个范围或者具体的版本号来限制 `Node` 的版本。

具体`engines`配置说明如下：

```js
// 指定特定版本号
"engines": {
  "node": "14.17.0"
}

// 范围符号：表示项目需要Node版本大于等于12.0.0且小于16.0.0。
"engines": {
  "node": ">=12.0.0 <16.0.0"
}

// 波浪线符号：表示项目需要Node版本为14.17.x
"engines": { "node": "~14.17.0" }

// 插入符号：表示项目需要Node版本为14.x.x
"engines": { "node": "^14.17.0" }
```

这样做的好处是，当项目成员运行 `npm install` 时，`npm` 会自动检查 `Node` 版本是否满足要求，并给出警告或错误信息。

![1713592462711](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592462711.png)

但是，我们在使用`npm install`时，发现`engines`配置并没有起作用，然后换`yarn`安装，发现`engines`配置又起作用了。

![1713592484184](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592484184.png)

到底是什么原因导致的呢？

## **2.使用.npmrc文件**

原来 `engines` 只是建议，默认不开启严格版本校验，只会给出提示，需要手动开启严格模式。在根目录下 `.npmrc` 添加 `engine-strict = true`才会起作用。配置完成后再执行`npm install`:

```js
# .npmrc
engine-strict = true
```

```js
npm ERR! code ENOTSUP
npm ERR! notsup Unsupported engine for react_antd_admin_template@1.0.0: wanted: {"node":"14.17.5","npm":"6.14.14"} (current: {"node":"16.18.1","npm":"8.19.2"})
npm ERR! notsup Not compatible with your version of node/npm: react_antd_admin_template@1.0.0
npm ERR! notsup Not compatible with your version of node/npm: react_antd_admin_template@1.0.0
npm ERR! notsup Required: {"node":"14.17.5","npm":"6.14.14"}
npm ERR! notsup Actual:   {"node":"16.18.1","npm":"8.19.2"}
```

此时通过 `npm` 安装，限制 `Node` 版本便起作用了。

## **3.使用.nvmrc文件**

通过上面的方式，可以做到让大家使用相同的 `Node` 版本，但每次提示版本不符合，需要开发人员到`package.json`中查看版本号，然后再使用`nvm`切换指定版本，太麻烦了，高效开发不是这么干的。

我们可以创建一个`.nvmrc`文件， 指定项目 `Node` 版本：

```
# .nvmrc
v14.17.5
```

此时，执行`nvm use` 自动就切换到项目执行的 `Node` 版本。![1713592570865](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592570865.png)

`.nvmrc`文件是一个存放指定 `Node` 版本的配置文件，可以告诉项目的成员应该使用哪个`Node` 版本来运行项目。

如果我们没有安装对应版本的 `Node` ，执行时也会提示没有安装

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713592593389.png)

