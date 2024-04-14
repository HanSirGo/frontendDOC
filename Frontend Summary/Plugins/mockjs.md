## mockjs

>  mock.js:是一款模拟数据生成器，可以[生成随机数](https://so.csdn.net/so/search?q=生成随机数&spm=1001.2101.3001.7020)据，拦截 Ajax 请求
>
> **Mock.js的特性**
>
> 1. 使用mockjs模拟后端接口，可随机生成所需数据，模拟对数据的增删改查
> 2. 数据类型丰富，支持生成随机的文本、数字、布尔值、日期、邮箱、链接、图片、颜色等。
> 3. 拦截Ajax请求不需要修改既有代码就可以拦截，返回模拟的响应数据。

### 遇到的问题

##### xhr.upload.addEventListener is not a function 错误

```js
// 这个问题主要是因为我在写后台管理的时候使用mock.js，它改变了 XhrRequest 对象的名称，改成 了MockXhrRequest 所以才会报以上的错误但是之后还要用它模拟数据，不可能将其卸掉，那么就在 node_modules 文件里面下手。解决步骤如下：

// 首先找到 node_modules/mockjs/dist/mock.js （下面图示的第一个文件）中的第8312行，加入如下代码：

MockXMLHttpRequest.prototype.upload = xhr.upload;
```

![1713093743354](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713093743354.png)

