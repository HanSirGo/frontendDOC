### axios基础
#### 1. axios
axios 是一个基于 Promise 的 HTTP 库，可以用在浏览器和 node.js 中。在Web端本质上就是Ajax + Promise，服务端是htttp + Promise
**Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。**

> **axios是一个函数对象，但是文档并没有表明new axios可以产生新的axios实例（如果你细心一点，可以注意到axios的原型prototype几乎是一个空对象），而是通过**
> ![1713767763543](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767763543.png)
#### 2. 与fetch 对比
可以了解fetch
#### 3. axios 为什么需要 create
axios是一个实例对象，axios.create相当于是这类对象的“构造函数”，每个实例应该保存内部属性，当调用方法时通过传入配置参数，将外部属性和实例内部属性合并，最后发送请求

因此，对axios来说是有很多内部属性都可以配置的，但是一旦改变axios属性，意味着以后所有的通过axios实例发出的请求，都会将外部配置和当前axios实例内部配置合并，这是非常可怕的。比如

> //假设最开始，由于服务器1网速比较快，连接时间就可以比较短，于是你设置axios内部属性timeout 为 3000
> axios.defaults.timeout = 3000 
> //但是由于新增业务，你要的服务器2网速比较慢，连接时间就可以比较长，于是你设置axios内部属性timeout 为 5000
> axios.defaults.timeout = 5000
> //但是现在你又发现，如果需要再次请求服务器1，超时时间也是5000毫秒，于是直接凌乱，不知所措

虽然axios每次发送请求都可以再次传入实时的外部配置，但是一旦请求多起来，你需要的外部配置就会越来越复杂

因此，能够生产另外一个新的axios实例对象是非常重要的，axios.create解决了这一问题

```javascript
const axiosTimeOutSlow = axios.create({ //慢连接
	timeout: 5000
})
const axiosTimeOutFast = axios.create({//快连接
	timeout: 3000
})

```
但是还有一个问题，新建的axios实例真的是和原来的axios实例几乎一样吗？不是的！axios.create(config).prototype =>
![1713767781189](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767781189.png)

#### 4. axios实例的方法
```javascript
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```
##### config配置选项包括：
这些是创建请求时可以用的配置选项。只有 url 是必需的。**如果没有指定 method，请求将默认使用 get 方法。**
```javascript
{
   // `url` 是用于请求的服务器 URL
  url: '/user',

  // `method` 是创建请求时使用的方法
  method: 'get', // default

  // `baseURL` 将自动加在 `url` 前面，除非 `url` 是一个绝对 URL。
  // 它可以通过设置一个 `baseURL` 便于为 axios 实例的方法传递相对 URL
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` 允许在向服务器发送前，修改请求数据
  // 只能用在 'PUT', 'POST' 和 'PATCH' 这几个请求方法
  // 后面数组中的函数必须返回一个字符串，或 ArrayBuffer，或 Stream
  transformRequest: [function (data, headers) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
  transformResponse: [function (data) {
    // 对 data 进行任意转换处理
    return data;
  }],

  // `headers` 是即将被发送的自定义请求头
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 是即将与请求一起发送的 URL 参数
  // 必须是一个无格式对象(plain object)或 URLSearchParams 对象
  params: {
    ID: 12345
  },

  // `paramsSerializer` 是一个负责 `params` 序列化的函数
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'repeat'})
  },
  // 上边适合vue2，下边适合vue3
  // paramsSerializer: {
  //    serialize: (params) => {
  //  		return Qs.stringify(params, {arrayFormat: 'repeat'})
  //    }
  // },  
      
  // `data` 是作为请求主体被发送的数据
  // 只适用于这些请求方法 'PUT', 'POST', 和 'PATCH'
  // 在没有设置 `transformRequest` 时，必须是以下类型之一：
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - 浏览器专属：FormData, File, Blob
  // - Node 专属： Stream
  data: {
    firstName: 'Fred'
  },

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  // 'json': 默认，服务器响应会被解析成JavaScript对象
  // 'arraybuffer': 服务器响应会被解析成一个ArrayBuffer对象，适合处理二进制数据，如文件下载
  // 'text': 服务器响应会被解析成字符串
  // 'document': 服务器响应会被解析成一个HTML文档
  // 'stream': (仅限Node.js环境) 服务器响应会被处理成一个Stream对象，可以用来处理大文件或者边下载边处理的场景
  // 'blob': (仅限浏览器环境) 服务器响应会被解析成一个Blob对象，适合文件处理，尤其是大文件或者需要存储到本地的情况
  responseType: 'json', // default

  // `responseEncoding` 表示用于解码响应的编码
  // 注意：忽略“stream”或客户端请求的“responseType”
  responseEncoding: 'utf8', // default

   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` 是带有 xsrf 令牌值的 http 标头的名称
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // 使用本机进度事件做任何你想做的事
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // `socketPath` 定义了一个在 node.js 中使用的 UNIX Socket。
  // 例如 '/var/run/docker.sock' 向 docker 守护进程发送请求。
  // 只能指定 `socketPath` 或 `proxy`。
  // 如果两者都指定，则使用 `socketPath`。
  socketPath: null, // default

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```
##### 响应结构
某个请求的响应包含以下信息
```javascript
{
  // `data` 由服务器提供的响应
  data: {},

  // `status` 来自服务器响应的 HTTP 状态码
  status: 200,

  // `statusText` 来自服务器响应的 HTTP 状态信息
  statusText: 'OK',

  // `headers` 服务器响应的头
  headers: {},

   // `config` 是为请求提供的配置信息
  config: {},
 // 'request'
  // `request` 是生成此响应的请求
  // 它是 node.js 中的最后一个 ClientRequest 实例（在重定向中）
  // 和浏览器的 XMLHttpRequest 实例
  request: {}
}
```
#### 5. 拦截器
**在请求或响应被 then 或 catch 处理前拦截它们**
```javascript
// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
  }, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  });

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
  }, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  });
```
如果你想在稍后移除拦截器，可以这样：
```javascript
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```
可以为自定义 axios 实例添加拦截器
```javascript
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```
**可以使用 validateStatus 配置选项定义一个自定义 HTTP 状态码的错误范围。**
```javascript
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // 仅当状态码大于等于 500 时才拒绝
  }
})
```
#### 6. 取消
**使用 cancel token 取消请求**

1. **可以使用 CancelToken.source 工厂方法创建 cancel token**

```javascript
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
     // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```
2. **还可以通过传递一个 executor 函数到 CancelToken 的构造函数来创建 cancel token：**

```javascript
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// cancel the request
cancel();
```

> **注意: 可以使用同一个 cancel token 取消多个请求**

3. **在拦截器中取消重复请求**

```js
const pending = [] // 声明一个存储 接口请求的取消函数和标识
const removePending = (config) => {
    for (const p in pending) {
        if(pending[p].u === config.url+'&'+config.method)
        // 当前请求在数组中存在执行函数
        pending[p].f() //取消执行
        pending.splice(p,1) // 并从pending中删除掉这条记录
    }
}
axios.interceptors.request.use(
	(config)=>{
        //加token
        config.headers.Authorization = `Bearer xxx`
        // 重复点击取消前一次请求
        removePending(config)
        config.cancelToken = new axios.CancelToken((c)=>{
            pending.push({u:config.url+'&'+config.method,f:c})
        })
    }
)
```

