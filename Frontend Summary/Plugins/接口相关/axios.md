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

