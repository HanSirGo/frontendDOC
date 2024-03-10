#### Axios

##### (1) Axios

```
get:查 

axios.get(url, 后台参数) 

后台参数 = {params:{key:value}}  key 是后台字段 

post:增 

axios.post(url,后台参数) 

后台参数 = {key:value} key 是后台字段  

delete:删 执行完会报404 

axios.delete(url/id,后台参数）与get的后台参数一致 url后加 /id 

put:改 

axios.put(url/id,后台参数）与post的后台参数一致 url后加 /id 
```

##### (2) axios实例

```js
const instance = axios.create({ 
  baseURL: 'https://some-domain.com/api/', 
  timeout: 1000, 
  headers: {'X-Custom-Header': 'foobar'} 
}); 

instance 实例 
baseURL 基准地址 （域） 
timeout  请求时间 
headers: 请求头 信任令牌 
http://localhost:3000/products 

域(协议 域名 端口号)  /(path) ?xxx(search 没问号 query) #xxx(hash) 

axios.create() 创建一个实例 这个实例的api 与axios 一样 （不一样的是 他的参数） 

项目中 后台有三种接口 生产 测试 开发，他们的域是不一样的，在项目中使用的路径有几百上千个，一个个改，费时费力，可以用axios.create() 
```

##### (3) **Axios.all()**

```
并发请求；同时发送多个请求。 

let a1 = axios.get('http://localhost:3000/products') 

let a2 = axios.get('http://localhost:3000/cars') 

axios.all([a1,a2]) 

.then(res=>console.log(res)) 

只有所有请求都成功，才返回请求结果 

面试题：怎样解决并发请求中有错误请求，但是成功的依然返回请求结果？
```

##### (4) **Axios拦截器**

```js
// 添加请求拦截器 

axios.interceptors.request.use(function (config) { 
    // 在发送请求之前做些什么 
    return config; 
  }, function (error) { 
    // 对请求错误做些什么 
    return Promise.reject(error); 
  }); 
```

```js
// 添加响应拦截器 

axios.interceptors.response.use(function (response) { 
    // 对响应数据做点什么 
    return response; 
  }, function (error) { 
    // 对响应错误做点什么 
    return Promise.reject(error); 
  }); 
```

```js
想在稍后移除拦截器，可以这样：  

 const myInterceptor = axios.interceptors.request.use(function () {/*...*/}); 

 axios.interceptors.request.eject(myInterceptor); 

可以为自定义 axios 实例添加拦截器  

const instance = axios.create(); 

instance.interceptors.request.use(function () {/*...*/}); 
```

axios实例请求拦截---------例子 

```js
import axios from 'axios' 

const instance = axios.create({ 
  baseURL:'http://localhost:3000', 
  timeout:3000, 
  name:'zs', 
  hearders:{} 
}) 

// 请求拦截 

instance.interceptors.request.use(function(config){ 
  console.log(config); 
  config.name='lisi' 
  return config 
}) 
```

##### (5) **Axios全局使用**

```js
1.结合 vue-axios使用 

     首先在主入口文件main.js中引用  
        import axios from 'axios' 
        import VueAxios from 'vue-axios'     
        Vue.use(VueAxios,axios); 

     之后就可以使用了，在组件文件中的methods里去使用了： this.axios.get() 

2.axios 改写为 Vue 的原型属性（不推荐这样用） 
     首先在主入口文件main.js中引用，之后挂在vue的原型链上 Vue.prototype.$ajax= axios 
     在组件中使用  this.$ajax.get() 

3.结合vuex 中的 action 
     在vuex的仓库文件store.js中引用，使用action添加方法  
        import Vue from 'Vue' 
        import Vuex from 'vuex'   
        import axios from 'axios'        
        Vue.use(Vuex) 
        const store = new Vuex.Store({ 
          // 定义状态 
          state: { 
            user: {  name: 'xiaoming'  } 
          }, 
          actions: { // 封装一个 ajax 方法 
            login (context) { 
              axios({ 
                method: 'post', 
                url: '/user', 
                data: context.state.user 
              }) 
            } 
          } 
        }) 
         
        export default store 
    在组件中发送请求：this.$store.dispatch 
    this.$store.dispatch('login') 
```

##### (6) **Axios请求数据的方式**

```js
请求方式： 

查get 请求的参数 在params   http://localhost:3000/user?id=0 

增post 的请求参数在 body中 （请求体中） 
    参数类型：application/json  == "{"name":"list"}" 
            application/x-www-form-urlencoded =="name:list" 
            后台第二，前台第一 会报501 
            application/form-data 用于文件上传 

改put 更新数据，参数类型跟post 一样 http://localhost:3000/user/3   把要修改的数据 填到表里 

删delete 删除数据 ，参数类型跟 get一样 http://localhost:3000/user/3  

请求头 header {'content-type':'application/json'} 

需要前台向后台 发送的参数是 json格式 

header {'content-type':'application/x-www-form-urlencoded'} 

需要前台向后台 发送的参数是 key=value 格式 


如何将json格式 转为 ‘key=value’格式 

npm i qs 
import qs from 'qs'   // 或者使用commonjs引入   const qs = require('qs')  
axios.post(url,qs.stringify({"name":"zs"}))  
```

