# Nginx负载均衡

负载均衡：负载均衡分配，有效降低服务器压力。通过算法（轮询、IP哈希、随机）实现。

当出现我Nginx分配到的服务器上压力已经过满时，有个retry机制（重试机制）可以重新分配负载服务器

![1720266517645](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266517645.png)

## **配置反向代理**

关键字：proxy_pass，该关键字后面的规则不会进行匹配了

语法：`proxy_pass [主机、网址]或[一组服务器]`

注意：不支持https的proxy_pass的，因为需要和域名证书扯上关系

1）配置文件，写了二级域名

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
        server_name  test01.qiuyl.com;

        location / {
            proxy_pass http://www.atguigu.com;   # 这里二级域名写了www
            # root   /ww/qiuyl01;
            # index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }


}

[root@localhost nginx]# systemctl reload nginx
```

2）浏览器访问：

![1720266546336](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266546336.png)

3）配置文件，不写二级域名

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
        server_name  test01.qiuyl.com;

        location / {
            proxy_pass http://atguigu.com;
            # root   /ww/qiuyl01;
            # index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }


}
[root@localhost nginx]# systemctl reload nginx
```

4）浏览器访问

可以看到浏览器的URL地址的变化，已经192.168.200.7（Nginx服务器）请求的状态码为302（临时重定向状态码），所有咱们浏览器URL地址会变化。

该重定向是可以关闭的。

![1720266571137](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266571137.png)

## **负载均衡基本配置**

安装Docker，启动两个Nginx容器，并修改默认的index.html文件。修改宿主机的nginx.conf文件来实现负载均衡的效果。

1）安装Docker：docker-ce安装教程-阿里巴巴开源镜像站

2）启动两个Nginx容器，并修改index.html文件

```
[root@localhost ~]# docker pull nginx
[root@localhost ~]# docker run -dit --name web1 -p 81:80  nginx
00bce741b8a29c3ad68b861ed09239cb5f956746174650407fdb5f57d5030916
[root@localhost ~]# docker run -dit --name web2 -p 82:80  nginx
6643bbf0f2b8dea366f5b790dd2affed342c63a2b402afcd3738f1c34880f377
[root@localhost ~]# ll
total 1056
-rw-------. 1 root root    1647 Jan 28  2021 anaconda-ks.cfg
drwxr-xr-x  9 1001 1001     186 Apr 21 18:29 nginx-1.22.1
-rw-r--r--  1 root root 1073948 Apr 21 17:59 nginx-1.22.1.tar.gz
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                               NAMES
6643bbf0f2b8   nginx     "/docker-entrypoint.…"   5 seconds ago    Up 4 seconds    0.0.0.0:82->80/tcp, :::82->80/tcp   web2
00bce741b8a2   nginx     "/docker-entrypoint.…"   15 seconds ago   Up 13 seconds   0.0.0.0:81->80/tcp, :::81->80/tcp   web1

[root@localhost ~]# docker exec -it  web1 bash
root@00bce741b8a2:/# echo web1 > /usr/share/nginx/html/index.html
root@00bce741b8a2:/# exit

[root@localhost ~]# docker exec -it  web2 bash
root@6643bbf0f2b8:/# echo web2 > /usr/share/nginx/html/index.html
root@6643bbf0f2b8:/# exit
exit

[root@localhost ~]# curl 127.0.0.1:81
web1
[root@localhost ~]# curl 127.0.0.1:82
web2
```

3）配置宿主机的nginx.conf文件，实现负载均衡

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
    
 # 通过upstream和proxy_pass来实现负载均衡
    upstream web {
      server localhost:81;
      server localhost:82;
    }

    server {
        listen       80;
        server_name  test01.qiuyl.com;

        location / {
            proxy_pass http://web; # 跳转到upstream区域中
            # root   /ww/qiuyl01;
            # index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }


}

[root@localhost conf]# systemctl reload nginx
```

4）浏览器访问

每次刷新浏览器，可以看到不同的内容。对应到后端不同的机器（这里是容器）。

![1720266606789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266606789.png)

## **负载均衡策略**

默认采用轮询方式

![1720266618142](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266618142.png)

下列几种策略中down和backup不常用，因为加入策略时需要重新reload使配置更新（reload会使应用短时间内中断），这种手动效率不行出现意外情况不能第一时间处理。一般采用动态方式。

1）weight（权重）

应用场景：每台机器的配置可能不一，有的配置高，有的配置低。配置高的咱们需要多处理一些请求负载，于是就有了权重（weight）。

![1720266884115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266884115.png)

![1720266897355](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266897355.png)

2）down（下线）

应用场景：down可以使某个负载均衡节点不参与负载均衡了

![1720266910116](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266910116.png)

3）backup（备用服务器）

应用场景：其它负载节点实在是不可用了，才会使请求访问到标有backup的机器上。

![1720266920432](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266920432.png)

## **其它负责均衡策略**

下列几种策略生产环境也都不会使用。主要原因是没法动态的上下限服务器，因为需要reload。一般使用 lua 脚本编程去管理动态上下线。

> ❝
>
> 默认使用轮询的负载均衡策略，有个问题就是**不能保持会话**。
>
> **生产环境一般采用 lua 脚本、Token会话保持（配合轮询），来保持会话**

![1720266934107](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720266934107.png)

ip_hash：已经不太适应现在这个移动互联网时代了。因为咱们手机客户端移动时IP是会动态变动的，有的时候信号是好是坏 IP 也会变化。所以这种方式不太合适。没法保持会话

least_conn：使后端会话连接数保持一个均衡。但是一般会话连接数比较少的原因是咱们给某台机器的权重比较低，所以连接少。而且也不适合保证服务的上下线

> ❝
>
> 1. 因为我们增加一台新的机器时，连接会话数最少，那么后面是不是所有的连接都会在这台机器上。
> 2. 配置这个策略需要reload，reload的原理是需要将老的线程kill掉。所以咱们后端服务器的会话数都变为0了

url_hash：每个URL地址会被Nginx解析为一个hash值。我们知道注册、登录、首页的url地址是不一致的，如果登录后访问其它页面地址变化了那么url_hash值也会变化，有可能会转发在不同的服务器。

fair：需要下载第三方插件，根据后端服务器的响应时间转发请求。也不合理。会出现流量倾斜的情况。因为有可能因为网络原因导致

