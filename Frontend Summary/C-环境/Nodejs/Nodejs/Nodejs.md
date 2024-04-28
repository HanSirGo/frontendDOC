# Node.js

> Node.js是一个开源的，跨平台的javascript运行环境。
>
> 作用：
>
> 1. 开发服务器应用
> 2. 开发工具类应用（webpack、vite、Babel）
> 3. 开发桌面端应用（nodejs->electron->VSCode、Postman）

## 安装node

```js
官网：https://nodejs.org/en/
https://nodejs.org/zh-cn/download/
https://blog.csdn.net/qq_40712862/article/details/120231621

选择适合的版本安装
```

![1709257385297](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709257385297.png)

```js
// 安装成功
node -v
```

## 配置环境变量

> 配置应用程序的环境变量，就可以在cmd终端中输入程序，从而打开应用程序。
>
> <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714013163352.png" alt="1714013163352" style="zoom:50%;" />

## 命令行工具

> 思考?: 如何使用命令行工具打开谷歌浏览器并访问百度首页。
>
> 1. 打开命令行工具：
>
>    a. 搜索 / win键/ 开始--->  输入cmd ---> 点击 最佳匹配下的  命令行提示符
>
>    b.  win键 + R键
>
> 2.  在命令行 输入 chrome （打开的是chrome浏览器）
> 3. 输入 chrome http://www.baidu.com (打开了百度首页)
>
> ![1714014001045](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714014001045.png)
>
> ![1714029448989](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029448989.png)

### 常用命令

> 思考?: 命令行如何查看 【D:/Program Files】里的内容

```js
// 切盘 c -> d
C:\> d:
// 查看 d盘内容 . 当前目录 .. 上级目录
D:\> dir

// 进入 Program Files  
D:\> cd Program Files

// 返回上级目录
cd ..

// 查看文件夹下的所有文件内容,包括子文件的内容
dir /s

ctrl + c // 停止
```

## 运行js

```js
// vscode 中 a.js 右键 '在集成终端中打开' 也可以在cmd中
终端 : node a.js
```

## 注意点

> Node.js中不能使用BOM和DOM的API
>
> nodejs顶级对象 global，浏览器 window
>
> ES2020中引入了顶级对象 globalThis
>
> ![1714029528077](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029528077.png)

![1714029477203](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029477203.png)

![1714029498751](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029498751.png)

## Buffer

![1714029552203](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029552203.png)

![1714029578298](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029578298.png)

![1714029614779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029614779.png)

### 创建Buffer

```js
// 1. alloc 分配
let buf = Buffer.alloc(10)
// 2. allocUnsafe (不安全)会包含旧的内存数据
let buf = Buffer.allocUnsafe(10)
// 3. from将一个字符串或者数组转为buffer 
// 字符串 --> 每一个字母都会转化为这个字符在Uncode码表当中对应的数字，这个数字转成二进制，存到buffer中
let buf = Buffer.from('hello')
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714029639862.png" alt="1714029639862"  />

## 计算机基本组成

> CPU
>
> 内存：存储数据；读写速度较快，断电丢失数据
>
> 硬盘：存储数据；读写速度较慢，断电不丢失数据
>
> 一般程序下载安装在硬盘中
>
> 主板
>
> 显卡

## 程序运行的基本流程

> 程序一般保存在硬盘中，软件安装的过程就是将程序写入硬盘的过程。
>
> 程序在运行时会加载进入内存，然后由CPU读取并执行程序。

> 操作系统：操作系统也是一种应用程序，用来管理和调度硬件资源。
>
> windows Linux macOS
>
> 装系统：将操作系统安装到硬盘上的过程

> **计算机启动的基本过程**：将硬盘上的程序 -载入到内存里边  -  由CPU运行 - 有视频信号 交给显卡处理 显卡处理完成后交由显示器呈现；有声音信号 交给声卡处理 声卡处理完后 交给播放设备音响耳机等

> **应用程序运行**：下载 -- 安装（硬盘）-- 启动（双击） --  将程序从硬盘载入内存，CPU从内存中读取指令执行，执行中遇到视频信号交给显卡处理，显卡把信号传递给显示器显示，遇到音频，交给声卡处理，交给播放设备

## 进程与线程

### 进程

> 进程：行进的程序；进程是程序的一次执行过程。
>
> 查看进程：任务管理器

### 线程

> 线程是一个进程中执行的一个执行流
>
> 一个线程是属于某个进程的
>
> 一个进程可以有多个线程
>
> cmd中可以查看线程：pslist -dmx  进程ID

## Nodejs模块

### fs模块

> fs模块可以实现与硬盘的交互。例如：文件的创建、删除、重命名、移动，还有文件内容的写入、读取，以及文件夹的相关操作。

```js
// 导入 fs
const fs = require('fs')
```

> 换行写入： '\r\n'

#### 文件操作

##### 文件写入

###### writeFile异步写入

```js
fs.writeFile(file, data[, options], callback)