## 

## 引言

**不从需求出发的二次封装都是技术流氓**

> axios的出现是为了帮助前端开发者更加方便的完成前后端数据交互，不同的业务需求对axios做二次封装的设计是不同的。本篇文章主要讲一讲各场景下的二次封装实现思路。
>
> **不从需求出发的二次封装都是技术流氓** 不做为了封装而封装的事情、不做为了装杯而封装的事情

Axios 是基于 Ajax 的第三方库，它诞生的初衷就是为了更加方便的实现前后端数据交互。但 Axios 并不是直接继承自传统的 XMLHttpRequest（XHR）对象，而是基于 Promise 的一个现代库，它对 XHR 进行了封装，并提供了更简洁的 API 以及额外的功能。

二次封装的目的是基于它提供的简洁的API用法，进一步针对性的把业务中通用的部分内置进去，减少后期的代码量，降低维护成本。

## 通用的基本配置

**这一部分都不能算作是封装，只是axios的基本使用。**

axios提供了许多用于请求的方法，这里不一一列出，就以`post()`和`get()`为例。

如下图，我准备了三个接口，这三个接口啥事不做就只是为了前端请求用。

![1709986103025](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986103025.png)

前端请求接口如果直接使用axios提供的`get()`、`post()`方法，那势必如下面这样

```js
import axios from 'axios'
const request1=()=>{
  axios.get('http://192.168.3.91:3000/course/list')
    .then(res => {

    })
}
const request2=()=>{
  axios.get('http://192.168.3.91:3000/model/list')
    .then(res => {
      
    })
}
const request3=()=>{
  axios.post('http://192.168.3.91:3000/course/create',{
    name:'书苑书斋'
  })
    .then(res => {
      
    })
}
```

这样直接调用axios提供的请求方法唯一的好处就是简单明了，但是通常在实际项目中不会直接这样使用。在**不修改axios默认配置**的情况下，它的缺点是很明显的：

- 每次都要写完整的服务地址，基础服务地址无法公用
- 无法统一配置超时时间，统一设置请求头等
- 无法配置请求、响应拦截，对返回值做统一处理

> 当然你可以通过 `axios.defaults.baseURL`修改默认配置，但这依然不是推荐的方案

### 创建实例配置全局

**axios**还提供了一个`create()`方法用来创建一个axios实例，创建实例的方式在项目中是普遍使用，也是推荐使用的。在创建实例的过程中允许传入一个`config`配置项，用于设置**服务基础地址**、**超时时间**、**请求头**、**跨域凭证**等。

```js
export const request = axios.create({
  baseURL:'http://192.168.3.91:3000',   // 基础服务地址，这个值会被添加到实际请求 URL 的前面
  timeout:5000, // 超时时间 单位是毫秒
  headers:{ // 请求头设置 可以设置全局或特定的headers
    'Content-Type':'application/json;charset=utf-8'
  },
  withCredentials:true, //是否携带 cookie 发送跨域请求。默认为 false
})
```

创建axios实例的配置常用的就以上四个，还有其它的都不常用。

这样就可以用request实例调用请求方法了，用这个request实例调用的请求方法都会继承使用创建实例时的配置项，后需要修改基础服务地址或其他都只需更改一个地方即可。

```js
request.get('/course/list').then(res=>{
    console.log(res)
})
```

### 为什么需要配置响应拦截器

发起请求，此时观察控制台输出的内容你会看到控制台输出的内容可能并不是你希望的。

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986173350.png)它返回的对象中包含了如上许多属性对象，但其实在业务代码中我们只关注后端响应回来的**data**数据，并不需要关心发送请求时使用的`config`配置对象是什么，并且对`status` HTTP状态码不为200的也会做统一的异常处理。**因此，需要设置响应拦截器**

最简单的响应拦截，从response中取出业务代码需要的data并且返回

```js
request.interceptors.response.use(
  response => {
    return response.data
  }
)
```

那么此时在控制台输出打印的就一定是**data**的属性值了。

**请求异常，统一处理** 响应拦截器还接收第二个回调参数，在请求发生异常时会执行这个回调参数。通常会在这个异常回调中根据Http状态码统一处理异常时的逻辑。

**举例**现在将两个接口的返回状态码分别设置成`401`和`403`，服务端接口如下图：

![1709986215363](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986215363.png)然后分别请求这两个接口，我们希望的是当请求异常时，在响应拦截器中就直接做出错误提示了。

