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

## 遇到的问题

##### 请求地址变成了localhost:8080 + 配置的本地地址 

![1713089221797](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713089221797.png)

```js
# 原因：baseURL配置的ip不完整

axios.defaults.baseURL = '192.168.1.107'

# 处理方法：配置完善的ip地址，加上http或https

axios.defaults.baseURL = 'http://192.168.1.107'

#再次调用后发现已经可以了
```

![1713089263383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713089263383.png)

##### get请求传参

```js
# { name:'xiaoli',age:18 }

axios.get('url',{
	params:{ name:'xiaoli',age:18 }
})
// url?name=xiaoli&age=18

# {ids:[1,2,3]}

axios.get('url',{
    params:{ids:[1,2,3]}
})
// url?ids[]=1&ids[]=2&ids[]=3

axios.get('url',{
    params,
    paramsSerializer: params => {
            return qs.stringify(params,{arrayFormat:'repeat'})
        }
    }
})
// 上边的代码报错 可以试试这一段代码
paramsSerializer: {
    serialize: params => {
        return qs.stringify(params,{arrayFormat:'repeat'})
    }
}
// url?ids=1&ids=2&ids=3

# 1、qs.stringify({ ids:[1,2,3] }, { arrayFormat: 'indices' })
// 输出结果：'ids[0]=1&ids[1]=2&ids[2]=3'
# 2、qs.stringify({ ids:[1,2,3] }, { arrayFormat: 'brackets' })
// 输出结果：'ids[]=1&ids[]=2&ids[]=3'
# 3、qs.stringify({ ids:[1,2,3] }, { arrayFormat: 'repeat' })
// 输出结果：'ids=1&ids=2&ids=3'
# 4、qs.stringify({ ids:[1,2,3] }, { arrayFormat: 'comma' })
// 输出结果：'ids[0]=1&ids[1]=2&ids[2]=3'
# 5、qs.stringify({ ids:[1,2,3] }, { indices: false })
// 输出结果：'ids=1&ids=2&ids=3'
# 6、qs.stringify({ ids:[1,2,3] }, { indices: true })
// 输出结果：'ids[0]=1&ids[1]=2&ids[2]=3'
```

##### get请求日期时间参数

```js
params = {time:'2024-04-18 00:00:00'}
# 1. 一般情况
axios.get('url',{
    params
})
// url?time=2024-04-18+00:00:00
# 2. 手动拼接
axios.get(`url?time=${params.time}`)
// url?time=2024-04-18%2000:00:00
# 3. qs
axios.get('url',{
    params,
    paramsSerializer: params => {
            return qs.stringify(params,{arrayFormat:'repeat'})
        }
    }
})
// url?time=2024-04-18%2000%3000%3000
```

##### CancelToken、AbortController取消重复请求
 **场景**

> 在开发中会遇到连续点击按钮同时多次请求相同接口，一般我们会对按钮进行一个loading操作，但是有时候可能是一个tab页签，点击页签就要请求一次数据，当频繁的来回点击页签的时候，会发送多次请求，如果在数据还没有请求到的时候禁用tab页签点击，会给用户一种页签组件点不动的感觉，于是我考虑换个思路，发现本次的请求和之前的请求相同，则取消之前的请求，以本次的请求为准。

###### 1. axios 实例

**V1.0（使用CancelToken，无白名单，基本够用）**

由于项目中使用到的是axios来进行请求，经过一通百度大法，发现axios有一个叫CancelToken的api，用于给请求打上一个标识，并返回一个函数，只要调用这个函数，就可以取消被标识的请求

整体思路如下：

1.定义请求队列，以唯一标识（url+method）为key来缓存取消函数

2.在请求拦截中判断，如果标识相同，说明为重复请求，根据唯一标识删除缓存队列中的取消函数，再将新的取消函数进行缓存