// file 文件名,文件不存在，自动创建;存在,里边内容会被替换
// data 待写入的数据
// options 选项设置，可选
// callback 写入回调
// 返回值：undefined
```

```js
fs.writeFile('./a.txt','aa',err=> {
    // err 写入失败：错误对象；写入成功：null
    if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})
```

###### writeFileSync同步写入

```js
fs.writeFileSync(file, data[, options])

// file 文件名，文件不存在，自动创建;存在,里边内容会被替换
// data 待写入的数据
// options 选项设置，可选
// 返回值：undefined
```

```js
fs.writeFile('./a.txt','aa')
```

###### appendFile/appendFileSync追加写入

```js
fs.appendFile(file, data[, options], callback) // 异步
fs.appendFileSync(file, data[, options]) // 同步
// file 文件名
// data 待写入的数据
// options 选项设置，可选
// callback 写入回调
// 返回值：undefined
```

```js
fs.appendFile('./a.txt','aa',err=> {
    // err 写入失败：错误对象；写入成功：null
    if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})

fs.appendFileSync('./a.txt','aa')

// writeFile 也可以实现追加写入
fs.appendFile('./a.txt','aa',{
    flag: 'a'
},err=> {
    // err 写入失败：错误对象；写入成功：null
    if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})
```

###### createWriteStream流式写入

```js
fs.createWriteStream(path[, options])

// path 文件路径
// options 选项设置，可选
// 返回值：object
```

> 程序打开一个文件是需要消耗资源的，流式写入可以减少打开关闭文件的次数。
>
> 流式写入方式适用于大文件写入或者频繁写入的场景，writeFile适合于写入频率较低的场景

```js
// 创建写入流对象
const ws = fs.createWriteStream('./a.txt')
// write
ws.write('1\r\n')
ws.write('2\r\n')
ws.write('3')

// 关闭通道 写不写都可以
ws.close() // ws.end()
```

###### 写入文件的场景

> - 下载文件
> - 安装软件
> - 报错程序日志，如Git
> - 编辑器保存文件
> - 视频录制
>
> 当**需要持久化保存数据**的时候，就应该想到文件写入。

##### 文件读取

###### readFile

```js
// 异步读取
fs.readFile(path[, options], callback)

// path 文件路径
// options 选项配置 可选
// callback 回调函数
// 返回值： undefined
```

```js
fs.readFile('./a.txt',(err,data)=>{
    if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
    console.log(data) // 读取的是buffer
    console.log(data.toString())
})
```

###### readFileSync

```js
// 同步读取
fs.readFile(path[, options])

// path 文件路径
// options 选项配置 可选
// 返回值： data
```

```js
let data = fs.readFile('./a.txt')
console.log(data.toString())
```

###### createReadStream

```js
// 流式读取 一块一块的读取
fs.createReadStream(path[, options])

// path 文件路径
// options 选项配置 可选
```

```js
// 创建读取流对象
const rs = fs.createReadStream('./a.txt')
// 绑定data事件 每读取一块执行一次事件
rs.on('data', chunk => {
    // 每次读取64KB
    console.log(chunk.length) // 65536 字节 => 64KB
    console.log(chunk.toString()) // 读取视频等，是乱码
})
// end 可选事件
rs.on('end',()=>{
    console.log('读取完成')
})
```

###### 文件读取的场景

> - 电脑开机
> - 程序运行
> - 编辑器打开文件
> - 查看图片
> - 播放视频
> - 播放音乐
> - Git查看日志
> - 上传文件
> - 查看聊天记录

##### 练习

> **将文件复制一份**

```js
const fs = require('fs')
const process = require('process') // 查看内存使用量
# 方式一

