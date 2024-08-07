# web缓存

> **web缓存主要指的是两部分：浏览器缓存和http缓存。**

## 浏览器缓存

> 浏览器缓存：比如,localStorage,sessionStorage,cookie等等。这些功能主要用于缓存一些必要的数据，比如用户信息。比如需要携带到后端的参数。亦或者是一些列表数据等等。
>
> **像localStorage，sessionStorage这种用户缓存数据的功能，他只能保存5M左右的数据，多了不行。cookie则更少，大概只能有4kb的数据**。

> **前端新能优化篇之localStorage和sessionStorage的区别及其使用方式 \- 掘金 \(juejin.cn\)**[1]。

### 为什么需要浏览器缓存？

对于浏览器缓存，主要针对的是前端的静态资源，最好的效果是，在发起请求后，拉取相应的静态资源，并保存在本地。如果服务器的静态资源没有更新，那么在下次请求的时候，直接从本地读取资源即可；如果服务器的静态资源已经更新，那么我们再次请求的时候，就要去服务器拉取新的资源，并保存到本地。这样就大大减少了请求次数，提高网站性能。这就用到浏览器的缓存策略了。

总之使用浏览器缓存，有以下优点：

> - 加快客户端网页加载速度，减少用户等待时间，提升用户体验，直接从内存或磁盘中取缓存数据肯定是比从服务器请求更快的。
> - 减少了冗余的数据传输。
> - 减少服务器的负担，大大提升了网站性能。

### Cookie

```js
var cookie = {}; 

/* * @描述 设置cookie * @函数 cookie.set * @参数 */ 

cookie.set = function(key,value,day){ 
    day = day || 1; 
    var date = new Date(); 
    var expires; 
    date.setTime(date.getTime()+day*24*60*60*1000); expires = 'expires='+date.toGMTString(); 

    document.cookie=key+'='+value+';'+expires+';path=/'; 

} 

 

/* * @描述 获取cookie * @函数 cookie.get * @参数 key */ 

cookie.get = function(key){ 
    var cookie = document.cookie; 
    var arr = cookie.split(';'); 
    var obj = {}; 
    var element; 
    for(var i=0;i<arr.length;i++){
        Element = arr[i].split(“=”);
        Obj[element[0]] = element[1];
    }
	return obj[key] ? obj[key] : obj[' '+key];
}


/* * @描述 删除cookie * @函数 cookie.remove * @参数 key 键 */ 

cookie.remove = function(key){ cookie.set(key,cookie.get(key),-1); }
```



### Localstorage （HTML5）

```
setItem(): "增（改）"，将数据以键值对的形式存到 LocalStorage 中

getItem(): "查"，从 LocalStorage 获得对应键值的数据

removeItem(): "删"，删除 LocalStorage 中给定键的数据条目

clear(): 清空 LocalStorage

key(): 传递一个数字 n，获取第 n 个键的数据
```

#### **localStorage 的局限性**

尽管 localStorage 很方便，但它也有一些限制：

- **阻塞 API：**  localStorage 是同步操作的，可能会阻塞主线程，影响应用性能。
- **数据结构有限：**  localStorage 只支持简单的键值对存储，无法处理复杂的数据结构或关系。
- **存储限制：**  每个域的 localStorage 存储容量有限，大约 5MB。
- **缺乏索引:**  localStorage 无法进行高效的查询和迭代操作。
- **标签页阻塞:**  多标签页环境下，一个标签页的 localStorage 操作可能会影响其他标签页的性能。

#### **为什么 localStorage 仍然有价值**

与人们对性能的担忧相反，localStorage 实际上在与 IndexedDB 或 OPFS 等其他存储方案相比时，速度相当快。它尤其擅长处理小型键值对的存储。由于其简洁性和与浏览器的紧密集成，访问和修改 localStorage 数据的开销很小。

对于需要快速且简单数据存储的场景，localStorage 仍然是一个不错的选择，例如：

- RxDB 使用 localStorage 来管理简单的键值对，而将 "正常" 文档存储在 IndexedDB 等其他存储中。

#### **何时不使用 localStorage**

虽然 localStorage 很方便，但并非所有场景都适合它。以下情况，你可能需要考虑其他方案：

- **数据需要查询:**  如果你的应用需要根据特定标准查询数据，localStorage 可能会效率低下。
- **存储大型 JSON 文档:**  localStorage 不适合存储大型 JSON 文档，会导致性能下降。
- **频繁读写操作:**  频繁的读写操作会影响 localStorage 的性能，建议使用其他更适合频繁操作的方案。
- **需要跨会话持久化:**  如果你的应用需要在不同会话间保存数据， localStorage 可能不适合。

#### **其他存储选项**

**IndexedDB**

IndexedDB 专门用于存储 JSON 文档，而不是简单的键值对。它可以处理更大的数据集，并支持索引，方便高效查询。不过，IndexedDB 比 localStorage 稍微复杂一些。

你可以使用像 RxDB 或 Dexie.js 这样的库来简化 IndexedDB 的使用，并提供更多功能。

**文件系统 API (OPFS)**

OPFS 提供了对基于源的、沙盒化的文件系统的直接访问，性能更佳。但 OPFS 使用起来比较复杂，只能在 WebWorker 中使用。

你可以使用 RxDB 的 OPFS RxStorage 库来简化 OPFS 的使用。

**Cookies**

