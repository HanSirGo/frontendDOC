### 后端携带set-cookie（自动设置cookie）
### document.cookie（手动设置cookie--0）
```javascript
document.cookie = "ioiopipoadiasdasdbasdbas";   // 非跨域传递cookie 直接设置cookie即可
```
### 携带cookie（手动设置cookie--1）
**请求头中携带**

```javascript
header = {  'Cookie':'cto_bundle=_DpLll9WcDY3UCUyRlBnWDZt;_jzqx=1.1691985966.1691985966.1.jzqsr=aa7a%2Ecn|jzqct=/.-;ECS_ID=a9bd0486ba5b0f2f65637f64719d72e75add4995; ECS[visit_times]=8;'
}
// get 
res = requests.get('http://www.aa7a.cn/',headers=header)
// post 
res = requests.post('https://dig.chouti.com/link/vote', data=data, headers=header)
```
### 插件——js-cookie（手动设置cookie--2）
> **js-cookie是一个简单的，轻量级的处理cookie的jsAPI。用来处理cookie相关的插件**
>  - 适用于所有的浏览器； 
>  - 接收任何字符； 
>  - 严格的测试； 
>  - 无依赖； 
>  - 支持ES模块； 
>  - 支持AMD/CommonJS； 
>  - RFC6265标准； 
>  - 维基；
>  - 可用自定义编码/解码； 
>  - <800字节压缩；
#### 引入
```javascript
npm install js-cookie --save
import Cookies from 'js-cookie'
```
#### js-cookie的添加 获取 删除

 - **添加cookie**
```javascript
// 创建一个名称为name，对应值为value的cookie，由于没有设置失效时间，默认失效时间为该网站关闭时
Cookies.set(name, value)

// 创建一个有效时间为7天的cookie
Cookies.set(name, value, { expires: 7 })

// 创建一个带有路径的cookie
Cookies.set(name, value, { path: '' })

// 创建一个value为对象的cookie
const obj = { name: 'ryan' }
Cookies.set('user', obj)
```
 - **获取cookie**
```javascript
// 获取指定名称的cookie
Cookies.get(name) // value

// 获取value为对象的cookie
const obj = { name: 'ryan' }
Cookies.set('user', obj)
JSON.parse(Cookies.get('user'))

// 获取所有cookie
Cookies.get()
```
 - **删除cookie**
```javascript
// 删除指定名称的cookie
Cookies.remove(name) // value

// 删除带有路径的cookie
Cookies.set(name, value, { path: '' })
Cookies.remove(name, { path: '' })
```
#### 如何将cookie的过期时间设置为在一天之内呢？
js-cookie 的expires 属性是支持一个Data实例对象的。
这提供了很大的灵活性，因为 Date 实例可以指定任何时刻。

例子：
> 假设cookie是要在15分钟之后过期
```javascript
var inFifteenMinutes = new Date(new Date().getTime() + 15 * 60 * 1000);
Cookies.set('foo', 'bar', {
    expires: inFifteenMinutes
});
```
> 你也可以设置cookie的有效时间只有半天
```javascript
var inHalfADay = 0.5;
Cookies.set('foo', 'bar', {
    expires: inHalfADay
});
```
> 或者是 半个小时（30分钟）
```javascript
var in30Minutes = 1/48;
Cookies.set('foo', 'bar', {
    expires: in30Minutes
});
```
#### 请求登录接口获取到token值以后，再存储token到cookie中
![1713767502588](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767502588.png)

> 发送发送同源请求，已经自动携带上了 cookie
### cookie属性的设置：
可以通过 withAttributes() 创建 cookiesIns 的实例, 并且对设置 Cookie 属性全局默认值,
或是
通过将普通对象作为最后一个参数传递给 Cookies.set(...)。 每次调用 Cookies.set(...) 时，属性会覆盖全局默认属性。

使用 withAttributes 设置全局默认项:
```javascript
const cookiesIns = Cookies.withAttributes({ path: '/', domain: '.example.com' })
```
 - **expires**
> 规定cookie何时失效，会被删除 。
> 类型：数字，它的含义是，从cookie被创建时算起的天数或一个 Date 实例。 如果省略，cookie 将成为会话 cookie。
> 要创建在不到一天内过期的 cookie，您可以查看 Wiki 上的常见问题解答。 
> **默认值：是一个会话cookie, 当用户关闭浏览器时 Cookie 就会被删除。**

