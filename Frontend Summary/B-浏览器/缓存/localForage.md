# localForage

前端本地化存储算是一个老生常谈的话题了，我们对于 cookies、Web Storage（sessionStorage、localStorage）的使用已经非常熟悉，在面试与实际操作之中也会经常遇到相关的问题，但这些本地化存储的方式还存在一些缺陷，比较明显的缺点如下：

1. **存储量小**：即使是web storage的存储量最大也只有 5M
2. **存取不方便**：存入的内容会经过序列化，当存入非字符串的时候，取值的时候需要通过反序列化。

当我们的存储量比较大的时候，我们一定会想到我们的 indexedDB，让我们在浏览器中也可以使用数据库这种形式来玩转本地化存储，然而 indexedDB 的使用是比较繁琐而复杂的，有一定的学习成本，但 localForage 的出现几乎抹平了这个缺陷，让我们轻松无负担的在浏览器中使用 indexedDB。

- 既然要存储的数量大，得排除cookie
- localStorage，虽然比cookie多，但是同样有上限（5M）左右，备选
- websql 使用简单，存储量大，兼容性差，备选
- indexDB api多且繁琐，存储量大、高版本浏览器兼容性较好，备选

##  indexedDB

IndexedDB 是一种底层 API，用于在客户端存储大量的结构化数据（也包括文件/二进制大型对象）。

### 存取方便

IndexedDB 是一个基于 JavaScript 的面向对象数据库。IndexedDB 允许你存储和检索用键索引的对象；可以存储结构化克隆算法支持的任何对象。

之前我们使用 webStorage 存储对象或数组的时候，还需要先经过先序列化为字符串，取值的时候需要经过反序列化，那indexedDB就比较完美的解决了这个问题，可以轻松存取对象或数组等结构化克隆算法支持的任何对象。

以 `stackblitz.com/` 网站为例，我们来看看对象存到 indexedDB 的表现

![1713071440676](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713071440676.png)

### 异步存取

我相信你肯定会思考一个问题：localStorage如果存储内容多的话会消耗内存空间，会导致页面变卡。那么 IndexedDB 存储量过多的话会导致页面变卡吗？

不会有太大影响，因为 IndexedDB 的读取和存储都是异步的，不会阻塞浏览器进程。

### 庞大的存储量

IndexedDB 的储存空间比LocalStorage 大得多，一般可达到500M，甚至没有上限。

But.....关于 indexedDB 的介绍就到此为止，详细使用在此不再赘述，因为本篇文章我重点想介绍的是 localForage！

### 关于存储量

首先indexDB的存储，理论上是硬件有多大内存就可以存多少，但是有些浏览器厂商会限制，具体限制各家不同，但是基本最小是250M起步

## localForage

> localForage 是基于 indexedDB 封装的库，通过它我们可以简化 IndexedDB 的使用。

![1713071480531](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713071480531.png)

### 兼容性

想必你一定很关注兼容性问题吧，我们可以看下 localStorage 与 indexedDB 兼容性比对，两者之间还是有一些小差距。

但是你也不必太过担心，因为 localforage 已经帮你消除了这个心智负担，它有一个优雅降级策略，若浏览器不支持 IndexedDB 或 WebSQL，则使用 localStorage。在所有主流浏览器中都可用：Chrome，Firefox，IE 和 Safari（包括 Safari Mobile）。

### localForage 的使用

1. 下载

```js
//  通过 npm 安装
import localforage from 'localforage'
// 直接引用
<script src="localforage.js"></script>
<script>console.log('localforage is: ', localforage);</script>
```

2. 创建一个 indexedDB

```js
const myIndexedDB = localforage.createInstance({
  name: 'myIndexedDB',
})
```

3. 存值

```
setItem(key, value, successCallback)
```

将数据保存到离线仓库。你可以存储如下类型的 JavaScript 对象：