let data = fs.readFileSync('./a.txt')
fs.writeFileSync('./a-1.txt',data)
// 查看内存使用情况
console.log(process.memoryUsage())

# 方式二

const rs = fs.createReadSteam('./a.txt')
const ws = fs.createWriteStream('./a-3.txt')

// 写入一
rs.on('data', chunk => {
    ws.write(chunk)
})

rs.on('end', ()=>{
    console.log(process.memoryUsage())
})

// 写入二
rs.pipe(ws)
```

##### 文件重命名与移动

```js
fs.rename(oldPath, newPath, callback)
fs.renameSync(oldPath, newPath)

// oldPath 文件当前的路径
// newPath 文件新的路径
// callback 操作后的回调
```

```js
// 重命名
fs.rename('./a.txt','./b.txt', err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})

// 移动
fs.rename('./a.txt','./text/b.txt', err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})

fs.renameSync('./a.txt','./b.txt')
```

##### 文件删除

```js
fs.unlink(path, callback)
fs.unlinkSync(path)
fs.rm(path, callback)
fs.rmSync(path)

// path 文件路径
// callback 操作后的回调
```

```js
fs.unlink('./a.txt', err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})

fs.unlinkSync('./a.txt')
```

#### 文件夹操作

##### 创建文件夹

```js
fs.mkdir(path[, options],callback)
fs.mkdirSync(path[, options])

// path : 文件夹路径
// options : 选项配置，可选
// callback : 操作后回调
```

```js
fs.mkdir( './h' , err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})
// 递归创建 a/b/c
fs.mkdir( './a/b/c' ,{
    recursive: true
}, err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})
```

> **文件夹折叠**
>
> ![1714113258545](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113258545.png)

##### 读取文件夹

```js
fs.readdir(path[, options],callback)
fs.readdirSync(path[, options])

// path : 文件夹路径
// options : 选项配置，可选
// callback : 操作后回调
```

```js
fs.readdir( './h' , (err, data) => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
    console.log(data)
})
```

##### 删除文件夹

```js
fs.rmdir(path[, options],callback)
fs.rmdirSync(path[, options])

// path : 文件夹路径
// options : 选项配置，可选
// callback : 操作后回调

# rmdir 将来会被移除，建议使用 rm
```

```js
// 删除文件夹 但是只能删除 空文件夹
fs.rmdir( './h' , err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})

// 递归删除 将文件夹下的所有文件都删除了
fs.rmdir( './h' ,{
    recursive: true
}, err => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
})
```

#### 查看资源状态

```js
// 查看资源的详细信息
fs.stat(path[, options], callback) // 异步
fs.statSync(path[, options])

// path : 文件夹的路径
// options : 选项配置，可选
// callback : 操作后的回调

// 返回值 : object
size : 文件体积
birthtime : 创建时间
mtime : 最后修改时间
isFile 检测是否为文件
isDirectory  检测是否为文件夹
... 
```

```js
fs.stat('./a.txt', (err, data) => {
     if(err) {
        console.log('失败')
        return
    }
    console.log('成功')
    console.log(data)
})

let data = fs.statSync('./a.txt')
data.isFile() // 是否为文件
data.isDirectory() // 是否为文件夹
```

#### 相对路径的问题

```js
# 相对路径

./a.txt // 当前目录下的a.txt
a.txt // 当前目录下的a.txt
../a.txt // 当前目录的上一级目录中的a.txt

# 绝对路径

D:/Program Files // windows系统下的绝对路径
/usr/bin  // Linux系统下的绝对路径
```

> 相对路径中的**当前目录**，指的是**命令行的工作目录**，而并非是文件的所在目录。
>
> 所以当命令行的工作目录与文件所在目录不一致时，会出现一些bug。

#### __dirname

```js
# __dirname 与 require 类似，都是Node.js 环境中的‘全局’变量

# __dirname 保存着 当前文件所在目录的绝对路径，可以使用__dirname与文件名凭借成绝对路径

