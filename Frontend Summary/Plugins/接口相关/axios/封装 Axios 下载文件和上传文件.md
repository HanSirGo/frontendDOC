# 如何二次封装 Axios 下载文件和上传文件？

# **一 下载文件**

正常情况下，我们用axios下载下来的文件是一个二进制的东西，需要处理才能成一个真真的文件。比如：我用express写一个接口，就是当页面访问的时候，下载这个美女图片。

服务端

![1723978242125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978242125.png)

客户端

![1723978257178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978257178.png)

浏览器console

![1723978271504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978271504.png)



上图里面，对象data里面放了很多二进制的东西，就是我们的文件内容，我们需要用Blob对象，把他转成Blob，然后用 window.URL.createObjectURL(blob) 给出电脑内存里面的url地址，然后创建一个a标签，执行他的click事件，浏览器就完成了自动下载。所以说：文件的自动下载，是前端自己封装的，不是后端做的。

说明：当设置responseType: 'blob'的时候，请求返回值里面的data本来就是Blob类型了，我们就不用再用 `const blob = new Blob([response.data]);`转化了。直接拿内存里面的url即可。

![1723978285772](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978285772.png)



设置`responseType: 'blob'`的结果是

![1723978296494](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978296494.png)



一般为什么我们看不到它，就是因为有人在axios的二次封装里面，已经帮我们做好了。

下载文件的基础代码如下：

```
// axios发起下载文档请求
export const downloadDoc = (id: string) => {
  return request.get(`/downloadDoc?id=${id}`, {
   // 参考官方文档https://www.axios-http.cn/docs/req_config，responseType: 'blob'是浏览器专属
    responseType: 'blob'
  }) as Promise<Blob>
}
// 下载文件通用函数
export function download(filename: string, data: Blob): void {
  let downloadUrl = window.URL.createObjectURL(data) //创建下载的链接
  let anchor = document.createElement('a') // 通过a标签来下载
  anchor.href = downloadUrl
  anchor.download = filename // download属性决定下载文件名
  anchor.click() // //点击下载
  window.URL.revokeObjectURL(downloadUrl) // 下载后释放Blob对象
}
```

封装axios，这里面包含了大文件的下载，还有loading，当然你也可以把防抖，节流全部做进去，不过过度封装也会带来很多不必要的麻烦。

```
import axios, { AxiosInstance, AxiosRequestConfig } from "axios";
import { Message } from "element-ui";
import { jumpLogin, downloadFile } from "@/utils";
import { Loading } from "element-ui";
import { ElLoadingComponent } from "element-ui/types/loading";
// import vm from "@/main";

let loadingInstance: ElLoadingComponent | null = null;
let requestNum = 0;

const addLoading = () => {
  // 增加loading 如果pending请求数量等于1，弹出loading, 防止重复弹出
  requestNum++;
  if (requestNum == 1) {
    loadingInstance = Loading.service({
      text: "正在努力加载中....",
      background: "rgba(0, 0, 0, 0)",
    });
  }
};

const cancelLoading = () => {
  // 取消loading 如果pending请求数量等于0，关闭loading
  requestNum--;
  if (requestNum === 0) loadingInstance?.close();
};

export const createAxiosByinterceptors = (
  config?: AxiosRequestConfig
): AxiosInstance => {
  const instance = axios.create({
    timeout: 1000,    //超时配置
    withCredentials: true,  //跨域携带cookie
    ...config,   // 自定义配置覆盖基本配置
  });

  // 添加请求拦截器
  instance.interceptors.request.use(
    function (config: any) {
      // 在发送请求之前做些什么
      const { loading = true } = config;
      console.log("config:", config);
      // config.headers.Authorization = vm.$Cookies.get("vue_admin_token");
      if (loading) addLoading();
      return config;
    },
    function (error) {
      // 对请求错误做些什么
      return Promise.reject(error);
    }
  );

  // 添加响应拦截器
  instance.interceptors.response.use(
    function (response) {
      // 对响应数据做点什么
      console.log("response:", response);
      const { loading = true } = response.config;
      if (loading) cancelLoading();
      const { code, data, message } = response.data;
      // config设置responseType为blob 处理文件下载
      if (response.data instanceof Blob) {
        return downloadFile(response);
      } else {
        if (code === 200) return data;
        else if (code === 401) {
          jumpLogin();
        } else {
          Message.error(message);
          return Promise.reject(response.data);
        }
      }
    },
    function (error) {
      // 对响应错误做点什么
      console.log("error-response:", error.response);
      console.log("error-config:", error.config);
      console.log("error-request:", error.request);
      const { loading = true } = error.config;
      if (loading) cancelLoading();
      if (error.response) {
        if (error.response.status === 401) {
          jumpLogin();
        }
      }
      Message.error(error?.response?.data?.message || "服务端异常");
      return Promise.reject(error);
    }
  );
  return instance;
```

