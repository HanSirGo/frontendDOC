## 打包vue项目

```html
npm run build
```

> 打包时，vite.config.ts 中 最好 配置 base:'./'
## Nginx安装
### 下载Nginx

> https://nginx.org/en/download.html

![1713772384818](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772384818.png)

**以windows版本为例。下载后，将其解压到本地任意目录：**

> 解压路径一定不要用中文，有中文会报错

![1713772370344](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772370344.png)
### 将打包文件放在nginx的html中
![1713772359440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772359440.png)

> 现在是将打包的dist中的文件取出放置在了nginx的html文件夹下，也可以将dist整体放在html中。

> 想要部署多个项目就需要在html文件夹下分别创建各自项目的文件夹，请看下方的部署多个项目

### 配置nginx服务器
使用vscode打开nginx文件；conf文件下的nginx.conf；只改动server：

```html
server {
       listen       8090;
       server_name  localhost;
		
		# / nginx的根目录
       location / {
           root   html;
           # 解决 404 vue使用history导致的404问题，若是hash那么请忽略
           try_files $uri $uri/ /index.html last
           index  index.html index.htm;
       }
    }
```
### 启动Nginx
双击 nginx.exe 启动Nginx；

> 或者在 nginx.exe 那个路径下打开cmd 运行start nginx，屏幕有窗口闪了一下，也启动Nginx

![1713772347181](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772347181.png)
### 打开任务管理器
Ctrl+Alt+Delete ；查看nginx是否启动
![1713772335112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772335112.png)

## 查看项目是否启动
### 打开浏览器

> 输入 http://localhost:8090
## Nginx 部分文件作用
### logs（日志）
#### error.log
> 输出一些错误信息

#### nginx.pid
> 这串数字代表的是Nginx在任务管理器  该线程 的 PID （上方的图可以看到PID）

![1713772318986](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713772318986.png)
### Nginx命令

>查看版本
>nginx -v
>开启nginx服务
>start nginx
>重启服务
>nginx -s reload
>关闭服务
>nginx -s stop

## 注意事项

>  1. 每次更改nginx.conf都要去任务管理器里结束进程，重新双击 nginx.exe 
>  2. 在修改nginx.conf文件时，每一行必须带份号 ; ,否者会报错 
>  3. 在nginx.exe那个路径，打开命令行执行 nginx -s reload 也可以重启Nginx

## 部署多个项目

> 一个端口配置N个项目
> 在html文件夹下分别创建各自项目的文件夹，将打包的文件放在其中

```html
server {
		#端口号
        listen       8888;
		#域名，没有买域名就用本地的不需要改
        server_name  localhost;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
		#默认项目
        location / {
			#这个是目录，从html开始找项目目录
            root   html/web;
            index  index.html index.htm;
        }
		#/company 是需要进行配置，如果是端口默认的就不需要配置
        location /company {
      		#注意后面需要加/
	  		alias html/renren/dist/;
            	index  index.html index.htm;
        }
}
```

> 注意！！这个/company 名称要和你在webpack或者是vue上配置的名称一致，不然一直给我显示找不到 (没验证)