let _path = __dirname + '/data.txt'
```

> 使用fs模块的时候，尽量使用**__dirname**将路径转化为绝对路径，这样可以避免相对路径产生的bug。

#### __filename

```js
# __dirname 与 require 类似，都是Node.js 环境中的‘全局’变量

# __dirname 保存着 当前文件的绝对路径.
```

### path模块

```js
// 导入path模块
const path = require('path')
```

| API           | 描述                                                     |
| ------------- | -------------------------------------------------------- |
| path.resolve  | 拼接规范的绝对路径                                       |
| path.sep      | 获取操作系统的路径分割符                                 |
| path.parse    | 解析路径并返回对象                                       |
| path.basename | 获取路径的基础名称                                       |
| path.dirname  | 获取路径的目录名                                         |
| path.extname  | 获取路径的扩展名                                         |
| path.join     | 用于链接路径，并且会自动转换当前系统路径的分隔符"/"或"\" |

#### path.sep

```js
path.sep
// window \;  Linux /
```

#### path.resolve

```js
# 语法：path.resolve([from ...], to)
```

**一：路径为空或__dirname** 

```js
演示当"path"路径为空时，得到结果是当前文件所在的绝对路径，类似 __dirname ；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve(''));
console.log(path.resolve(__dirname);
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path
```

**二：./开头或者没有字符时，加不加 ./ 都不影响路径的拼接**

```js
演示字符以 ./ 开头或者没有字符时，得到的结果是加不加 ./ 都不影响路径的拼接；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('a'));
console.log(path.resolve('a','b'));
console.log(path.resolve('c','b','a'));
console.log(path.resolve('./a'));
console.log(path.resolve('a','./b'));
console.log(path.resolve('./c','b','./a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\Berlin-Case\path\c\b\a
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\Berlin-Case\path\c\b\a
```

**三： / 开头，跳转到该盘的根路径，(在哪个盘运行就是哪个盘)**

```js
演示字符以 / 开头，path.resolve()的特点之一就是碰到"/"斜杆则会直接跳转到E盘的根路径
(在哪个盘运行就是哪个盘)，这里跟在终端输出cd /是一样的原理，也会跳转到E盘的根路径；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('/a'));
console.log(path.resolve('/a','b','c'));
console.log(path.resolve('a','/b','c'));
console.log(path.resolve('c','b','/a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\a
// 输出：E:\a\b\c
// 输出：E:\b\c
// 输出：E:\a
```

**四：../开头，先回上一层**

```js
演示字符以../开头，也就是上一层的意思，path.resolve()会把下个要拼接的"path"路径给覆盖掉；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('../'));
console.log(path.resolve('../a'));
console.log(path.resolve('../a','b'));
console.log(path.resolve('a','../b'));
console.log(path.resolve('c','b','../a'));
console.log(path.resolve('../c','b','a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\
// 输出：E:\Berlin-Case\a
// 输出：E:\Berlin-Case\a\b
// 输出：E:\Berlin-Case\path\b
// 输出：E:\Berlin-Case\path\c\a
// 输出：E:\Berlin-Case\c\b\a
```

**五：__dername**

```js
演示path.resolve()搭配__dername变量

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve(__dirname,'a'));
console.log(path.resolve('a','b',__dirname));
console.log(path.resolve(__dirname,'./a','b'));
console.log(path.resolve(__dirname,'/a','b'));
console.log(path.resolve(__dirname,'../a','b'));
console.log(path.resolve(__dirname,'a','../b'));
console.log(path.resolve('a','/b',__dirname));
 
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\a\b
// 输出：E:\Berlin-Case\a\b
// 输出：E:\Berlin-Case\path\b
// 输出：E:\Berlin-Case\path
 从案例五例子可以看出，__dirname变量需放在第一个，否则会覆盖在它之前的'path'路径，包括斜杠' / '，
还有个要注意的点，__dirname之后也不能出现' / '，不然也覆盖之前的路径；
```

**总结**

```js
path.resolve("./path")的特性有以下几点：

从右往左读取"path"路径,并开始拼接,本身就自带绝对路径参数 "to" ；
当"path"路径为空时，则会直接获取当前文件所在的绝对路径；
当遇到字符以 ./ 开头或者没有字符，则正常拼接，所以可省略不加字符；
当遇到字符以 / 开头，则不会拼接到前面的路径并以自身所在盘为根路径加以拼接；
当遇到字符以 ../ 开头，则会将下一个要拼接"path"路径给覆盖，然后继续往左拼接；
搭配__dirname时，需将放置第一位，且与 ' / ' 有互相覆盖的冲突；
```

#### path.join

```js
语法：path.join([path1][, path2][, ...])
```

**一：无或空时，得到的结果是" . "**

```js
演示当"path"路径为无或空时，得到的结果是" . ",只有传入__dirname的时候，才能得到绝对路径

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.join());
console.log(path.join(''));
console.log(path.join(__dirname));
// 输出：.
// 输出：.
// 输出：E:\Berlin-Case\path
```

**二：./ 开头或者 / 和没有字符**

```js
演示字符以 ./ 开头或者 / 和没有字符，得到的结果是加不加都不影响路径的拼接，
此时你应该发现跟path.resolve()的有所不同了，因为resolve()只会单纯的去拼接你写入的"path"路径，
而不会像path.resolve()那样用cd去运作；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join());
console.log(path.join('a'));
console.log(path.join('a','b'));
console.log(path.join('c','b','a'));
console.log(path.join('./a'));
console.log(path.join('a','./b'));
console.log(path.join('./c','b','./a'));
console.log(path.join('/a'));
console.log(path.join('a','/b'));
console.log(path.join('/c','b','/a'));
// 输出：.
// 输出：a
// 输出：a\b
// 输出：c\b\a
// 输出：a
// 输出：a\b
// 输出：c\b\a
// 输出：\a
// 输出：a\b
// 输出：\c\b\a
```

**三：../ 开头的字符**

```js
演示以 ../ 开头的字符，此时你会发现join()不仅是单纯的去拼接路径，而且也是从右到左去拼接的，
../之后还有"path"路径的话，也是会被覆盖掉；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join());
console.log(path.join('../'));
console.log(path.join('../a'));
console.log(path.join('../a','b'));
console.log(path.join('a','../b'));
console.log(path.join('c','b','../a'));
console.log(path.join('../c','b','a'));
 
