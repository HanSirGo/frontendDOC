# 摆脱 try-catch！前端项目中优雅处理 async/await 异常

​       在我们的项目中，经常会遇到使用 try-catch 来处理 async/await 异常的情况。但是，这种写法让代码显得有些混乱，逻辑断层，不易理解。而且，大量的重复代码也会导致代码冗余，让代码变得臃肿。

![1726296891662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726296891662.png)



​       在本文中，我们将探讨 async/await 异常的处理时机和原因，并介绍一种优雅的处理方法，让我们摆脱冗余的 try-catch 代码。

​      什么时候会发生异步请求异常？在了解如何处理异常之前，我们先来了解一下异步请求异常发生的时机。异步请求可能会因以下原因导致异常：

1. **网络问题：网络断开连接或请求超时。**
2. **其他异常情况。**

​      何时需要处理请求异常？一旦出现上述情况，异步请求就会产生异常。由于 JavaScript 是单线程语言，一旦代码报错，后续的代码就无法继续执行。因此，在这种情况下，我们需要添加 try-catch 来捕获异步请求的异常，以确保代码能够继续执行。

​      但是，是否需要为所有的异步请求都添加 try-catch 呢？经过研究发现，在我们的项目中，只有以下几种情况需要添加 try-catch 处理：

1. **多个异步请求串行执行，后一个请求需要前一个请求的结果作为参数。**
2. **需要处理异步请求的 loading 状态，确保请求完成后及时更新状态。**

​      那么，如何优雅地处理异步请求中的 try-catch 呢？解决方案 使用 Promise 进行处理 首先，需要明确一点：await 后面通常是一个 Promise 对象。因此，我们可以在 Promise 对象上使用 .catch 方法来捕获异常。对于仅需处理 loading 状态的情况，可以直接在 .catch 中进行处理，并使用 if 条件判断提前退出，无需写冗余的 try-catch 代码。

例如，处理 loading 状态的异步请求可以改为：

```
loading.value = true;
let res = await getList().catch(() => (loading.value = false));
if (!res) return;// 请求成功后的操作
```

使用 await-to-js 库进行处理 对于复杂的多个异步操作，我们可以借助 await-to-js 库来优雅地处理异常。这个库的主要特点是：无需使用 try-catch 即可轻松处理错误。让我们一起来看看它的用法。

首先，安装 await-to-js 库：

```
# 使用 npm
npm i await-to-js --save
# 使用 yarn
yarn add await-to-js
```

然后，引入 await-to-js 库，并使用 to 函数来改造异步请求的异常处理：

```
import to from 'await-to-js';

// 获取列表list
const [err, data] = await to(getList(params));
if (err) return;
// 获取单个详情
const info = await to(getListById(list[0]?.id));
```

通过 to 函数的改造，如果返回的第一个参数不为空，则说明该请求出错，我们可以提前返回，否则表示异步请求成功。

**总结：**

本文通过研究 async/await 异常处理，发现了两种常见的异常捕获情况，并提出了两种简洁的解决方案。通过这些方法，我们可以摆脱冗余的 try-catch 代码，让代码更加清晰易读。

