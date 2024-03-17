## Ajax

> ajax是一种异步通信的方法，通过直接由 js 脚本向服务器发起 http 通信，然后根据服务器返回的数据，更新网页的相应部分，而不用刷新整个页面的一种方法。

#### 创建步骤

![1710586862065](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710586862065.png)

#### 原生Ajax

```js
//1：创建Ajax对象
var xhr = window.XMLHttpRequest?new XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');// 兼容IE6及以下版本
//2：配置 Ajax请求地址
xhr.open('get','index.xml',true);
//3：发送请求
xhr.send(null); // 严谨写法
//4:监听请求，接受响应
xhr.onreadysatechange=function(){
     if(xhr.readySate==4&&xhr.status==200 || xhr.status==304 )
          console.log(xhr.responsetXML)
}
```

```js
// 封装ajax
function ajax(method,url,params,callback) {
 //对参数进行处理
 method=method.toUpperCase()
 let post_params=null
 let get_params=''
 
 if (method==='GET') {
  if (typeof params==='object') {
   let tempArr=[]
   for (let key in params) {
    tempArr.push(`${key}=${params[key]}`)
   }
   params=tempArr.join('&')
  }
  get_params=`?${params}`
 } else {
  post_params=params
 }

 //发请求
 let xhr=new XMLHttpRequest()

 xhr.onreadystatechange=function(){
  if (xhr.readyState!==4) return
  callback(xhr.responseText)
 }

 xhr.open(method,url+get_params,false)
 if (method==='POST')
  xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')

 xhr.send(post_params) 
}

ajax('get','https://www.baidu.com',{id:15},data=>console.log(data))
```

#### jQuery写法

```js
  $.ajax({
      type:'post',
      url:'',
      async:ture,//async 异步  sync  同步
      data:data,//针对post请求
      dataType:'jsonp',
      success:function (msg) {

      },
      error:function (error) {

      }
  })
```

#### promise 封装实现

```js
// promise 封装实现：

function getJSON(url) {
  // 创建一个 promise 对象
  let promise = new Promise(function(resolve, reject) {
    let xhr = new XMLHttpRequest();

    // 新建一个 http 请求
    xhr.open("GET", url, true);

    // 设置状态的监听函数
    xhr.onreadystatechange = function() {
      if (this.readyState !== 4) return;

      // 当请求成功或失败时，改变 promise 的状态
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };

    // 设置错误监听函数
    xhr.onerror = function() {
      reject(new Error(this.statusText));
    };

    // 设置响应的数据类型
    xhr.responseType = "json";

    // 设置请求头信息
    xhr.setRequestHeader("Accept", "application/json");

    // 发送 http 请求
    xhr.send(null);
  });

  return promise;
}
```