- **Array**
- **ArrayBuffer**
- **Blob**
- **Float32Array**
- **Float64Array**
- **Int8Array**
- **Int16Array**
- **Int32Array**
- **Number**
- **Object**
- **Uint8Array**
- **Uint8ClampedArray**
- **Uint16Array**
- **Uint32Array**
- **String**

```js
myIndexedDB.setItem(key, value)
```

```js
localforage
  .setItem("somekey", "some value")
  .then(function (value) {
    // 当值被存储后，可执行其他操作
    console.log(value);
  })
  .catch(function (err) {
    // 当出错时，此处代码运行
    console.log(err);
  });

// 不同于 localStorage，你可以存储非字符串类型
localforage
  .setItem("my array", [1, 2, "three"])
  .then(function (value) {
    // 如下输出 `1`
    console.log(value[0]);
  })
  .catch(function (err) {
    // 当出错时，此处代码运行
    console.log(err);
  });

// 你甚至可以存储 AJAX 响应返回的二进制数据
req = new XMLHttpRequest();
req.open("GET", "/photo.jpg", true);
req.responseType = "arraybuffer";

req.addEventListener("readystatechange", function () {
  if (req.readyState === 4) {
    // readyState 完成
    localforage
      .setItem("photo", req.response)
      .then(function (image) {
        // 如下为一个合法的 <img> 标签的 blob URI
        var blob = new Blob([image]);
        var imageURI = window.URL.createObjectURL(blob);
      })
      .catch(function (err) {
        // 当出错时，此处代码运行
        console.log(err);
      });
  }
});
```

4. 取值

```
myIndexedDB.getItem(key, successCallback)
```

从仓库中获取 key 对应的值并将结果提供给回调函数。如果 key 不存在，`getItem()` 将返回 `null`。

由于indexedDB的存取都是异步的，建议使用 promise.then() 或 async/await 去读值

```js
myIndexedDB.getItem('somekey').then(function (value) {
  // we got our value
}).catch(function (err) {
  // we got an error
});

// 回调版本：
localforage.getItem('somekey', function(err, value) {
    // 当离线仓库中的值被载入时，此处代码运行
    console.log(value);
});
```

or

```js
try {
  const value = await myIndexedDB.getItem('somekey');
  // This code runs once the value has been loaded
  // from the offline store.
  console.log(value);
} catch (err) {
  // This code runs if there were any errors.
  console.log(err);
}
```

5. 删除某项

```js
myIndexedDB.removeItem('somekey')
```

```
removeItem(key, successCallback)
```

从离线仓库中删除 key 对应的值。

```js
localforage.removeItem('somekey').then(function() {
    // 当值被移除后，此处代码运行
    console.log('Key is cleared!');
}).catch(function(err) {
    // 当出错时，此处代码运行
    console.log(err);
});
```

6. 重置数据库

```js
myIndexedDB.clear()
```

细节及其他使用方式请参考官方中文文档 `localforage.docschina.org/#localforag…`

## VUE 推荐使用 Pinia 管理 localForage

如果你想使用多个数据库，建议通过 pinia 统一管理所有的数据库，这样数据的流向会更明晰，数据库相关的操作都写在 store 中，让你的数据库更规范化。

```js
// store/indexedDB.ts
import { defineStore } from 'pinia'
import localforage from 'localforage'

export const useIndexedDBStore = defineStore('indexedDB', {
  state: () => ({
    filesDB: localforage.createInstance({
      name: 'filesDB',
    }),
    usersDB: localforage.createInstance({
      name: 'usersDB',
    }),
    responseDB: localforage.createInstance({
      name: 'responseDB',
    }),
  }),
  actions: {
    async setfilesDB(key: string, value: any) {
      this.filesDB.setItem(key, value)
    },
  }
})
```

我们使用的时候，就直接调用 store 中的方法

```js
import { useIndexedDBStore } from '@/store/indexedDB'
const indexedDBStore = useIndexedDBStore()
const file1 = {a: 'hello'}
indexedDBStore.setfilesDB('file1', file1)
```