Cookies 曾经是主要的客户端数据存储方式，但现在已经不再流行了，因为它的性能远不如 localStorage。

**WebSQL**

WebSQL 是一种被弃用的技术，不建议使用。

**sessionStorage**

sessionStorage 仅在当前浏览器会话期间保存数据，页面刷新后会丢失。它适合存储一些临时数据。

**React Native 的 AsyncStorage**

React Native 开发者可以使用 AsyncStorage API 进行数据持久化，它类似于 localStorage，但支持异步操作。

**Node.js 的 node-localstorage**

Node.js 中没有内置的 localStorage，可以使用 node-localstorage 包来模拟浏览器中的 localStorage 行为。

**浏览器扩展中的 localStorage**

浏览器扩展可以使用 localStorage API，但最好使用 Extension Storage API 来存储与扩展相关的数据，因为它提供了更多功能和更好的安全性。

**Deno 和 Bun 中的 localStorage**

Deno 支持 localStorage API，而 Bun 不支持。在 Bun 中，可以使用 bun:sqlite 模块或其他 JavaScript 数据库来代替。

#### **选择合适的存储方案**

localStorage 是一种简单高效的存储方案，适合存储少量键值对数据。但对于复杂的应用场景，你需要根据实际情况选择更合适的存储方案。

- **IndexedDB**: 适合存储大量数据，并需要进行高效查询。
- **OPFS**: 适合需要高性能的文件存储。
- **React Native AsyncStorage**: 适合 React Native 应用中的数据持久化。
- **Node.js node-localstorage**: 适合 Node.js 中模拟浏览器 localStorage 行为。
- **浏览器扩展的 Extension Storage API**: 适合存储浏览器扩展相关数据。

### Sessionstorage

```
sessionStorage.setItem('testKey','这是一个测试的value值'); // 存入一个值

sessionStorage['testKey'] = '这是一个测试的value值';

sessionStorage.getItem('testKey'); // => 返回testKey对应的值

sessionStorage['testKey']; // => 这是一个测试的value值
```

> **同localStorage的方法相同**

##### Cookie、localStorage、sessionStorage的区别

```js
一、存储的时间有效期不同

1、cookie的有效期是可以设置的，默认的情况下是关闭浏览器后失效

2、sessionStorage的有效期是仅保持在当前页面，关闭当前会话页或者浏览器后就会失效

3、localStorage的有效期是在不进行手动删除的情况下是一直有效的

二、存储的大小不同

1、cookie的存储是4kb左右，存储量较小，一般页面最多存储20条左右信息

2、localStorage和sessionStorage的存储容量是5Mb(官方介绍，可能和浏览器有部分差异性)

三、与服务端的通信

1、cookie会参与到与服务端的通信中，一般会携带在http请求的头部中，例如一些关键密匙验证等。

2、localStorage和sessionStorage是单纯的前端存储，不参与与服务端的通信
```

##### localStorage和sessionStorage区别

```js
localStorage的数据是持久化的，只要我们不主动清除它，它就会一直存在。
而 sessionStorage 会随着选项卡/窗口的关闭，结束会话并清除存储的数据。

同域下localStorage可以共享数据：
	当然可以。
	// 在一个网页中保存数据
    localStorage.setItem('name', '张三')
    // 从另一个同域的网站中可以直接获取
    localStorage.getItem('name') // 张三

sessionStorage 可以在多个 tab 间共享数据吗？
	根据 mdn 的描述我们可以清楚的看到，打开多个相同的 URL 的 Tabs 页面，会创建各自的 sessionStorage 也就是说 不可共享。
```

![1710579053444](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710579053444.png)

```js
// 我们创建两个 html 文件，分别为 test.html  和  test02.html
// test.html
<body>
  <button id="btn">点我</button>

  <script>
    const btn = document.querySelector('#btn')
    btn.addEventListener('click', () => {
      window.sessionStorage.setItem('name', '张三')
      window.open('http://127.0.0.1:5500/test02.html')
    })
  </script>
</body>
// test02.html
<body>
  <div id="name"></div>

  <script>
    const nameEle = document.querySelector('#name')
    nameEle.innerHTML = window.sessionStorage.getItem('name')
  </script>
</body>

// 在 test 中我们保存了 name 到 sessionStorage 同时打开了 test02.html ，并且 保证他们是同域的，然后再 test02 中，输出了保存的 name（注意： 以上代码需要运行在服务上）。
// 执行以上代码之后，可以发现在 test02 中 成功 的打印出了保存的数据 张三！

分析原因：

那么以上实验证明 sessionStorage 似乎可以共享数据。难道 mdn 上说的是错误的吗？当然不是。

以下是结论：

sessionStorage 确实无法在多个窗口或标签之间共享数据。但是当通过 window.open 或链接打开新页面时，新页面将复制前一页面的 sessionStorage，以此来完成数据共享！
通过a标签新开的页面同样也会，原理相同
```

#### 本地存储二次封装（含加密、解密、过期处理）

> 很多人在用 **localStorage** 或 **sessionStorage** 的时候喜欢直接用，明文存储，直接将信息暴露在；浏览器中，虽然一般场景下都能应付得了且简单粗暴，但特殊需求情况下，比如设置定时功能，就不能实现。就需要对其进行二次封装，为了在使用上增加些安全感，那加密也必然是少不了的了。为方便项目使用，特对常规操作进行封装。

**结构设计**

在封装一系列操作本地存储的API之前，先准备了一个全局对象，对具体的操作进行判断，如下：