utils里面的函数

```
src/utils/index.ts

import { Message } from "element-ui";
import { AxiosResponse } from "axios";
import vm from "@/main";

/**
 *   跳转登录
 */
export const jumpLogin = () => {
  vm.$Cookies.remove("vue_admin_token");
  vm.$router.push(`/login?redirect=${encodeURIComponent(vm.$route.fullPath)}`);
};

/**
 * 下载文件
 * @param response
 * @returns
 */
export const downloadFile = (response: AxiosResponse) => {
  console.log("response.data.type:", response.data.type);
  return new Promise((resolve, reject) => {
    const fileReader = new FileReader();
    fileReader.onload = function () {
      try {
        console.log("result:", this.result);
        const jsonData = JSON.parse((this as any).result); // 成功 说明是普通对象数据
        if (jsonData?.code !== 200) {
          Message.error(jsonData?.message ?? "请求失败");
          reject(jsonData);
        }
      } catch (err) {
        // 解析成对象失败，说明是正常的文件流
        const blob = new Blob([response.data]);
        // 本地保存文件
        const url = window.URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.href = url;
        const filename = response?.headers?.["content-disposition"]
          ?.split("filename*=")?.[1]
          ?.substr(7);
        link.setAttribute("download", decodeURI(filename));
        document.body.appendChild(link);
        link.click();
        resolve(response.data);
      }
    };
    fileReader.readAsText(response.data);
  });
};
```

axios的封装就是创建一个axios的实例，然后在请求拦截器和响应拦截器里面写入你要拦截的东西，比如loading，就是在请求里面拦截，错误显示��是在响应里面拦截的。

![1723978336819](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978336819.png)



当然，你在处理错误的时候，可以把所有的错误都列出来

```
export const CODE_MESSAGE: { [key: number]: string } = {
    200: '服务器成功返回请求的数据',
    201: '新建或修改数据成功',
    202: '一个请求已经进入后台排队（异步任务）',
    204: '删除数据成功',
    400: '发出的请求有错误，服务器没有进行新建或修改数据的操作',
    401: '用户没有权限（令牌、用户名、密码错误）',
    403: '用户得到授权，但是访问是被禁止的',
    404: '发出的请求针对的是不存在的记录，服务器没有进行操作',
    406: '请求的格式不可得',
    410: '请求的资源被永久删除，且不会再得到的',
    422: '当创建一个对象时，发生一个验证错误',
    500: '服务器发生错误，请检查服务器',
    502: '网关错误',
    503: '服务不可用，服务器暂时过载或维护',
    504: '网关超时'
  }
```

然后在响应拦截里面这样做：

![1723978356419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978356419.png)



# **二 上传文件**

最简单的上传文件：在html里面写一个input标签，如下：

```
    <input class="upload" type="file" ref="upload" accept="image/jpeg,image/jpg,image/png" id="fileUpload">
```

在js文件里面写：

```
const fileUpload = document.getElementById("fileUpload")
fileUpload.onchange = (e)=>{
  console.log( e.target.files[0])
}
```