// 输出：.
// 输出：..\
// 输出：..\a
// 输出：..\a\b
// 输出：b
// 输出：c\a
// 输出：..\c\b\a
```

**四：__dername**

```js
演示path.join()搭配__dername变量，为什么一定要把它放在第一位？

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join(__dirname,'a'));
console.log(path.join('a',__dirname));
 
// 输出：E:\Berlin-Case\path\a
// 输出：a\E:\Berlin-Case\path
看出区别了吗？对的，没错，join会不管对错，直接把你写入的路径都拼接到一块，
这也是为什么要放在第一位的原因，当然resolve()就没这种问题，接下来继续演示，与字符的搭配；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join(__dirname,'/a'));
console.log(path.join(__dirname,'./a'));
console.log(path.join(__dirname,'../'));
console.log(path.join(__dirname,'../a'));
console.log(path.join(__dirname,'../a','b'));
 
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\
// 输出：E:\Berlin-Case\a
// 输出：E:\Berlin-Case\a\b

以上示例可以看出，'/'  './' 这两个字符在path.join()的方法中是不起作用的，不加也是一样的效果，
只有 '../ ' 才有返回上级目录的作用，所以使用path.join()时，加个__dirname，
拼上你要的"path"路径即可；（不加会很麻烦哦）
```

**总结**

```js
在path.join()方法中，'/' 与 './' 一般情况下可以不用（特殊情况的拼接除外哈）；
在path.join()方法中，最好与__dirname变量搭配使用；
path.join()方法也是从右到左依次被解析排列组成路径的；
path.resolve与path.join的区别
结合上面两个方法的演示后的总结，它们之间的区别如下：

