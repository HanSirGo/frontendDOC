## 使用Nginx部署element-ui官方文档
> 其他文档也可以依照下面的步骤部署，只讲打包之前，使用Nginx已经在-------使用nginx部署vue3项目（本地） 文章中写了
### 下载 element-ui
> Gitee地址: https://gitee.com/ElemeFE/element.git
> GitHub地址: https://github.com/ElemeFE/element.git
> elementui的源码中包含了elementui官方文档的源码

![1713772457066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772457066.png)
下载后解压

###  打包
查看package.json文件, 找到scripts节点, 此节点用于指定脚本命令, 供npm直接调用. 在script中找到带有deploy的节点, 会发现有两个命令deploy:build和deploy:extension, 我们要用到的就是这个deploy:build命令, 从名字可以看出,这个命令是用来部署构建的, 运行此命令

> 先安装依赖 
> `npm install`  
> 依赖安装完成后, 运行构建部署命令 
> `npm run deploy:build`
> 打包完成后在element\examples目录下可以发现生成了一个文件夹element-ui,这个就是我们需要的项目
### 部署
> 将 dist 文件 放置在nginx中 
> 参考 ----[使用nginx部署vue3项目（本地）](https://editor.csdn.net/md/?articleId=134896140)
### 某些资源找不到的问题
![1713772444248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772444248.png)
> 下载无法找到的资源到本地, 并在index.html中关联(资源地址在控制台报错时其实就显示了, 直接复制地址贴到下载器下载下来就行)

> 下载完成后, 在index.html中关联资源地址 (我本地将下载的JS放在了element-ui文件夹下的static\js\文件夹中, css放在了static\css\文件夹中, 这样后面拷贝就可以直接将所有文件都拷贝走,哪里需要就直接发布一下就能用了)
#### 需要修改位置如下：
![1713772429530](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772429530.png)
### 查看文档
> 运行Nginx，这样，内网（局域网）别人 想查看文档时，输入 （部署文档的电脑）IP + 路径

> 最好还是部署到 服务器上 服务器的Nginx上