```js
interface globalConfig {
  type: 'localStorage' | 'sessionStorage';
  prefix: string;
  expire: number;
  isEncrypt: boolean;
}

const config: globalConfig = {
  type: 'localStorage',              //存储类型，localStorage | sessionStorage
  prefix: 'react-view-ui_0.0.1',     //版本号
  expire: 24 * 60,                   //过期时间，默认为一天，单位为分钟
  isEncrypt: true,                   //支持加密、解密数据处理
};
```

> 1. **type** 表示存储类型，为 **localStorage** 或 **sessionStorage** ；
> 2. **prefix** 表示视图唯一标识，如果配置可在浏览器视图中放在前缀显示；
> 3. **expire** 表示过期时间，默认为一天，单位为分钟；
> 4. **isEncrypt** 表示支持加密、解密数据处理；

**加密准备工作**

```js
// 这里是用到了 crypto-js 来处理加密和解密，可先下载包并导入。
npm i --save-dev crypto-js

import CryptoJS from 'crypto-js';
```

```js
// 对 crypto-js 设置密钥和密钥偏移量,可以采用将一个私钥经 MD5 加密生成16位密钥获得。

const SECRET_KEY = CryptoJS.enc.Utf8.parse('3333e6e143439161'); //十六位十六进制数作为密钥
const SECRET_IV = CryptoJS.enc.Utf8.parse('e3bbe7e3ba84431a'); //十六位十六进制数作为密钥偏移量
```

**加密**

```js
const encrypt = (data: object | string): string => {
  //加密
  if (typeof data === 'object') {
    try {
      data = JSON.stringify(data);
    } catch (e) {
      throw new Error('encrypt error' + e);
    }
  }
  const dataHex = CryptoJS.enc.Utf8.parse(data);
  const encrypted = CryptoJS.AES.encrypt(dataHex, SECRET_KEY, {
    iv: SECRET_IV,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  return encrypted.ciphertext.toString();
};
```

**解密**

```js
const decrypt = (data: string) => {
  //解密
  const encryptedHexStr = CryptoJS.enc.Hex.parse(data);
  const str = CryptoJS.enc.Base64.stringify(encryptedHexStr);
  const decrypt = CryptoJS.AES.decrypt(str, SECRET_KEY, {
    iv: SECRET_IV,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  const decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);
  return decryptedStr.toString();
};
```

```js
# 这两个API都是将获取到的本地存储的value作为参数进行传递，这样就实现了加密和解密。

# 在传入数据进行处理、改变的时候需要进行解密； 在数据需要传出时需要进行加密。
```

**核心API实现**

> **setStorage 设置值**
>
> Storage 本身是不支持过期时间设置的，要支持设置过期时间，可以效仿 Cookie 的做法，setStorage(key, value, expire) 方法，接收三个参数，第三个参数就是设置过期时间的，用相对时间，单位分钟，要对所传参数进行类型检查。可以设置统一的过期时间，也可以对单个值得过期时间进行单独配置。

```js
const setStorage = (key: string, value: any, expire: number = 24 * 60): boolean => {
  //设定值
  if (value === '' || value === null || value === undefined) {
    //空值重置
    value = null;
  }
  if (isNaN(expire) || expire < 0) {
    //过期时间值合理性判断
    throw new Error('Expire must be a number');
  }
  const data = {
    value, //存储值
    time: Date.now(), //存储日期
    expire: Date.now() + 1000 * 60 * expire, //过期时间
  };
  //是否需要加密，判断装载加密数据或原数据
  window[config.type].setItem(
    autoAddPreFix(key),
    config.isEncrypt ? encrypt(JSON.stringify(data)) : JSON.stringify(data),
  );
  return true;
};
```

> **getStorageFromKey 根据key获取value**
>
> 首先要对 key 是否存在进行判断，防止获取不存在的值而报错。对获取方法进一步扩展，只要在有效期内就可以获取 Storage 值，如果过期则直接删除该值，并返回 null。

```js
const getStorageFromKey = (key: string) => {
  //获取指定值
  if (config.prefix) {
    key = autoAddPreFix(key);
  }
  if (!window[config.type].getItem(key)) {
    //不存在判断
    return null;
  }
  const storageVal = config.isEncrypt
    ? JSON.parse(decrypt(window[config.type].getItem(key) as string))
    : JSON.parse(window[config.type].getItem(key) as string);
  const now = Date.now();
  if (now >= storageVal.expire) {
    //过期销毁
    removeStorageFromKey(key);
    return null;
    //不过期回值
  } else {
    return storageVal.value;
  }
};
```

> **getAllStorage 获取所有存储值**

```js
const getAllStorage = () => {
  //获取所有值
  const storageList: any = {};
  const keys = Object.keys(window[config.type]);
  keys.forEach((key) => {
    const value = getStorageFromKey(key);
    if (value !== null) {
      //如果值没有过期，加入到列表中
      storageList[key] = value;
    }
  });
  return storageList;
};
```

> **getStorageLength 获取存储值数量**

```js
const getStorageLength = () => {
  //获取值列表长度
  return window[config.type].length;
};
```

> **removeStorageFromKey 根据key删除存储值**

```js
const removeStorageFromKey = (key: string) => {
  //删除值
  if (config.prefix) {
    key = autoAddPreFix(key);
  }
  window[config.type].removeItem(key);
};
```

> **clearStorage 清空存储列表**