3.响应拦截中判断，如果本次请求成功，根据唯一标识删除缓存队列中的取消函数，如果请求失败，通过axios内置函数isCancel判断，本次失败的原因是接口请求失败，还是通过CancelToken进行的取消
```javascript
import axios from 'axios'
 
// CancelToken能为一次请求‘打标识’
// isCancel用于判断请求是不是被CancelToken取消的
const { CancelToken, isCancel } = axios
 
// 请求队列，缓存发出的请求
const cacheRequest = {}
 
// axios实例
const service = axios.create({
  timeout: 10*60*1000,
  // headers: {
  //  'Content-Type': 'application/json; charset=UTF-8'
  // },
})
 
// 请求拦截
service.interceptors.request.use((config) => {
    const { url, method } = config
    // 请求地址和请求方式组成唯一标识，将这个标识作为取消函数的key，保存到请求队列中
    const reqKey = `${url}&${method}`
    // 如果存在重复请求，删除之前的请求
    removeCacheRequest(reqKey)
    // 将请求加入请求队列
    config.cancelToken = new CancelToken(c => {
      cacheRequest[reqKey] = c
    })
})
 
// 响应拦截
service.interceptors.response.use(
    (response) => {
        // 请求成功，从队列中移除
        const { url, method } = response.config
        removeCacheRequest(`${url}&${method}`)
    },
    (error) => {
    	// 请求失败，从队列中移除(这两行可加可不加)
        const { url, method } = error.config
        removeCacheRequest(`${url}&${method}`)
        
        // 请求失败，使用isCancel来区分是被CancelToken取消，还是常规的请求失败
        if (isCancel(error)) {
          // 通过CancelToken取消的请求不做任何处理
          return Promise.reject({
            message: '重复请求，自动拦截并取消'
          })
        } else{
        	// 判断一下是不是断网了
        	if(!window.navigator.onLine) return console.log('断网')
        	
            // 正常请求发生错误,抛出异常等统一提示
            console.log(error.response, 'errMsg')
        }
    }
)
 
/**
 * @desc 删除缓存队列中的请求
 * @param {String} reqKey 本次请求的唯一标识 url&method
 */
function removeCacheRequest(reqKey) {
  if (cacheRequest[reqKey]) {
    // 这里调用的就是上面的CancelToken生成的c函数，调用就会取消请求
    cacheRequest[reqKey]()
    delete cacheRequest[reqKey]
  }
}

// 如果不想 二次封装，可以直接导出 service,执行第三步 3
// export {service, cacheRequest}
```
看起来没有问题了，确实也达到了我所想要的效果，之前的请求如果没有完成都被取消，始终只有最新的请求会被发出去
![1713697556817](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713697556817.png)

**V2.0（使用AbortController，增加白名单）**

回想自己用了这么久axios，都还不知道官方提供了这种api，于是我决定翻一下文档，复习一下axios，结果一翻不得了，官方标识从v0.22.0开始已经弃用CancelToken，建议我们使用AbortController来实现，由于项目中使用的是0.26.1（明明还是可以用），还是紧跟时代步伐，换成官方推荐的吧。
![1713697572772](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713697572772.png)
 将上面的代码进行改造，顺便加了一个白名单，因为项目是vue的后台项目，有可能几个兄弟组件在初始化的时候都请求了同一个接口，这种情况下应该允许这些接口的重复请求

```javascript
import axios from 'axios'
 
// 注意：AbortController是fetch API提供的，不需要从axios引入
// isCancel用于判断请求是不是被AbortController取消的
const { isCancel } = axios
 
// 请求队列，缓存发出的请求
const cacheRequest = {}
// 不进行重复请求拦截的白名单
const cacheWhiteList = ['/foo/bar&get']
 
// axios实例
const service = axios.create({
  timeout: 10*60*1000,
  // headers: {
  //  'Content-Type': 'application/json; charset=UTF-8'
  // },
})
 
// 请求拦截
service.interceptors.request.use((config) => {
    const { url, method } = config
    // 请求地址和请求方式组成唯一标识，将这个标识作为取消函数的key，保存到请求队列中
    const reqKey = `${url}&${method}`
    // 如果存在重复请求，删除之前的请求
    if (cacheWhiteList.indexOf(reqKey) === -1) {
      removeCacheRequest(reqKey)
      // 将请求加入请求队列，通过AbortController来进行手动取消
      const controller = new AbortController()
      config.signal = controller.signal
      cacheRequest[reqKey] = controller
    }
})
 
// 响应拦截
service.interceptors.response.use(
    (response) => {
        // 请求成功，从队列中移除
        const { url, method } = response.config
        removeCacheRequest(`${url}&${method}`)
    },
    (error) => {
    	// 请求失败，从队列中移除(这两行可加可不加)
        const { url, method } = error.config
        removeCacheRequest(`${url}&${method}`)

        // 请求失败，使用isCancel来区分是被CancelToken取消，还是常规的请求失败
        if (isCancel(error)) {
          // 通过CancelToken取消的请求不做任何处理
          return Promise.reject({
            message: '重复请求，自动拦截并取消'
          })
        } else{
        	// 判断一下是不是断网了
        	if(!window.navigator.onLine) return console.log('断网')
        	
            // 正常请求发生错误,抛出异常等统一提示
            console.log(error.response, 'errMsg')
        }
    }
)
 
/**
 * @desc 删除缓存队列中的请求
 * @param {String} reqKey 本次请求的唯一标识 url&method
 */
function removeCacheRequest(reqKey) {
  if (cacheRequest[reqKey]) {
    // 通过AbortController实例上的abort来进行请求的取消
    cacheRequest[reqKey].abort()
    delete cacheRequest[reqKey]
  }
}
// 如果不想 二次封装，可以直接导出 service,执行第三步 3
// export {service, cacheRequest}

```
###### 2. axios 请求二次封装