有些人离开框架就不会写代码了。这真的很危险，操作如下：

![1723978375730](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978375730.png)



![1723978387472](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978387472.png)



当你看到`index.js`的时候，你会发现和写`vue，react`一样了，是不是？

现在启动页面，看看结果如下：上传一个文件以后，在开发者工具里面你就能看到上传的文件信息了

![1723978397788](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978397788.png)



如果咱们的数据是以form的形式传给后端，那么我们就要转化为`formdata`才行

```
const fileUpload = document.getElementById("fileUpload")

fileUpload.onchange = (e)=>{
  console.log( e.target.files[0])
  let file = e.target.files[0]
  // 实例化
  let formdata = new FormData()
  formdata.append('file', file)
}
```

现在我们写一下后端存储文件的代码，后端是express搭建的，直接用node ./server/index,js 就能启动，代码如下：

```
const express = require('express');
const multer = require('multer');
const app = express();
const port = process.env.PORT || 3002;


// 引入需要的模块

let path = require("path");
let fs = require("fs");
//app.use('/uploads',express.static("./upload")); // 托管静态资源
//1. 设置文件存放位置
let upload = multer({dest:'uploads'})
app.post('/upload',upload.single('file'),(req,res)=>{  // file 与前端input 的name属性一致
    res.setHeader('Access-Control-Allow-Origin', '*'); 
  // 2. 上传的文件信息保存在req.file属性中
    console.log(req.file)  
    let oldName = req.file.path; // 上传后默认的文件名 : 15daede910f2695c1352dccbb5c3e897
    let newName = 'uploads/'+req.file.originalname  // 指定文件路径和文件名
    // 3. 将上传后的文件重命名
    fs.renameSync(oldName, newName);
    // 4. 文件上传成功,返回上传成功后的文件路径
    res.send({
        err: null,   
        url: "http://localhost:3002/" +newName // 复制URL链接直接浏览器可以访问
    });
})

app.listen(port, () => {
    console.log(`已经启动服务，端口号是：${port}`)
})
```

启动后在vscode上就能看到信息如下：

![1723978420115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978420115.png)



用axios请求，我们需要用`POST`方法，再配置`headers`,需要这个浏览器才知道是表单 `headers: { 'Content-Type': 'multipart/form-data;charset=UTF-8' }`如果不指定是form-data，那么默认就是json的，不信你是试试看如下：

![1723978430612](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978430612.png)



所以需要在请求的时候手动设置下才行，设置完以后，发送请求，后端才能识别出来，你发送的是form数据，然后用具体的方式对待你传过来的文件。

```
const fileUpload = document.getElementById("fileUpload")
fileUpload.onchange = (e)=>{
  console.log(222222222222)
    // 获取file
    let file = e.target.files[0]
    // 实例化
    let formdata = new FormData()
    formdata.append('file', file)
    axios.post('http://localhost:3002/upload', formdata, {
        headers: {
          'Content-Type': 'multipart/form-data;charset=UTF-8'
        }
      })
      .then(response => {
        console.log('File uploaded successfully');
        // 处理响应数据
      })
      .catch(error => {
        console.log('Error uploading file');
        // 处理错误情况
      });
}
```

测试如下：

![1723978449797](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978449797.png)



![1723978459752](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978459752.png) 看文件是不是存到uploads里面去了

![1723978470100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978470100.png)



如果咱们是form上传，你就把input包在form里面。

```
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file" />
    <button>提交</button>
</form>
```

上传的时候，如果要对axios进行封装，我们只需要将请求头的`'Content-Type': 'multipart/form-data;charset=UTF-8'`纳入axios的请求拦截器里面就好了。

![1723978485256](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978485256.png)



封装代码如下：

![1723978499604](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978499604.png)



![1723978512341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978512341.png)



打包以后，我们接入看看。（你不打包也可以测试，是为了封装axios才打包的。）

![1723978523148](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978523148.png)



执行代码：

![1723978534529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723978534529.png)