```js
const clearStorage = () => {
  window[config.type].clear();
};
```

> **autoAddPreFix 基于全局配置的prefix参数添加前缀**

```js
const autoAddPreFix = (key: string) => {
  //添加前缀，保持浏览器Application视图唯一性
  const prefix = config.prefix || '';
  return `${prefix}_${key}`;
};
// 这是一个不导出的函数，作为整体封装的内部工具函数，在setStorage、getStorageFromKey、removeStorageFromKey会使用到。
```

> **导出函数列表**
>
> 提供了6个函数的处理能力，足够应对实际业务的大部分操作。

```js
export {
  setStorage,
  getStorageFromKey,
  getAllStorage,
  getStorageLength,
  removeStorageFromKey,
  clearStorage,
};
```

> **使用**
>
> 在实际业务中使用，则将函数导入即可
>
> <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711176508680.png" alt="1711176508680" style="zoom:33%;" />

```js
import {
  setStorage,
  getStorageFromKey,
  getAllStorage,
  getStorageLength,
  removeStorageFromKey,
  clearStorage
} from '../../_util/storage/config'

  setStorage('name', 'fx', 1)
  setStorage('age', { now: 18 }, 100000)
  setStorage('history', [1, 2, 3], 100000)
  console.log(getStorageFromKey('name'))
  removeStorageFromKey('name')
  console.log(getStorageFromKey('name'))
  console.log(getStorageLength());
  console.log(getAllStorage());
  clearStorage();
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711176549599.png" alt="1711176549599" style="zoom:50%;" />

> **完整代码**

```js
// config.ts:

import { encrypt, decrypt } from './encry';
import { globalConfig } from './interface';

const config: globalConfig = {
  type: 'localStorage', //存储类型，localStorage | sessionStorage
  prefix: 'react-view-ui_0.0.1', //版本号
  expire: 24 * 60, //过期时间，默认为一天，单位为分钟
  isEncrypt: true, //支持加密、解密数据处理
};

const setStorage = (key: string, value: any, expire: number = 24 * 60): boolean => {
  //设定值
  if (value === '' || value === null || value === undefined) {
    //空值重置
    value = null;
  }
  if (isNaN(expire) || expire < 0) {
    //过期时间值合理性判断
    throw new Error('Expire must be a number');
  }
  const data = {
    value, //存储值
    time: Date.now(), //存储日期
    expire: Date.now() + 1000 * 60 * expire, //过期时间
  };
  //是否需要加密，判断装载加密数据或原数据
  window[config.type].setItem(
    autoAddPreFix(key),
    config.isEncrypt ? encrypt(JSON.stringify(data)) : JSON.stringify(data),
  );
  return true;
};

const getStorageFromKey = (key: string) => {
  //获取指定值
  if (config.prefix) {
    key = autoAddPreFix(key);
  }
  if (!window[config.type].getItem(key)) {
    //不存在判断
    return null;
  }

  const storageVal = config.isEncrypt
    ? JSON.parse(decrypt(window[config.type].getItem(key) as string))
    : JSON.parse(window[config.type].getItem(key) as string);
  const now = Date.now();
  if (now >= storageVal.expire) {
    //过期销毁
    removeStorageFromKey(key);
    return null;
    //不过期回值
  } else {
    return storageVal.value;
  }
};
const getAllStorage = () => {
  //获取所有值
  const storageList: any = {};
  const keys = Object.keys(window[config.type]);
  keys.forEach((key) => {
    const value = getStorageFromKey(autoRemovePreFix(key));
    if (value !== null) {
      //如果值没有过期，加入到列表中
      storageList[autoRemovePreFix(key)] = value;
    }
  });
  return storageList;
};
const getStorageLength = () => {
  //获取值列表长度
  return window[config.type].length;
};
const removeStorageFromKey = (key: string) => {
  //删除值
  if (config.prefix) {
    key = autoAddPreFix(key);
  }
  window[config.type].removeItem(key);
};
const clearStorage = () => {
  window[config.type].clear();
};
const autoAddPreFix = (key: string) => {
  //添加前缀，保持唯一性
  const prefix = config.prefix || '';
  return `${prefix}_${key}`;
};
const autoRemovePreFix = (key: string) => {
  //删除前缀，进行增删改查
  const lineIndex = config.prefix.length + 1;
  return key.substr(lineIndex);
};

export {
  setStorage,
  getStorageFromKey,
  getAllStorage,
  getStorageLength,
  removeStorageFromKey,
  clearStorage,
}
```

```js
// encry.ts:

import CryptoJS from 'crypto-js';

const SECRET_KEY = CryptoJS.enc.Utf8.parse('3333e6e143439161'); //十六位十六进制数作为密钥
const SECRET_IV = CryptoJS.enc.Utf8.parse('e3bbe7e3ba84431a'); //十六位十六进制数作为密钥偏移量