这一部分的统一处理就在相应拦截中统一做，代码如下所示

```js
request.interceptors.response.use(
  response => {
    console.log(response)
    return response.data
  },
  error=>{
    let message = ''
    if (error && error.response) {
      switch (error.response.status) {
        case 401:
          message = '未登录，请先登录'
          break;
        case 403:
          message = '您没有权限操作！'
          break;
      }
    }
    ElMessage({ type: 'error', message, duration:1000 })
  }
)
```

### 请求拦截其中的身份令牌

前后端数据交互，通常需要权限的接口都要求前端在发起请求的时候在请求头上携带一个token去请求后端接口，后端拿到token就可以校验用户身份。

在请求拦截其中最基本的事情就是把token设置到请求头headers上，比如:

```js
const token = sessionStorage.getItem('token')
token && (config.headers.Authorization = token)
```

### 请求拦截中的参数加密

**参数加密**通常为了安全性考虑请求的参数也不是明文请求，你打开控制台也无法查看请求参数是什么，这样一定程度上防止了黑客通过请求参数分析你的接口，如下图：

![1709986264496](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986264496.png)参数加密.png

前端发送给后端的是这样一串经过加密算法加密的字符串，后端会用同一种算法对这串字符进行解密。

参数加密这一步也是在请求拦截器中完成，**例如**请求接口，对参数进行加密，后端接收到解密后直接返回解密后的数据，展示在界面上，效果如下图：

![1709986283710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986283710.png)实现参数加密，只需要在请求拦截器中对data参数重写即可

```js
request.interceptors.request.use(
  config => {
    config.data = { data: encrypt(JSON.stringify(config.data)) }
    return config
  }
)
```

## axios进阶使用

以上是一个前端开发者必须掌握的axios基础技能，只是axios的基本配置使用。

如果项目中有部分请求需要参数加密，有部分请求不需要参数加密。再比如经常困扰前端的一个点击按钮重复提交的问题等，这些都可以在上面的基础上进行扩展解决。

首先在上面基础上可以做一点点修改，不直接在代码中使用`request.get('/course/list')`的这种方式发送请求。而是将所有的请求用一个单独的api文件来维护，比如像这样：

![1709986318370](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986318370.png)那么在页面代码中就可以直接调用对应的接口方法

```js
getCourseList().then(res => {
    console.log(res)
})
```

### 自定义参数

这里的自定义参数指的是配置像是参数是否加密、如何处理响应结果等。这些自定义的参数完全是根据你自己的项目去制定的，我这里以参数是否加密为例。

已知我们已经创建了axios实例，也可以在请求头中处理参数加密，但是所有使用这个axios实例的请求都会对参数进行加密。我们需要一个参数来控制，那可以创建一个函数，在函数中返回这个axios实例，同时这个函数接收自定义参数。

如下：只需要加上一条`if`语句控制即可

```js
function createInstance(axiosConfig,customConfig){
  const request = axios.create({
    baseURL:'http://192.168.3.91:3000',   // 基础服务地址，这个值会被添加到实际请求 URL 的前面
    timeout:60 * 1000 * 2, // 超时时间 单位是毫秒
    headers:{ // 请求头设置 可以设置全局或特定的headers
      'Content-Type':'application/json;charset=utf-8'
    }
  })
  const customOpt = Object.assign(
    {
      isCrypto: false, // 是否开启加密，默认true
    },
    customConfig
  )
  request.interceptors.request.use(
    config => {
      if(customOpt.isCrypto){
        config.data = { data: encrypt(JSON.stringify(config.data)) }
      }
      return config
    }
  )
  // 请求拦截 响应拦截省略
  ...
  return request(axiosConfig)
}
```

更多自定义参数可自行在`cursomOpt`配置对象中添加默认值。

### 防止重复提交

我们不希望用户暴力点击，不希望在前要给请求的结果还没有返回，又不断的发送同一个请求。我们希望同一个请求，只有在上一次请求结果回来之后才能再次发送。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986359260.png)多点.gif

**思考：要解决暴力点击的问题，你觉得使用防抖的思路可以吗？**

这里我们从axios入手，思路很简单：创建一个集合，当请求进来之后先在集合中找这个请求存不存在，如果存在则代表请求已经发送了，则不执行发送请求的操作；如果不存在则先把这个请求塞到集合中，继续发送请求操作，只有当请求的结果返回才把这个请求从集合中删除掉（**不管结果是成功还是异常都要删除**）

```js
const server = (()=> {
  let history = []
  return function(config){
    const { url } = config
    if(history.includes(url)){
      return Promise.reject({ msg:'请求已提交' })
    }
    history.push(url)
    return request({
      ...config
    }).finally(()=>{
      const idx = history.indexOf(url)
      history.splice(idx,1)
    })
  }
})()
```

最终效果如下图：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709986385828.png)