path.resolve()自带to参数，也就是当前输出文件的路径，而path.join()没有；
path.resolve()遇到 ' / ' 则会跳转到根目录(E:\),而path.join()则没效果；
path.resove()搭配__dirname变量使用时，就算__dirname在最右边，
resolve()会把左边的"path"路径给覆盖
```

### http模块

> Hypertext Transfer Protocol 超文本传输协议

#### http协议

![1714113282880](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113282880.png)

> 可以使用工具fiddler 截取报文
>
> <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113364738.png" alt="1714113364738" style="zoom:33%;" />
>
> ![1714113379209](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113379209.png)

#### http报文

##### 请求报文

> ![1714113648907](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113648907.png)
>
> 简化的请求报文
>
> ![1714113687228](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113687228.png)
>
> ![1714113749404](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113749404.png)

###### 请求行

> ![1714113780941](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714113780941.png)

```js
// 请求方法
get post put ...

// URL
Uniform Resource Locator 统一资源定位符
```

![1714120660691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120660691.png)

![1714120696341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120696341.png)

**http版本号**

| 版本号 | 发布时间 |
| ------ | -------- |
| 1.0    | 1996年   |
| 1.1    | 1999年   |
| 2      | 2015年   |
| 3      | 2018年   |

###### 请求头

> 请求头参考：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers
>
> ![1714120739636](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120739636.png)
>
> 由键值对组成，记录浏览器的信息等

###### 请求体

> 请求体的内容格式是非常灵活的，可以设置任意内容。

##### 响应报文

> ![1714120756254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120756254.png)

###### 响应行

> ![1714120773975](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120773975.png)

```js
# HTTP版本号
# 响应状态码
// 状态码参考：https://https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status
200 请求成功
404 找不到资源
500 服务器内部错误
1xx 信息响应
2xx 成功响应
3xx 重定向消息
4xx 客户端错误响应
5xx 服务端错误响应
# 响应状态的描述
```

###### 响应头

> 响应头参考：https://https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers
>
> ![1714120791681](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120791681.png)

###### 响应体

> ![1714120805896](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120805896.png)
>
> 响应体的内容格式是非常灵活的，常见的响应体格式有：
>
> - HTML
> - CSS
> - JavaScript
> - 图片
> - 视频
> - JSON

#### 网络基础

##### IP

> 用于寻找网络设备。
>
> IP也称为【IP地址】，本身是一个数组标识。
>
> 例如：192.168.1.3
>
> IP本质是32bit的二进制数字：
>
> ![1714120822188](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120822188.png)
>
> 但是不利于我们去使用，特别不方便，所以就进行了拆分，每8bit分组：
>
> ![1714120842115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120842115.png)
>
> 将二进制的数字转为十进制数字：
>
> ![1714120852124](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120852124.png)
>
> 在把十进制数字合在一起用 . 分开：
>
> ![1714120861741](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120861741.png)
>
> **作用：**IP用来标识网络中的设备，实现设备间通信

##### IP分类

> IP 32bit的二进制数字最多能表示 2的32次方个IP地址，但是IP还是不够用的。
>
> ![1714120880845](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120880845.png)
>
> 所以就有了共享IP，共享IP分为区域共享和家庭共享
>
> 以家庭共享为例，手机，电脑等连接路由器，形成一个网络，我们把这个网络叫**局域网**，而路由器给我们分配的IP地址，我们称为**局域网IP**，也叫 **私网IP**，在这个网络里边，设备是可以通信的。如果想和外部通信就需要连接互联网。在移动等办理业务后，我们的路由器就有了另一个IP，这个IP就是 **公网IP**，也叫 **广域网IP**，共享IP指的是共享公网IP。
>
> ![1714120906503](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120906503.png)

> **本地回环IP地址**：127.0.0.1，这个IP地址永远指向当前本机的

![1714120923068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714120923068.png)

> IP标准分类：https://zhuanlan.zhihu.com/p/193729352

##### 端口

> 端口：应用程序的数字标识。
>
> 一台现代计算机有65536个端口（0~65535）。
>
> 一个应用程序可以使用一个或多个端口。
>
> 作用：实现不同主机应用程序之间的通信。
>
> 图片

#### node.js创建http服务

```js
// 导入http模块
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;
// 创建服务对象
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

// 监听端口 启动服务
server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});

// 浏览器发送请求
```

> node ./xx.js  启动服务，浏览器发送http请求。
>
> ctrl+c 停止服务

