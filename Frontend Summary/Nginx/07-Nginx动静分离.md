# Nginx动静分离

Nginx动静分离的原理主要是基于Nginx作为反向代理服务器的角色，利用其高效处理静态内容的能力，将网站的动态请求和静态请求分发到不同的后端服务器或处理器上，以此来优化资源的加载速度和提升整个系统的性能。

具体来说，动静分离的工作机制包含以下几个关键点：

1. **URL匹配**：Nginx通过配置文件中的`location`指令对客户端的请求URL进行匹配。可以根据文件扩展名或者路径前缀来区分静态资源请求和动态资源请求。
2. **静态内容直接处理**：当Nginx识别出请求的是静态资源（如HTML、CSS、JavaScript、图片等），它会直接从静态文件存储位置（通常是本地文件系统或者配置的缓存目录）读取并返回给客户端，而无需将请求转发给后端的应用服务器（如Tomcat、Apache等）。Nginx处理静态内容的效率非常高，能显著减少延迟。
3. **动态内容转发**：对于动态内容请求（如PHP、JSP等脚本生成的页面），Nginx则将其转发（代理Pass）到后端的应用服务器进行处理。处理完成后，应用服务器将结果返回给Nginx，Nginx再将响应转发给客户端。
4. **负载均衡**：在动静分离的同时，Nginx还可以作为负载均衡器，根据设定的策略将动态请求分发到多个应用服务器，以进一步提高系统的可用性和扩展性。
5. **缓存策略**：Nginx还可以配置缓存策略，对频繁访问的静态资源进行缓存，减少对后端存储的访问，提高响应速度。

比如，咱们接下来要做的案例是：

> 将静态资源存放在Nginx侧，动态资源存放在Tomcat服务器侧。静态资源通过Nginx的`location`指令来实现路径匹配，其它请求路径都将在Tomcat服务器侧处理。

![1720267173038](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267173038.png)

1）Docker部署Tomcat

```
[root@localhost ~]# docker pull tomcat
[root@localhost ~]# docker run -dit --name tomcat -p 8080:8080 tomcat
[root@localhost ~]# netstat -ntplu
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      4924/docker-proxy
...
...
```

浏览器访问

> 注意：如果无法访问到Tomcat，可以检查一下Tomcat的首页文件是否存在

![1720267192150](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267192150.png)

```
[root@localhost ~]# docker exec -it tomcat bash
root@332a1fc3630c:/usr/local/tomcat# ls
bin           conf             lib      logs            NOTICE     RELEASE-NOTES  temp     webapps.dist
BUILDING.txt  CONTRIBUTING.md  LICENSE  native-jni-lib  README.md  RUNNING.txt    webapps  work
```

进入 webapps 目录，发现里面是空的（Tomcat 默认的欢迎页面实际上放在的路径应该是：`webapps/ROOT/WEB-INF`）

这里看见 webapps.dist 目录（备份或者示例目录），将该目录下的所有文件目录都复制一份到 webapps 目录下即可

```
root@332a1fc3630c:/usr/local/tomcat# ls -al webapps
total 0
drwxr-xr-x 2 root root  6 Apr 26 02:28 .
drwxr-xr-x 1 root root 30 Apr 26 02:28 ..
root@332a1fc3630c:/usr/local/tomcat# ls -al webapps.dist/
total 4
drwxr-xr-x  7 root root   81 Apr 16 12:17 .
drwxr-xr-x  1 root root   30 Apr 26 02:28 ..
drwxr-xr-x 16 root root 4096 Apr 26 02:28 docs
drwxr-xr-x  7 root root   99 Apr 26 02:28 examples
drwxr-xr-x  6 root root   79 Apr 26 02:28 host-manager
drwxr-xr-x  6 root root  114 Apr 26 02:28 manager
drwxr-xr-x  3 root root  223 Apr 26 02:28 ROOT
root@332a1fc3630c:/usr/local/tomcat# cp -rf webapps.dist/* webapps
```

再次访问

![1720267248835](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267248835.png)

2）准备项目

> 微信搜一搜公众号**秋意零**，关注回复 charts 获取示例项目

3）部署项目

将项目上传到宿主机并复制到容器中

```
[root@localhost ~]# docker cp charts-project-master/ tomcat:/usr/local/tomcat
Successfully copied 2.07MB to tomcat:/usr/local/tomcat
```

进入Tomcat容器将项目文件cp到webapps/ROOT目录下

```
[root@localhost ~]# docker exec -it tomcat bash
root@332a1fc3630c:/usr/local/tomcat# cp -rf charts-project-master/* webapps/ROOT/
```

