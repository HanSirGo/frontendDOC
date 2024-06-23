# 前端必备技能之--使用Electron若依集成打包成exe文件

**Electron简介**

Electron(官网:https://www.electronjs.org/zh/)是一个开源的桌面应用程序开发工具，它允许开发者使用常用的网页技术，如 HTML、CSS 和 JavaScript，来构建跨平台的桌面应用程序。Electron 的核心思想是使用 Web 技术来构建应用程序，同时借助 Node.js 来提供本机操作系统的访问和功能。这使得开发者可以使用熟悉的工具和语言来创建高性能、可扩展且功能丰富的桌面应用。

Electron 的工作原理相当简单。开发者编写基于 Web 技术的前端界面，然后使用 Node.js 提供的 API 访问本地操作系统的功能，如文件系统、网络和系统通知。最终，Electron 将前端界面和本地系统功能整合到一个独立的桌面应用程序中。

Electron 在跨平台方面表现出色，开发者只需编写一次代码，就可以在 Windows、macOS 和 Linux 等主流操作系统上运行。这使得开发者可以更加专注于应用程序的功能和用户体验，而无需为不同平台的兼容性问题而担忧。

由于其灵活性和强大的生态系统，Electron 已经成为许多知名应用程序的首选开发工具。其中包括 Visual Studio Code、Slack、Discord 等等。这些应用程序展示了 Electron 的优势，既可以提供出色的性能和用户体验，又能够跨越不同的操作系统。

## **准备工作**

首先需要版本号大于14.0.0的较新的node版本

建议最好先安装淘宝镜像源，命令：

```
npm config set  registry https://registry.npm.taobao.org
```

查看是否安装成功

```
cnpm -v
```

如图安装成功

![1716110905661](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716110905661.png)

然后执行

```
cnpm install electron --save-dev
```

## **Electron安装**

  \1. 添加打包工具

```
vue add electron-builder
```

   \2. 配置electron镜像地址：

```js
npm config set registry http://registry.npm.taobao.org/
npm config set electron_mirror="https://npm.taobao.org/mirrors/electron/"
npm config set electron_builder_binaries_mirror="http://npm.taobao.org/mirrors/electron-builder-binaries/"
```

   \3. 打开env.production文件，项目中设置跨域（访问后端接口）

![1716110983055](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716110983055.png)

```
VUE_APP_BASE_API = '线上地址/prod-api'
```

​    \4. 设置应用图标。

需要256*256，格式为ico。

注意：不能采用直接修改后缀名的方式，需要使用专门的网站转换

在build文件夹里 新增一个 256X256的名字为icon的ico文件

打开 background.js文件 在createWindow事件里面

```
const win = new BrowserWindow({ width: 800, height: 600, icon: 'src/assets/logo/logo.ico', })
```

​     \5. 进行打包，命令：

```
npm run electron:build
```

打包成功后会在dist_electron中生成了exe文件，点击安装即可

![1716111040778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716111040778.png)

## **其他问题**

#### 1. 登录成功不能跳转，是因为jscookie的问题，将登录页面 （login）页面的所有cookies修改成 localStorage

> -  const username = Cookies.get("username")
>
>   修改成
>
>   const username = localStorage.getItem("username")
>
>   
>
> - 找到路径 src/utils/auth 页面的所有cookies修改成  localStorage
>
>   export function getToken() {
>
>     return localStorage.getItem(TokenKey)
>
>   }
>
>    
>
> - 找到路径 src/router/index页面的 将history修改成 hash
>
>   export default new Router({
>
>    mode: 'hash', // 去掉url中的#
>
>   scrollBehavior: () => ({ y: 0 }),
>
>   routes: constantRoutes
>
>    })
>
>   
>
> - 如果发现跳转到该页面没有调接口，也要把该页面cookie修改成localStorage

## 2. 修改打包名称为中文

​    在package.json文件 增加

```
"productName": "你的中文名称",
```

\3. 打包后退出登录页面白屏

找到退出登录页面 src/layout/components/Navbar文件



> 找到退出事件 
>
>  async logout() {
>
>   this.$confirm('确定注销并退出系统吗？', '提示', {
>
> ​    confirmButtonText: '确定',
>
> ​    cancelButtonText: '取消',
>
> ​    type: 'warning'
>
>   }).then(() => {
>
> ​    this.$store.dispatch('LogOut').then(() => {
>
> ​     // location.href = '/index';    
>
> ​          换成 
>
> ​        this.$router.push('/login')
>
> ​    })
>
>   }).catch(() => {});
>
> }
>
> }

\4. 设置只能打开一个exe文件

（在background.js 增加下面这段代码，将app关闭的事件也放在里面）



> let myWindow = null
>
> const additionalData = { myKey: 'myValue' }
>
> const gotTheLock = app.requestSingleInstanceLock(additionalData)
>
>   if (!gotTheLock) {
>
> ​    app.quit()
>
>   } else {
>
> app.on('second-instance', (event, commandLine, workingDirectory, additionalData) => {
>
>   // 输出从第二个实例中接收到的数据
>
>   console.log(additionalData)
>
>   // 有人试图运行第二个实例，我们应该关注我们的窗口
>
>   if (myWindow) {
>
> ​    if (myWindow.isMinimized()) myWindow.restore()
>
> ​    myWindow.focus()
>
>   }
>
> })
>
>  app.on('window-all-closed', () => {
>
> ​    // On macOS it is common for applications and their menu bar
>
> ​    // to stay active until the user quits explicitly with Cmd + Q
>
> ​    if (process.platform !== 'darwin') {
>
> ​      app.quit()
>
> ​    }
>
>   })
>
> }

