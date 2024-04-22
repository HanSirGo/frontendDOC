## qs

```
传递参数要将参数序列化。
qs 是一个增加了一些安全性的查询字符串解析和序列化字符串的库。
```

```js
npm install qs
import qs from 'qs’

qs.parse()是将URL解析成对象的形式
qs.stringify()是将对象 序列化成URL的形式，以&进行拼接
```

另外qs还不止这点本事，这里附上一个详细的文档

https://blog.csdn.net/sansan_7957/article/details/82227040

### 传参序列化（qs）插件使用
#### 下载
```javascript
npm install qs
```
#### 使用
```javascript
import axios from 'axios';
import qs from 'qs'
```
#### 方法：
```javascript
qs.parse()：将URL解析成对象的形式
qs.stringify()：将对象 序列化成URL的形式，以&进行拼接
```
#### 在axios使用qs

```javascript
paramsSerializer(params) {
    return qs.stringify(params, { arrayFormat: 'repeat', allowDots: true });
  }
```
上边的代码报错 可以试试这一段代码

```javascript
paramsSerializer: {
	serialize: (params)=>{
    	return qs.stringify(params, { arrayFormat: 'repeat', allowDots: true });
	}
}
```

##### 1. 整体axios使用
```javascript
import axios from 'axios';
import qs from 'qs'// 引入qs
const axios_ = axios.create({
  baseURL: config.baseURL,
  timeout: 60000, // 请求时间超过60秒视为超时
  withCredentials: true,
  headers: {
    'X-Requested-With': 'XMLHttpRequest'
  },
  paramsSerializer(params) {
    return qs.stringify(params, { arrayFormat: 'repeat', allowDots: true });
  }
});
```
##### 2. 某一个get请求来设置
```javascript
axios.get(url,{
	 params, 
	 paramsSerializer(params) {
	    return qs.stringify(params, { arrayFormat: 'repeat', allowDots: true });
	  }
}).then(res => {}).catch(err => {})
```
##### 3. 在拦截器中设置
```javascript
    //只针对get方式进行序列化
    if (config.method === 'get') {
      config.paramsSerializer = function (params) {
        return qs.stringify(params, { arrayFormat: 'repeat' })
      }
    }
```
#### 解决的问题
##### 1. 数组参数
**paramsSerializer序列化，处理数组有如下几个形式**
```javascript
qs.stringify({ids: [1, 2, 3]}, {indices: false})
 //形式：ids=1&ids=2&id=3
 
qs.stringify({ids: [1, 2, 3]}, {arrayFormat: 'indices'})
 //形式：ids[0]=1&ids[1]=2&ids[2]=3
 
qs.stringify({ids: [1, 2, 3]}, {arrayFormat: 'brackets'})
 //形式：ids[]=1&ids[]=2&ids[]=3
 
qs.stringify({ids: [1, 2, 3]}, {arrayFormat: 'repeat'}) 
//形式： ids=1&ids=2&id=3
```
**qs.stringify(params, {indices: false})和qs.stringify(params, {arrayFormat: ‘repeat’})都可达到预期的效果**
##### 2. 时间参数
**对于传时间参数时，如‘2023-01-01 12:00:00’，如果没有配置paramsSerializer，中间的空格，会被解析成+**
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/d952657cee174ac3a6e53db22965c03b.png)
**payload中是正常的，但是自动拼接到url后，空格就变成了+，会导致调用接口失败，报400。此时，需要后台特殊处理，或者前端配置paramsSerializer**。
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/2974e3dc19a7409188153e6aadd803e7.png)
**配置paramsSerializer后，空格就会被编码成%20**
##### 3. 排序
```javascript
let params = { c: 'b', a: 'd' };
qs.stringify(params, (a,b) => a.localeCompare(b))

// 结果是
'a=b&c=d'
```
##### 4. 处理json格式的参数
在默认情况下，json格式的参数会用 [] 方式编码，
```javascript
let json = { a: { b: { c: 'd', e: 'f' } } };

qs.stringify(json);
//结果 'a[b][c]=d&a[b][e]=f'
```
但是某些服务端框架，并不能很好的处理这种格式，所以需要转为下面的格式
```javascript
qs.stringify(json, {allowDots: true});
//结果 'a.b.c=d&a.b.e=f'
```