例子：
```javascript
Cookies.set('name', 'value', { expires: 365 })
Cookies.get('name') // => 'value'
Cookies.remove('name')
```
 - **path**
> 类型：字符串，规定 cookie 可见的路径。 
> 默认值：/

例子：
```javascript
Cookies.set('name', 'value', { path: '' })
Cookies.get('name') // => 'value'
Cookies.remove('name', { path: '' })
```
 - **domain**
> 类型：字符串，规定 cookie 可见的有效域（实际上就是域名）。 该 cookie 也将对该域名下的所有子域可见。
> 默认值：Cookie 仅对创建 cookie 的页面的域或子域可见

例子：假设在 site.com 上创建了一个 cookie：
```javascript
Cookies.set('name', 'value', { domain: 'subdomain.site.com' })
Cookies.get('name') // => undefined (需要在'subdomain.site.com'这个域名下去读取这个cookie)
```
 - **secure**
> 类型：true 或 false，指示 cookie 传输是否需要安全协议 (https)
> 默认值：false, 无安全协议要求。
```javascript
Cookies.set('name', 'value', { secure: true })
Cookies.get('name') // => 'value'
Cookies.remove('name')
```
 - **sameSite**
> 类型：字符串，允许控制浏览器是否与跨站点请求一起发送 cookie
> 默认值：未设置

例子：
```javascript
Cookies.set('name', 'value', { sameSite: 'strict' })
Cookies.get('name') // => 'value'
Cookies.remove('name')
```
### [解决跨域携带cookie问题](http://www.wangt.cc/2022/07/%E9%9D%A2%E8%AF%95%E9%A2%98-%E8%B7%A8%E5%9F%9F%E8%AF%B7%E6%B1%82%E5%A6%82%E4%BD%95%E6%90%BA%E5%B8%A6cookie/)
**在前端请求的时候设置 request 对象的属性 withCredentials 为 true;**

XMLHttpRequest.withCredentials 属性是一个Boolean类型，它指示了是否该使用类似 cookies,authorization headers(头部授权)或者 TLS 客户端证书这一类资格证书来创建一个跨站点访问控制（cross-site Access-Control）请求。在同一个站点下使用withCredentials属性是无效的。

如果在发送来自其他域的 XMLHttpRequest 请求之前，未设置withCredentials  为 true，那么就不能为它自己的域设置 cookie 值。而通过设置withCredentials  为 true 获得的第三方 cookies，将会依旧享受同源策略，因此不能被通过document.cookie或者从头部相应请求的脚本等访问。
> XMLHttpRequest发请求需要设置
> withCredentials=true

> fetch 发请求需要设置
> credentials = include

> axios发请求配置：
> axios.defaults.withCredentials = true
```javascript
// 修改跨域请求的代码
crossButton.onclick = function () {
  axios({
    withCredentials: true, // ++ 新增
    method: "get",
    url: "http://localhost:8003/anotherService",
  }).then((res) => {
    console.log(res);
  });
};
```
> 接下来解决跨域问题，就可以了
> 跨域服务端在response的header中配置"Access-Control-Allow-Origin", “http://xxx:${port}”;  // 不能设置为 “” 或 *，而要设置为客户端发送请求时的IP地址
> 跨域服务端在response的header中配置"Access-Control-Allow-Credentials", “true”
### 如果一个cookie同时满足以下条件，则这个cookie会被附带到请求中
- **cookie没有过期** 
- **cookie中的域和这次请求的域是匹配的**
> 比如cookie中的域是123.com，则可以匹配的请求域是123.com、www.123.com、234.123.com等等
> 比如cookie中的域是www.123.com，则只能匹配www.123.com这样的请求域
> cookie是不在乎端口的，只要域匹配即可

 - **cookie中的path和这次请求的path是匹配的**
> 比如cookie中的path是/news，则可以匹配的请求路径可以是/news、/news/detail、/news/a/b/c等等，但不能匹配/blogs
> 如果cookie的path是/，可以想象，能够匹配所有的路径

 - **验证cookie的安全传输**
> 如果cookie的secure属性是true，则请求协议必须是https，否则不会发送该cookie
> 如果cookie的secure属性是false，则请求协议可以是http，也可以是https
> 如果一个cookie满足了上述的所有条件，则浏览器会把它自动加入到这次请求中

具体加入的方式是，浏览器会将符合条件的cookie，自动放置到请求头中。
### Vue3项目中js-cookie库的使用
![1713767540080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767540080.png)
![1713767553041](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767553041.png)