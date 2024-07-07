# Nginx虚拟主机

Nginx 虚拟主机允许在一个单一的物理服务器上运行多个独立的网站或应用，每个都具有自己的域名、文档根目录、日志文件等，仿佛它们各自运行在独立的服务器上。Nginx 支持三种主要类型的虚拟主机配置：基于端口、基于域名和基于IP地址。

## **虚拟主机配置**

### **环境准备**

`C:\Windows\System32\drivers\etc` Win系统中配置hosts

![1720264604501](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264604501.png)

右击hosts文件查看属性，检查登录的Win系统用户是否有修改权限，没有则编辑修改权限即可

![1720264617047](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264617047.png)

编辑hosts映射

![1720264636641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264636641.png)

创建虚拟主机站点目录：qiuyl01和qiuyl02

```
[root@localhost ~]# mkdir -p  /www/qiuyl0{1,2}
[root@localhost ~]# cd /www/
[root@localhost www]# ll
total 0
drwxr-xr-x 2 root root 6 Apr 22 17:11 qiuyl01
drwxr-xr-x 2 root root 6 Apr 22 17:11 qiuyl02
[root@localhost www]# echo "this is a qiuyl01." > qiuyl01/index.html
[root@localhost www]# echo "this is a qiuyl02." > qiuyl02/index.html
[root@localhost www]# ll qiuyl01/
total 4
-rw-r--r-- 1 root root 19 Apr 22 17:13 index.html
[root@localhost www]# ll qiuyl02/
total 4
-rw-r--r-- 1 root root 19 Apr 22 17:13 index.html
```

### **端口虚拟主机**

注意：端口虚拟主机，端口不能一致

```
[root@localhost conf]# vi nginx.conf

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
        listen       81;
        server_name  localhost;

        location / {
            root   /www/qiuyl01; # 虚拟主机站点目录
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }
 
 # 添加一个虚拟主机
    server {
        listen       82;
        server_name  localhost;

        location / {
            root   /www/qiuyl02; # 虚拟主机站点目录
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}

[root@localhost conf]# systemctl reload nginx
```

浏览器访问

![1720264667519](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264667519.png)

![1720264677906](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264677906.png)

### **域名虚拟主机**

注意：域名虚拟主机，端口可一致

修改本地hosts映射

![1720264692609](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264692609.png)

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
        server_name  test01.qiuyl.com; # 虚拟主机域名

        location / {
            root   /www/qiuyl01;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

    server {
        listen       80;
        server_name  test02.qiuyl.com; # 虚拟主机域名

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
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

![1720264716307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264716307.png)

![1720264727031](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264727031.png)

### **IP虚拟主机**

在ens33网卡上添加一个IP

```
[root@localhost ~]# ip addr add 192.168.200.11/24 dev ens33
[root@localhost ~]#
[root@localhost ~]# ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.200.7/24 brd 192.168.200.255 scope global noprefixroute ens33
       valid_lft forever preferred_lft forever
    inet 192.168.200.11/24 scope global secondary ens33
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever

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
        # IP虚拟主机
        listen       192.168.200.7:80;
        server_name  localhost;

        location / {
            root   /www/qiuyl01;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

    server {
        # IP虚拟主机
        listen       192.168.200.11:80;
        server_name  localhost;

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
}

[root@localhost nginx]# systemctl reload nginx
```

浏览器访问不同IP返回不同页面

![1720264763423](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264763423.png)

![1720264773315](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264773315.png)