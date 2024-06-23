# 前端使用JSZip压缩包库实现文件打包下载

在平常的开发中, 经常会遇到一些zip打包下载, 比如用户上传了很多文件, 在审核的时候, 审核人员需要对其文件进行下载, 查看是否合规, 一张一张下载是很麻烦的, 所以, 我们今天就来实现一下zip打包下载

首先, 我们来定义一个数组, 数组中存放两个多个url

![1719047817478](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047817478.png)

两个在线文件我们就定义好了

我们思考一下实现逻辑, 首先点击按钮进行zip下载的时候, 我们可以发起请求获取文件内容, 然后, 将文件内容push到一个数组中, 最后引入jsZip并创建实例, 将文件依次添加到zip文件当中, 生成一个完整的zip文件, 最后创建一个a标签, 模拟点击进行下载

现在我们大概逻辑有了, 先写下载部分, 我们封装一个请求函数, 传入一个url地址, 进行下载, 并将下载结果返回出去

![1719047834720](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047834720.png)

在函数外面再写一个downLoadFile方法, 我们给btn绑定一个点击事件, 点击之后, 我们就调用downLoadFile函数

![1719047855227](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047855227.png)

看一下, 请求是否发起

![1719047872182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047872182.png)

随后, 我们再看一下打印结果

![1719047885310](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047885310.png)

这个blob对象, 我们就可以获取到了, 接下来我们将对象进行返回

![1719047904407](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047904407.png)

我们再打印一下获取到的数组是什么样子

![1719047920167](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047920167.png)

这样, 我们算完成了一大半了, 随后, 需要我们引入JSZip库

建议使用jszip-sync来替代jszip, 因为在生产环境的话, zip无法下载, 就是因为在生产环境zip.generateAsync不执行

jszip-sync只是在整体打包zip的外面加了一个zip.sync, 里面有一个回调函数, 将jszip的所有写法进行包裹即可, 我们可以看一下写法

![1719047943284](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047943284.png)

我引入了一个cdn, 大家可以通过npm进行依赖安装

```
<script src="https://cdn.bootcdn.net/ajax/libs/jszip/3.10.1/jszip.js"></script>
```

引入之后, 就可以创建zip实例

```
const zip = new JSZip()
```

上面我们已经获取到包含blob对象和name的数组了, 并且每一项都是个promise

![1719047975009](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047975009.png)

后面, 我们需要将数组中的每一项都添加到zip文件中, 需要使用zip提供的file API

![1719047990222](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719047990222.png)

随后使用zip的generateAsync生成完整的zip文件

![1719048004761](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048004761.png)

这里, 我们需要看文档了

> https://stuk.github.io/jszip/documentation/api_jszip/generate_async.html

type选项主要是返回的zip文件类型, compression选项是默认的文件压缩方式, 主要有两个选项, STORE表示无压缩, 如果设置为DEFLATE, 我们可以通过compressionOptions: { level: 6 }来指定压缩级别, 范围是1-9的任意级别, 1表示最快速度, 9表示最佳压缩

生成zip之后, 我们可以模拟a标签点击下载

![1719048020899](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048020899.png)

接下来, 我们看看效果

![1719048036644](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048036644.png)

貌似, 下载之后格式好像有点错误

![1719048056475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048056475.png)

哦对, name后面没有后缀名, 排查一下, 我截取的时候, 貌似截取到斜杠(/)之后, 点(.)之前, 正好没有后缀名

![1719048073828](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048073828.png)

改完之后再看看效果

![1719048094656](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719048094656.png)

打包zip文件下载, 我们就讲解完了

大家切记如果要部署到服务器, 需要使用JSZip-sync将JSZip替换掉

将JSZip的书写逻辑, 放到zip.sync的回调函数里面即可