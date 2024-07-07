# 深入了解Vite：依赖预构建原理

## **前言**

前面我们有提到`Vite`在开发阶段，提倡的是一个`no-bundle`的理念，不必与`webpack`那样需要先将整个项目进行打包构建。但是`no-bundle`的理念只适合源代码部分（我们自己写的代码），`vite`会将项目中的所有模块分为**依赖**与**源码**两部分。

**依赖：**指的是一些不会变动的一些模块，如：`node_modules`中的第三方依赖，这部分代码`vite`会在启动本地服务之前使用**esbuild**进行预构建。esbuild 使用 Go 编写，比使用 JavaScript 编写的打包器预构建依赖快 10-100 倍。

**源码：**指的是我们自己开发时写的那部分代码，这部分代码可能会经常变动，并且一般不会同时加载所有源代码。

所以总结来说：**no-bundle是针对源码的，而预构建是针对第三方依赖的**

## **使用预构建的原因**

主要有以下两点：

- **commonJS 与 UMD兼容**：因为`Vite`在开发阶段主要是依赖浏览器原生ES模块化规范，所以无论是我们的源代码还是第三方依赖都得符合ESM的规范，但是目前并不是所有第三方依赖都有ESM的版本，所以需要对第三方依赖进行预编译，将它们转换成EMS规范的产物。

比如`React`，它就没有`ESM`的版本，所以在使用`Vite`时需要预构建

![1720343259553](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343259553.png)

- **性能：**为了提高后续页面的加载性能，Vite将那些具有许多内部模块的 ESM 依赖项转换为单个模块。

比如常用的`loads-es`

我们引入`lodash-es`工具包中的`debounce`方法，此时它理想状态应该是只发出一个请求

```
import  { debounce }  from 'lodash-es'
```

事实也是这样

![1720343275804](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343275804.png)

但这是预构建的功劳，如果我们对`lodash-es`关闭预构建呢？

`vite`配置文件加上如下代码，再来试试：

```
// vite.config.js
optimizeDeps: {
    exclude: ['lodash-es']
  }
```

![1720343298604](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343298604.png)

可以看到，此时发起了600多个请求，这是因为`lodash-es` 有超过 600 个内置模块！

**vite通过将 lodash-es 预构建成单个模块，只需要发起一个HTTP请求！可以很大程度地提高加载性能**

> ❝
>
> 由于Vite的预构建是基于性能优异的Esbuild来完成的，所以并不会造成明显的打包性能问题

## **开启预构建**

### **默认配置**

一般来说，`Vite`帮我们默认开启了预构建

![1720343316053](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343316053.png)

预构建产物会存放在：`node_modules/.vite/deps`

![1720343329040](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343329040.png)

里面会有一个`_metadata.json`的文件，这里保存着已经预构建过的依赖信息

对于预构建产物的请求，`Vite`会设置为强缓存，有效时间为1年，对于有效期内的请求，会直接使用缓存内容

![1720343342690](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343342690.png)

如果只有HTTP强缓存肯定也不行，如果用户更新了依赖版本，在缓存过期之前，浏览器拿到的一直是旧版本的内容。

所以`Vite`对本地文件也设置了缓存判断，如果下面几个地方任意一个地方有变动，`Vite`将会对依赖进行重新预构建：

- 项目依赖`dependencies`变更

![1720343355938](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343355938.png)

- 各种包管理器的`lock`文件变更

![1720343370283](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343370283.png)

- `optimizeDeps`配置内容变更

![1720343380589](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343380589.png)

### **自定义配置**

#### **entries**

默认情况下，`Vite`会抓取项目中的`index.html`来检测需要预构建的依赖

```
optimizeDeps: {  entries: ['index.html']}
```

如果指定了 `build.rollupOptions.input`，Vite 将转而去抓取这些入口点。

#### **exclude**

排除需要预构建的依赖项

```
optimizeDeps: {  exclude: ['lodash-es']}
```



#### **include**

默认情况下，不在 `node_modules` 中的依赖不会被预构建。使用此选项可强制选择预构建的依赖项。

```
optimizeDeps: {  include: ['lodash-es']}
```

## **预构建流程**

还是从源码入手，在启动服务的过程中会执行一个`initDepsOptimizer`表示初始化依赖优化

![1720343409143](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343409143.png)

接着找到定义`initDepsOptimizer`方法的地方

![1720343421089](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343421089.png)

在这里会执行`createDepsOptimizer`方法，再接着找到定义`createDepsOptimizer`的地方

![1720343434577](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343434577.png)

这里首先会去执行`loadCachedDepOptimizationMetadata`用于获取本地缓存中的`metadata`数据

![1720343452523](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343452523.png)

该函数会在获取到`_metadata.json`文件内容之后去对比lock文件hash以及配置文件optimizeDeps内容，如果一样说明预构建缓存没有任何改变，无需重新预构建，直接使用上次预构建缓存即可

如果没有缓存时则需要进行依赖扫描

![1720343478825](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343478825.png)

这里主要是会调用`scanImport`方法，从名字也能看出该方法应该是通过扫描项目中的`import`语句来得到需要预编译的依赖

![1720343496242](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343496242.png)

最终会返回一个`prepareEsbuildScanner`方法

![1720343509614](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720343509614.png)

最后该方法中会使用`esbuild`对扫描出来的依赖项进行预编译。