const encrypt = (data: object | string): string => {
  //加密
  if (typeof data === 'object') {
    try {
      data = JSON.stringify(data);
    } catch (e) {
      throw new Error('encrypt error' + e);
    }
  }
  const dataHex = CryptoJS.enc.Utf8.parse(data);
  const encrypted = CryptoJS.AES.encrypt(dataHex, SECRET_KEY, {
    iv: SECRET_IV,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  return encrypted.ciphertext.toString();
};

const decrypt = (data: string) => {
  //解密
  const encryptedHexStr = CryptoJS.enc.Hex.parse(data);
  const str = CryptoJS.enc.Base64.stringify(encryptedHexStr);
  const decrypt = CryptoJS.AES.decrypt(str, SECRET_KEY, {
    iv: SECRET_IV,
    mode: CryptoJS.mode.CBC,
    padding: CryptoJS.pad.Pkcs7,
  });
  const decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);
  return decryptedStr.toString();
};

export { encrypt, decrypt };
```

```ts
// interface.ts:

interface globalConfig {
  type: 'localStorage' | 'sessionStorage';
  prefix: string;
  expire: number;
  isEncrypt: boolean;
}

export type { globalConfig };

```



### 离线存储（HTML5）

离线存储指的是：在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。

##### (1) 浏览器是怎么对 HTML5 的离线储存资源进行管理和加载的呢？

l 在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问页面 ，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。如果已经访问过页面并且资源已经进行离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，就会重新下载文件中的资源并进行离线存储。

l 离线的情况下，浏览器会直接使用离线存储的资源。

##### (2) **原理**

原理：[HTML5](https://so.csdn.net/so/search?q=HTML5&spm=1001.2101.3001.7020)的离线存储是基于一个新建的 .appcache 文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示

##### (3) **使用方法**

（1）创建一个和 html 同名的 manifest 文件，然后在页面头部加入 manifest 属性：

```
<html lang="en" manifest="index.manifest">
```

（2）在 cache.manifest 文件中编写需要离线存储的资源：

```
CACHE MANIFEST
    \#v0.11
    CACHE:
    js/app.js
    css/style.css
    NETWORK:
    resourse/logo.png
    FALLBACK:
    / /offline.html
```

**l** **CACHE**: 表示需要离线存储的资源列表，由于包含 manifest 文件的页面将被自动离线存储，所以不需要把页面自身也列出来。

**l** **NETWORK**: 表示在它下面列出来的资源只有在在线的情况下才能访问，他们不会被离线存储，所以在离线情况下无法使用这些资源。不过，如果在 CACHE 和 NETWORK 中有一个相同的资源，那么这个资源还是会被离线存储，也就是说 CACHE 的优先级更高。

**l** **FALLBACK**: 表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问 offline.html 。

 

（3）在离线状态时，操作 window.applicationCache 进行离线缓存的操作。

如何更新缓存：

​    （1）更新 manifest 文件

​    （2）通过 javascript 操作

（3）清除浏览器缓存

##### (4) **注意事项**

（1）浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。

（2）若manifest 文件，或者内部列举的某一个文件不能正常下载，整个更新过程都将失败，浏览器继续全部使用老的缓存。

（3）引用 manifest 的 html 必须与 manifest 文件同源，在同一个域下。

（4）FALLBACK 中的资源必须和 manifest 文件同源。

（5）当一个资源被缓存后，该浏览器直接请求这个绝对路径也会访问缓存中的资源。

（6）站点中的其他页面即使没有设置 manifest 属性，请求的资源如果在缓存中也从缓存中访问。

（7）当 manifest 文件发生改变时，资源请求本身也会触发更新

## http缓存

> 官方介绍:Web 缓存是可以自动保存常见文档副本的 HTTP 设备。当 Web 请求抵达缓存时， 如果本地有“已缓存的”副本，就可以从本地存储设备而不是原始服务器中提取这 个文档。

**举个例子↓**

![1710673086596](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673086596.png)

> 看图，问题就是出在，**服务器需要处理http的请求，并且http去传输数据，需要带宽，带宽是要钱买的啊。而我们缓存，就是为了让服务器不去处理这个请求，客户端也可以拿到数据。**

> 注意，我们的缓存主要是针对html,css,img等静态资源，常规情况下，我们不会去缓存一些动态资源，因为缓存动态资源的话，数据的实时性就不会不太好，所以我们一般都只会去缓存一些不太容易被改变的静态资源。

### DNS缓存

这是我们输入网址后，最开始的一个缓存；通常我们输入一个网址，它包含了`域名`和`端口`可以指定唯一的IP地址，然后建立连接进行通信，而域名查找IP地址的过程就是`DNS解析`。

```js
www.baidu.com (域名) - DNS解析 -> 180.76.76.76 (IP地址)
```

这个过程会对网络请求带来一定的损耗，所以浏览器在第一次获取到IP地址后，会将其缓存起来。下次相同域名再次发起请求时，浏览器会先查找本地缓存，如果缓存有效，则会直接返回该IP地址，否则会继续开始寻址之旅。关于寻址过程请看我之前的文章 **浏览器从输入URL到页面渲染加载的过程（浏览器知识体系整理）**[1]

#### 缓存读写顺序

![1709985694174](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709985694174.png)在这里插入图片描述

备注：

memory cache =》浏览器本地缓存

disk cache =》 硬盘缓存

1. 先在 `浏览器缓存` 中查找，如果有，直接加载。
2. 如果 `浏览器缓存` 中不存在，则在 `硬盘` 中查找，这里又细分：

- 如果有强缓存且未失效，则使用强缓存，不请求服务器。
- 如果有强缓存但已失效，使用协商缓存，比较后确定 304 还是 200；

1. 如果硬盘中也不存在，向服务器发起网络请求。
2. 请求获取的资源缓存到硬盘和内存。

下面将从 **缓存位置** 和 **缓存策略** 两个角度介绍浏览器缓存。

#### 缓存位置

##### memory cache（浏览器本地缓存）

是浏览器内存中的缓存，相比于 disk cache 它的特点是读取速度快，但容量小，且时效性短；不受开发者控制，也不受HTTP协议头的约束。一旦浏览器 `tab` 页关闭，`memory cache` 就将被清空，再次重新打开相同页面时不再出现`from memory cache`的情况。

##### disk cache（硬盘缓存）重点！！！

硬盘缓存取决于HTTP中的响应头信息，它也是浏览器缓存中最重要的内容。因为DNS缓存它主要是做一个ip地址查找并且是自主完成的，memory cache 也是不受控制，算是一个黑盒。所以剩下的可以受我们控制的硬盘缓存的重要性就不言而喻了，`大多优化手段也是针对硬盘缓存`。

根据 `HTTP 响应头`的各类字段进行判定资源的缓存规则，比如是否可以缓存，什么时候过期，过期之后是否需要重新发起请求呢？相比于 memory cache 的 disk cache 拥有存储空间时间长等优点。

HTTP所控制下的 disk cache 缓存分为`强缓存`和`协商缓存`。

### 缓存

> 缓存可以解决什么问题。
>
> - **减少不必要的网络传输，节约宽带（就是省钱）**
> - **更快的加载页面（就是加速）**
> - **减少服务器负载，避免服务器过载的情况出现。（就是减载）**

> 缺点
>
> - 占内存（有些缓存会被存到内存中）
>
> 其实日常的开发中，我们最最最最关心的，还是"更快的加载页面";尤其是对于react/vue等SPA（单页面）应用来说，首屏加载是老生常谈的问题。这个时候，缓存就显得非常重要。不需要往后端请求，直接在缓存中读取。速度上，会有显著的提升。是一种提升网站性能与用户体验的有效策略。

根据 `HTTP header` 的字段将缓存分为两个部分，分别是`强缓存`和`协商缓存`。

1. **强缓存**：使用强缓存策略时，如果缓存资源在`过期时间`内，是的话直接从本地缓存中读取资源，不与服务器进行通信。
2. **协商缓存**：如果强缓存失效后，客户端将向服务器发出请求，进行协商缓存。浏览器携带上一次请求返回的响应头中的 **缓存标识** 向服务器发起请求（如ETag、Last-Modified等），由服务器判断资源是否更新。如果`资源没有更新`，则返回状态码 `304` Not Modified，告诉浏览器可以使用本地缓存；否则返回新的资源内容。`强缓存优先级高于协商缓存`，但是协商缓存可以更加灵活地控制缓存的有效性。

强缓存的字段有：`Expires` 和 `Cache-Control`。协商缓存的字段有：`Last-Modified` 和 `ETag`。

- 当`Expires` 和 `Cache-Control` 都被设置的时候，浏览器会优先考虑后者。

- 当 `Last-Modified` 和 `ETag` 都被设置的时候，浏览器会优先考虑后者。

  ![1709985716936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709985716936.png)

![1710673271484](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673271484.png)

#### 强制缓存

> 强制缓存，我们简称强缓存。
>
> 从强制缓存的角度触发，如果浏览器判断请求的目标资源有效命中强缓存，如果命中，则可以直接从内存中读取目标资源，无需与服务器做任何通讯。

##### 基于Expires字段实现的强缓存

> 在以前，我们通常会使用响应头的`Expires`字段去实现强缓存。如下图↓

![1710673357095](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673357095.png)

> `Expires`字段的作用是，设定一个强缓存时间。在此时间范围内，则从内存（或磁盘）中读取缓存返回。

```
比如说将某一资源设置响应头为:Expires:new Date("2022-7-30 23:59:59")；

那么，该资源在2022-7-30 23:59:59 之前，都会去本地的磁盘（或内存）中读取，不会去服务器请求。

但是，**Expires已经被废弃了**。对于强缓存来说，Expires已经不是实现强缓存的首选。
```

> **因为Expires判断强缓存是否过期的机制是:获取本地时间戳，并对先前拿到的资源文件中的****Expires字段的时间做比较。来判断是否需要对服务器发起请求。这里有一个巨大的漏洞：“如果我本地时间不准咋办？”**
>
> **是的，****Expires过度依赖本地时间，如果本地与服务器时间不同步，就会出现资源无法被缓存或者资源永远被缓存的情况。所以，Expires字段几乎不被使用了。现在的项目中，我们并不推荐使用Expires，强缓存功能通常使用cache-control字段来代替Expires字段。**

##### 基于Cache-control实现的强缓存（代替Expires的强缓存实现方法）

> `Cache-control`这个字段在http1.1中被增加，`Cache-control`完美解决了`Expires`本地时间和服务器时间不同步的问题。是当下的项目中实现强缓存的最常规方法。
>
> `Cache-control`的使用方法页很简单，只要在资源的响应头上写上需要缓存多久就好了，单位是秒。比如↓

```js
//往响应头中写入需要缓存的时间
res.writeHead(200,{
    'Cache-Control':'max-age=10'
});
```

下图的意思就是，从该资源第一次返回的时候开始，往后的10秒钟内如果该资源被再次请求，则从缓存中读取。

![1710673495061](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673495061.png)

> **Cache-Control:max-age=N，N就是需要缓存的秒数。从第一次请求资源的时候开始，往后N秒内，资源若再次请求，则直接从磁盘（或内存中读取），不与服务器做任何交互。**
>
> `Cache-control`中因为max-age后面的值是一个滑动时间，从服务器第一次返回该资源时开始倒计时。所以也就不需要比对客户端和服务端的时间，解决了`Expires`所存在的巨大漏洞。
>
> `Cache-control`有**max-age**、**s-maxage**、**no-cache**、**no-store**、**private**、**public**这六个属性。
>
> - **max-age**决定客户端资源被缓存多久。
> - **s-maxage**决定代理服务器缓存的时长。
> - **no-cache**表示是强制进行协商缓存。
> - **no-store**是表示禁止任何缓存策略。
> - **public**表示资源即可以被浏览器缓存也可以被代理服务器缓存。
> - **private**表示资源只能被浏览器缓存。

###### no-cache和no-store

> **no_cache**是`Cache-control`的一个属性。它并不像字面意思一样禁止缓存，实际上，**no-cache**的意思是强制进行协商缓存。如果某一资源的`Cache-control`中设置了**no-cache**，那么该资源会直接跳过强缓存的校验，直接去服务器进行协商缓存。而**no-store**就是禁止所有的缓存策略了。

> 注意，no-cache和no-store是一组互斥属性，这两个属性不能同时出现在`Cache-Control`中。

###### public和private

一般请求是从客户端直接发送到服务端，如下↓

![1710673584705](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673584705.png)

但有些情况下是例外的：比如，出现代理服务器，如下↓

![1710673601978](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673601978.png)

> 而**public**和**private**就是决定资源是否可以在代理服务器进行缓存的属性。
>
> 其中，**public**表示资源在客户端和代理服务器都可以被缓存。
>
> **private**则表示资源只能在客户端被缓存，拒绝资源在代理服务器缓存。
>
> 如果这两个属性值都没有被设置，则默认为**private**

> 注意，**public**和**private**也是一组互斥属性。他们两个不能同时出现在响应头的`cache-control`字段中。

###### max-age和s-maxage

> **max-age**表示的时间资源在客户端缓存的时长，而**s-maxage**表示的是资源在代理服务器可以缓存的时长。
>
> 在一般的项目架构中**max-age**就够用。
>
> 而**s-maxage**因为是代理服务端的缓存时长，他必须和上面说的**public**属性一起使用（public属性表示资源可以在代理服务器中缓存）。

> 注意，**max-age**和**s-maxage**并不互斥。他们可以一起使用。

> 那么,Cache-control如何设置多个值呢？用逗号分割，如下↓
>
> ```
> Cache-control:max-age=10000,s-maxage=200000,public
> ```
>
> **强制缓存就是以上这两种方法了。现在我们回过头来聊聊，****Expires难道就一点用都没有了吗？也不是，虽然Cache-control是Expires的完全替代品，但是如果要考虑向下兼容的话，在Cache-control不支持的时候，还是要使用Expires，这也是我们当前使用的这个属性的唯一理由。**

#### 协商缓存

其实响应头和请求头的对应关系就是 `Last-Modify/If-Modify-Since` 和 `ETag/If-None-Match`。

之所以要发两个信息，是为了兼容不同的服务器，因为有些服务器只认`If-Modified-Since`，有些服务器只认`If-None-Match`，有些服务器两个都认,但是一般来说 `If-None-Match` 的优先级高于 `If-Modified-Since`

###### Last-Modify / If-Modify-Since

浏览器第一次请求一个资源的时候，服务器返回的 header 中会加上 `Last-Modify`，`Last-Modify`是一个时间标识该资源的最后修改时间。当浏览器再次请求该资源时，请求头中会包含 `If-Modify-Since`，该值为缓存之前返回的 `Last-Modify`。服务器收到 `If-Modify-Since` 后，根据资源的最后修改时间判断资源是否更新。

- 资源未更新，返回304重定向，表示资源未更新可以继续使用缓存中的资源。
- 资源更新，返回200状态码，返回新的资源，并进行硬盘和浏览器缓存。

**缺点**：如果资源更新的速度是小于 1 秒的，那么该字段将失效，因为 `Last-Modified` 时间是精确到秒的。所以有了 `ETag`。

###### ETag / If-None-Match

与 Last-Modify/If-Modify-Since 不同的是，Etag/If-None-Match 返回的是一个校验码。`ETag` 可以保证每一个资源是唯一的，资源变化都会导致 ETag 变化。服务器根据浏览器上发送的 `If-None-Match` 值来判断是否缓存。与 Last-Modified 不一样的是，当服务器返回 304 Not Modified 的响应时，由于 ETag 重新生成过，response header 中还会把这个 ETag 返回，即使这个 ETag 跟之前的没有变化。

##### 基于last-modified的协商缓存

> 基于last-modified的协商缓存实现方式是:
>
> 1. 首先需要在服务器端读出文件修改时间，
> 2. 将读出来的修改时间赋给响应头的`last-modified`字段。
> 3. 最后设置`Cache-control:no-cache`
>
> 三步缺一不可。

![1710673752657](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673752657.png)

> 第一行，读出修改时间。
>
> 第二行，给该资源响应头的`last-modified`字段赋值修改时间
>
> 第三行，给该资源响应头的`Cache-Control`字段值设置为:no-cache.(上文有介绍，Cache-control:no-cache的意思是跳过强缓存校验，直接进行协商缓存。)

> **还没完。到这里还无法实现协商缓存**

当客户端读取到`last-modified`的时候，会在下次的请求标头中携带一个字段:`If-Modified-Since`。

![1710673820556](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673820556.png)

而这个请求头中的`If-Modified-Since`就是服务器第一次修改时候给他的时间，也就是上图中的

![1710673844212](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673844212.png)

> **那么之后每次对该资源的请求，都会带上**If-Modified-Since这个字段，而务端就需要拿到这个时间并再次读取该资源的修改时间，让他们两个做一个比对来决定是读取缓存还是返回新的资源。

![1710673884749](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673884749.png)

这样，就是协商缓存的所有操作了。

![1710673913629](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710673913629.png)

> **使用以上方式的协商缓存已经存在两个非常明显的漏洞。这两个漏洞都是基于文件是通过比较修改时间来判断是否更改而产生的。**
>
> **1.因为是更具文件修改时间来判断的，所以，在文件内容本身不修改的情况下，依然有可能更新文件修改时间（比如修改文件名再改回来），这样，就有可能文件内容明明没有修改，但是缓存依然失效了。**
>
> **2.当文件在极短时间内完成修改的时候（比如几百毫秒）。因为文件修改时间记录的最小单位是秒，所以，如果文件在几百毫秒内完成修改的话，文件修改时间不会改变，这样，即使文件内容修改了，依然不会 返回新的文件。**
>
> **为了解决上述的这两个问题。从****http1.1开始新增了一个头信息，ETag(Entity 实体标签)**

##### 基础ETag的协商缓存

> `ETag`就是将原先协商缓存的比较**时间戳**的形式修改成了比较**文件指纹**。
>
> 文件指纹:根据文件内容计算出的唯一哈希值。文件内容一旦改变则指纹改变。

###### 流程

```js
1.第一次请求某资源的时候，服务端读取文件并计算出文件指纹，将文件指纹放在响应头的etag字段中跟资源一起返回给客户端。

2.第二次请求某资源的时候，客户端自动从缓存中读取出上一次服务端返回的ETag也就是文件指纹。并赋给请求头的if-None-Match字段，让上一次的文件指纹跟随请求一起回到服务端。

3.服务端拿到请求头中的is-None-Match字段值（也就是上一次的文件指纹），并再次读取目标资源并生成文件指纹，两个指纹做对比。如果两个文件指纹完全吻合，说明文件没有被改变，则直接返回304状态码和一个空的响应体并return。如果两个文件指纹不吻合，则说明文件被更改，那么将新的文件指纹重新存储到响应头的ETag中并返回给客户端
```

![1710674016187](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710674016187.png)

> 从校验流程上来说，协商缓存的修改时间比对和文件指纹比对，几乎是一样的。

> ### ETag也有缺点
>
> - **ETag需要计算文件指纹这样意味着，服务端需要更多的计算开销。。如果文件尺寸大，数量多，并且计算频繁，那么ETag的计算就会影响服务器的性能。显然，ETag在这样的场景下就不是很适合。**
> - **ETag有强验证和弱验证，所谓将强验证，ETag生成的哈希码深入到每个字节。哪怕文件中只有一个字节改变了，也会生成不同的哈希值，它可以保证文件内容绝对的不变。但是，强验证非常消耗计算量。ETag还有一个弱验证，弱验证是提取文件的部分属性来生成哈希值。因为不必精确到每个字节，所以他的整体速度会比强验证快，但是准确率不高。会降低协商缓存的有效性。**

> 值得注意的一点是，不同于`cache-control`是`expires`的完全替代方案(说人话:能用`cache-control`就不要用`expiress`)。`ETag`并不是`last-modified`的完全替代方案。而是`last-modified`的补充方案（说人话：项目中到底是用`ETag`还是`last-modified`完全取决于业务场景，这两个没有谁更好谁更坏）。

#### 哪些文件对应哪些缓存

> 有哈希值的文件设置强缓存即可。没有哈希值的文件（比如index.html）设置协商缓存

**为什么有哈希值的文件设置强缓存**

![1710674146506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710674146506.png)

![1710674156318](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710674156318.png)

#### 总结一下

> - http缓存可以减少宽带流量，加快响应速度。
> - 关于强缓存，`cache-control`是`Expires`的完全替代方案，在可以使用`cache-control`的情况下不要使用`expires`
> - 关于协商缓存,`etag`并不是`last-modified`的完全替代方案，而是补充方案，具体用哪一个，取决于业务场景。
> - 有些缓存是从磁盘读取，有些缓存是从内存读取，有什么区别？答：从内存读取的缓存更快。
> - 所有带304的资源都是协商缓存，所有标注（从内存中读取/从磁盘中读取）的资源都是强缓存。

## 不需要缓存的时候

并不是所有请求都能被缓存，无法被浏览器缓存的请求如下：

- HTTP 信息头中包含 `Cache-Control: no-cache` ，`pragma: no-cache（HTTP1.0）`，或 `Cache-Control: max-age=0` 等告诉浏览器不用缓存的请求；
- 需要根据 `Cookie`、认证信息等决定输入内容的动态请求是不能被缓存的；
- 经过 `HTTPS` 安全加密的请求；
- `POST` 请求无法被缓存；
- HTTP 响应头中不包含 `Last-Modified/Etag`，也不包含 `Cache-Control/Expires` 的请求无法被缓存；

本文参考

**一文读懂浏览器缓存**[2]

**实践这一次,彻底搞懂浏览器缓存机制**[3]

**浏览器缓存缓存策略（看完就懂）**[4]

