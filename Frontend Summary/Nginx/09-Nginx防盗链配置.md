# Nginx防盗链配置

防盗链本质是存在服务器上的资源只能由服务器本身进行访问，其他引用的资源不能访问

**如何实现？**

浏览器的第二次请求可以看到Referer数据来源，在Request Headers里面是有Referer，表示之前第一次页面的资源是从这里过来的，后续的资源也将从这里获取。

所以，我们可以根据判断该Referer值来判断是否具有访问权限。

![1720267688931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267688931.png)

# 操作案例

## 站点准备

准备两台机器，第一台机器能正常访问自己网站。第二台机器配置反向代理到第一台机器，配置防盗链后不能访问网站

| 主机IP         | 备注                                                         |
| :------------- | :----------------------------------------------------------- |
| 192.168.200.7  | 首先通过容器部署Tomcat，其次通过Nginx代理到自身的8080上（在之前Nginx篇章有介绍） |
| 192.168.200.10 | Nginx代理到192.168.200.7主机上                               |

1）192.168.200.7（第一台主机）

站点环境

```
[root@localhost conf]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED       STATUS          PORTS                                       NAMES
332a1fc3630c   tomcat    "catalina.sh run"        12 days ago   Up 32 minutes   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   tomcat
```

配置

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
        server_name  localhost;

        location / {
            proxy_pass http://192.168.200.7:8080; # 代理到容器部署的Tomcat
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
[root@localhost nginx]# systemctl reload nginx
```

浏览器访问

![1720267718210](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267718210.png)

2）192.168.200.10（第二台主机）配置

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
        server_name  localhost;

        location / {
            proxy_pass http://192.168.200.7;   # 代理到第一台主机
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
[root@localhost nginx]# systemctl reload nginx
```

浏览器访问

![1720267736373](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267736373.png)

## 防盗链配置

Nginx 的 `valid_referers` 指令用于检查HTTP请求中的`Referer`头信息，以确定请求是否源自允许的来源。这个指令可以帮助实施防盗链策略，保护网站资源不被未经授权的第三方网站直接链接。

```
valid_referers [none|blocked|server_names|string ...];
```

参数说明：

- `none`: 表示请求没有`Referer`头信息
- `blocked`: 表示`Referer`头信息被防火墙或代理服务器屏蔽或修改
- `server_names`: 一个或多个主机名，可以是具体的域名或者IP地址，也可以使用通配符 `*`。如：`valid_referers none server_names example.com *.example.org;`
- `string`: 允许直接指定一个字符串作为合法的referer值进行匹配，也可以是正则表达式（需使用`~*`或`~`前缀来标记）

1）防盗链配置解释

```
valid_referers 192.168.200.7;
if ($invalid_referer) {  # 注意这里if后要加空格
    return 403; # 返回错误码403
}
```

- `valid_referers 192.168.200.7;` 这一行指定了被认为是合法（valid）的referer来源。在这个例子中，只有当请求是从`192.168.200.7`这个IP地址发起的，才被视为合法的referer。你可以添加多个IP或域名，用空格分隔，例如 `valid_referers 192.168.200.7 example.com;`。
- `if ($invalid_referer) {` 这里使用了一个条件判断语句，`$invalid_referer`是一个内部变量，由Nginx自动设置。当请求的referer头不符合`valid_referers`列表中的任何一个时，`$invalid_referer`会被设置为一个非空值，表示这是一个无效的referer。
- `return 403;` 当`$invalid_referer`为真，即请求的referer不在允许的列表内时（浏览器侧），Nginx会返回HTTP状态码403（Forbidden），意味着服务器理解客户端的请求，但是拒绝执行此请求。这通常用于阻止未经授权的外部网站直接链接到你的资源，保护你的带宽和内容免遭盗用。

一句话总结：这段配置的作用是检查HTTP请求头中的Referer字段，如果请求不是从指定的IP地址（在这个例子中是`192.168.200.7`）发起的，则拒绝该请求，返回403 Forbidden错误。

2）配置nginx.conf

只有valid_referers列表中地址，才能访问静态数据

```
[root@localhost nginx]# cat conf/nginx.conf

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
        server_name  localhost;

        location / {
            proxy_pass http://192.168.200.7:8080;
        }
  
        location ~*/(js|css|fonts|images) {
            valid_referers 192.168.200.7;
            if ($invalid_referer) {  # 注意这里if后要加空格
                return 403; # 返回错误码403
            }
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

3）浏览器访问

第一台主机，可以看到第一台主机的数据来源是http://192.168.200.7

![1720267763835](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267763835.png)

第二台主机，可以看到由于第一台主机配置防盗链之后，所有样式都无法访问

![1720267777090](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267777090.png)

4）none配置

如果上面配置后，不但引用访问不能访问，直接访问站点资源也是不行的。如果需要直接访问可行，需要配置none

![1720267801780](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267801780.png)

直接访问静态资源图片失败

配置none

```
location ~*/(js|css|fonts|images) {
            valid_referers none 192.168.200.7;  
            if ($invalid_referer) {  # 注意这里if后要加空格
                return 403; ## 返回错误码
            }
            root html;
            index index.html index.htm;
        }
```

直接访问静态资源图片

![1720267823773](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267823773.png)

直接访问静态资源图片成功

## Curl工具测试

`-I` 表示：只获取HTTP头部信息

```
[root@localhost nginx]#  curl  -I 192.168.200.7/images/bg.jpg
HTTP/1.1 200 OK # 状态码
Server: nginx/1.22.1
Date: Tue, 14 May 2024 13:13:40 GMT
Content-Type: image/jpeg
Content-Length: 519806
Last-Modified: Fri, 03 May 2024 07:50:08 GMT
Connection: keep-alive
ETag: "66349730-7ee7e"
Accept-Ranges: bytes
```

`-e` 表示：设置HTTP请求中的`Referer`头字段，可用于测试防盗链

```
[root@localhost nginx]#  curl -e "https://qiuyl.com" -I 192.168.200.7/images/bg.jpg
HTTP/1.1 403 Forbidden # 状态码
Server: nginx/1.22.1
Date: Tue, 14 May 2024 13:13:43 GMT
Content-Type: text/html
Content-Length: 153
Connection: keep-alive
```

## 防盗链资源返回页面或图片

1）盗链返回页面

```
[root@localhost html]# cat ../conf/nginx.conf

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
        server_name  localhost;

        location / {
            proxy_pass http://192.168.200.7:8080;
        }

        location ~*/(js|css|fonts|images) {
            valid_referers  192.168.200.7;
            if ($invalid_referer) {  # 注意这里if后要加空格
                return 403; ## 返回错误码
            }
            root html;
            index index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        error_page   403  /40x.html;   # 如果状态码返回为403，则跳转至40x.html文件页面
        location = /40x.html {
            root   html;
        }
    }
}
```

在html文件目录创建40x.html

```
[root@localhost html]# cat 40x.html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Error</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>An error occurred.</h1>
<p>非法访问.</p>
<p>If you are the system administrator of this resource then you should check
the error log for details.</p>
<p><em>Faithfully yours, nginx.</em></p>
</body>
</html>
```

浏览器访问

![1720267858425](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267858425.png)

2）盗链返回图片

注意：需要提前准备好`x.png`图片，并上传至`/usr/local/nginx/html/images`目录（根据自身环境而定）

```
location ~*/(js|css|fonts|images) {
            valid_referers  192.168.200.7;
            if ($invalid_referer) {  
                rewrite ^/ /images/x.png break;   # 匹配所有路径，跳转到x.png图片
                # return 403; ## 返回错误码
            }
            root html;
            index index.html index.htm;
        }
```

浏览器访问

![1720267875515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267875515.png)