浏览器访问

![1720267273908](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267273908.png)

接着这里我们可以打开浏览器开发者模式，可以看到我们主页面的静态资源文件（css、js）路径保存位置，一般都是相对路径。所以我们只需要将这里访问的静态资源地址，代理到访问Nginx的静态资源地址即可。

![1720267284655](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267284655.png)

4）配置动静分离

将Tomcat容器中charts项目的静态资源目录删除或重命名

```
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT# mv js js.bak
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT# mv images/ images.bak
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT# mv fonts fonts.bak
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT# mv css/ css.bak
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT#
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT#
root@332a1fc3630c:/usr/local/tomcat/webapps/ROOT# ls -al
total 196
drwxr-xr-x 7 root root  4096 May  3 06:50 .
drwxr-xr-x 1 root root    81 May  3 05:58 ..
-rw-r--r-- 1 root root 27235 May  3 05:58 asf-logo-wide.svg
-rw-r--r-- 1 root root   713 May  3 05:58 bg-button.png
-rw-r--r-- 1 root root  1918 May  3 05:58 bg-middle.png
-rw-r--r-- 1 root root  1401 May  3 05:58 bg-nav.png
-rw-r--r-- 1 root root  3103 May  3 05:58 bg-upper.png
drwxr-xr-x 2 root root    23 May  3 06:44 css.bak
-rw-r--r-- 1 root root 21630 May  3 05:58 favicon.ico
drwxr-xr-x 2 root root   102 May  3 06:44 fonts.bak
drwxr-xr-x 2 root root   104 May  3 06:44 images.bak
-rw-r--r-- 1 root root 20036 May  3 06:44 index.html
-rw-r--r-- 1 root root 12241 May  3 05:58 index.jsp
drwxr-xr-x 2 root root   116 May  3 06:44 js.bak
-rw-r--r-- 1 root root   837 May  3 06:44 README.en.md
-rw-r--r-- 1 root root   926 May  3 06:44 README.md
-rw-r--r-- 1 root root  6776 May  3 05:58 RELEASE-NOTES.txt
-rw-r--r-- 1 root root  5584 May  3 05:58 tomcat.css
-rw-r--r-- 1 root root 67795 May  3 05:58 tomcat.svg
drwxr-xr-x 2 root root    21 May  3 06:42 WEB-INF
```

浏览器访问，可以看到目前页面没有任何图片或样式

![1720267312595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267312595.png)

在宿主机侧，将charts-project项目的`css、js、images、fonts`目录cp到Nginx的静态目录（Nginx根目录）中

配置nginx.conf

> 目的：只要请求是静态资源（js、images、css、fonts）的都会在**Nginx的root指令指定的目录查找**，而不是去Tomcat服务器端查找

```
[root@localhost conf]# cat nginx.conf

worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    upstream web {
      #server localhost:81;
      #server localhost:82;
       server localhost:8080;
    }

    server {
        listen       80;
        server_name  test01.qiuyl.com;

        location / {
            proxy_pass http://web;
            # root   /ww/qiuyl01;
            # index  index.html index.htm;
        }
	    
        location /images {
            root html;
            index index.html index.htm;
        }

        location /js {
            root html;
            index index.html index.htm;
        }

        location /css {
            root html;
            index index.html index.htm;
        }

        location /fonts {
            root html;
            index index.html index.htm;
        }


        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

浏览器访问，可以看到本地资源也是正常访问到的，页面也正常显示

![1720267330673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267330673.png)

## 使用正则配置动静分离

这里有个问题：这样去写location可能导致location比较多

![1720267344882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267344882.png)

Nginx配置里面的正则：区分大小写的正则`~`开头，不区分大小写的正则`~*`开头

`(js|img|css)`：| 符号表示或者，这里意思就是js、img、css三个目录都能匹配到

> 详情正则表达式规则请看 [Nginx ServerName详解(系列篇04)](https://mp.weixin.qq.com/s?__biz=Mzg3OTc2OTE3NA==&mid=2247484939&idx=1&sn=db28e75a65e6492d364cc5e13018f698&chksm=cf7e2532f809ac24d25332abc34dd7f137d962839f1ddea572f57cdc80a6aa733fa39b87edb6&scene=21#wechat_redirect)

![1720267359843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267359843.png)

```
[root@localhost conf]# cat nginx.conf

worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;

    server {
        listen       80;
        server_name  test01.qiuyl.com;

        location / {
            proxy_pass http://web;
        }

        location ~*/(js|css|fonts|images) {
            root html;
            index index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