```javascript
/**
 * get方法，对应get请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 */
export function get(url, params) {
  return new Promise((resolve, reject) => {
    service.get(url, {
      params
    })
      .then(res => {
        resolve(res.data);
      })
      .catch(err => {
        reject(err.data)
      })
  });
}
/**
 * post方法，对应post请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 */
export function post(url, params) {
  return new Promise((resolve, reject) => {
    service.post(url, params)
      .then(res => {
        resolve(res.data);
      })
      .catch(err => {
        reject(err.data)
      })
  });

}
/**
 * put方法，对应put请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 */
export function put(url, params) {
  return new Promise((resolve, reject) => {
    service.put(url, params)
      .then(res => {
        resolve(res.data);
      })
      .catch(err => {
        reject(err.data)
      })
  });
}
/**
 * delete方法，对应delete请求
 * @param {String} url [请求的url地址]
 * @param {Object} params [请求时携带的参数]
 */
export function $delete(url, params) {
  return new Promise((resolve, reject) => {
    service.delete(url, {
      params: params
    })
      .then(res => {
        resolve(res.data);
      })
      .catch(err => {
        reject(err.data)
      })
  });
}
```
###### 3. api接口统一管理文件，后续用到的接口都在这个文件导出

```javascript
import { get, post, $delete, put } from "./http";
export const getUserDataList=(data)=>{
//  /a表示接口
  return get('/a',data)
}
export const getUserData=(data)=>{
//  /a/b表示接口
  return post('/a/b',data)
}
//编辑用户
export const putUserData=(data)=> {
  return put(`/a/b/detail/${data.user_id}`, data);
};
//删除用户
export const delUserData=(data)=> {
  return $delete(`/a/b/detail/${data.user_id}`, data);
};
```
###### 扩展
###### axios单个取消请求
```javascript
var CancelToken = axios.CancelToken;
var cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// 取消请求
cancel();
```
###### 路由切换取消接口

```javascript
import { cacheRequest } from './api/axios'
router.beforeEach((to,from,next)=> {
  // 方法1：
  for (const item in cacheRequest) {
    cacheRequest[item].abort()
    // cacheRequest[item]()
    delete cacheRequest[item]
  }
  next()
})
```
###### ajax 取消请求

```javascript
document.querySelect('button').addEventListener('clicl',()=>{
	const xhr = new XMLHttpRequest()
	const url = 'http://localhost:3000/users'
	xhr.open('get',url)
	xhr.onreadystatechange=()=> {
		console.log(xhr.readyState)
	}
	xhr.send()
	setTimeout( () => {
		xhr.abort()
	}, 1000 )
})
```
###### 后端返回数据缺失精度

> js中的数值Number没人是int，4个字节，16位，最大只能存16位，超16位后面的数据可能会被转化为0
> Number 类型的数据是 64 位双精度，能够安全表示的数据范围为 -253 ~ 253 ， 即最大安全值为 9007199254740992，在我的项目中，后端传递的数据明显超出这个范围，造成了精度丢失，只保证了前 16 位的精度，16 位之后的数据，只用 0 表示。
>

> 有意思的地方是，postman测试接口时，查看返回值精度并未丢失，是字符串。

```javascript
var Num = 1234567891011121314
js会被转化为 1234567891011121000
```

 - 第一种：后端将此字段类型修改为 String 类型
 - 第二种：前端在接收到响应数据时，反序列化之前，将此字段类型修改为 String 类型

> 此次我用到的就是这种解决方式，新封装一个 axios 实例，专门用来解决这种精度丢失的问题，在创建实例时，使用正则对 id 属性进行类型的转换
```javascript
	// 创建 axios 实例
	const transformResponse = function (res) {
	    // id 超过16位， 精度丢失，解决
	    res = res.replace(/\"id\":(\d+)/g, '"id":"$1"')
	    return JSON.parse(res)
	}
	const serviceTransform = axios.create({
	    transformResponse
	});

```
 - 第三种：下载 json-bigint 插件解决

json-bigint是一个第三方包，它在把json字符串转json对象的过程中，自动识别大整数，把大整数转成一个对象来表示，这样就不会产生精度丢失的问题了。

```javascript
npm i json-bigint
```
导入

```javascript
import JSONBig from 'json-bigint'
const JSONBigtoString = JSONBig({ storeAsString: true })
const service = axios.create({
	transformResponse: [
		function (data) {
			try { 
				return JSONBigtoString.parse(data)
			} catch (error) {
				return data
			}
		}
	]
})
```
###### 拦截器添加 Authorization 权限

```javascript
service.interceptors.request.use(
	(config) => {
		if(本地存在token) {
			config.headers.Authorization = `Bearer ${ 需要的token }`
		}
		if(特殊需要用户名和密码的接口) {
			config.headers.Authorization = `Basic ${ 将用户名和密码先转为base64格式 }`
		}
	},
	(error) => {
		return Promise.reject(error)
	}
)
```