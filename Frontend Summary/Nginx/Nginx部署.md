## 高级前端应该掌握的Nginx相关知识

现在服务器安装Nginx很简单，一般只需要两行命令即可，安装完成后，启动服务。

```
	# 安装nginx
    yum install -y nginx
    # 启动nginx
    systemctl start nginx
    # 查看nginx运行状态
    systemctl status nginx
```

![1726294489429](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294489429.png)



当我们看到下图，说明你的Nginx运行正常，此时Nginx会启动服务，默认80端口。此时如果我们的服务器外网防火墙`80`端口开启，那么访问外网IP，就能看到Nginx启动的服务

![1726294500161](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294500161.png)



Nginx的配置文件，一般位于`/etc/nginx`目录下，具体内容如下：

![1726294509332](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294509332.png)



我们基本只需要关注文件`nginx.conf`和`conf.d`目录，下面是一份`nginx.conf`配置文件，不懂也不要怕，基本都不需要改动，默认80服务已经开启了。

```
    user nginx; # nginx进行运行的用户
    error_log /var/log/nginx/error.log; # 错误日志

    http {
        log_format main ...; # nginx日志格式
        access_log /var/log/nginx/access.log main; # 日志位置
        
        # 引入的nginx配置文件，可以将server放在该目录下，方便管理
        include /etc/nginx/conf.d/*.conf; 
        # 一个nginx服务一个server
        server {
            listen 80; # 服务启动的端口
            server_name _; # 服务域名或IP
            root /usr/share/nginx/html; # 服务指向的文件地址
            
            error_page 404 /404.html; # 找不到资源重定向到404页面
            location = /40x.html {};
            
            error_page 500 502 503 504 /50x.html; # 系统错误重定向50x页面
            location = /50x.html {};
        }
        # server {
        #    listen 443; # 支持https协议
        #    server_name _;
        #    root /usr/share/nginx/html;
        #    ...
        # }
    }
```

我们可以看到该文件分成了多层

- 第一层：user、error_log、http
- 第二层：log_format、access_log、include、server

在http下可以有多个`Server`，启动多个服务，但如果都写在一个文件里面，文件就越来越大了，那么为了便于管理多个服务，我们要对`nginx.conf`进行拆分。

conf.d目录下一般是空的，我们新建文件 web.conf或者任意命名的以.conf结尾的文件即可被Nginx使用，内容为：

```
    server {
        listen 80; 
        server_name _; 
        root /usr/share/nginx/html; 

        error_page 404 /404.html; 
        location = /40x.html {};

        error_page 500 502 503 504 /50x.html; 
        location = /50x.html {};
    }
```

由于这里使用了80端口，之前nginx.conf文件server中listen为80的可以删除了。

此时nginx.conf中的文件内容为：

```
    user nginx; # nginx进行运行的用户
    error_log /var/log/nginx/error.log; # 错误日志

    http {
        log_format main ...; # nginx日志格式
        access_log /var/log/nginx/access.log main; # 日志位置
        
        # 引入的nginx配置文件，可以将server放在该目录下，方便管理
        include /etc/nginx/conf.d/*.conf; 
    }
```

`include /etc/nginx/conf.d/*.conf; `我们看到这一行语句发现，include帮助我们引用conf.d下以.conf结尾的配置文件。

完成后执行nginx指令

```
    # 检查nginx配置文件是否正确，如果错误会提示具体的错误信息
    nginx -t
    # 重新启动nginx服务
    nginx -s reload
```

![1726294549591](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294549591.png)



观察日志，此时发现Nginx就重新启动了，读取的是新的配置文件。

**其他操作nginx的指令**

```
    nginx -s stop
    nginx -s start
```

## **二、启动一个简单的Nginx服务**

- 一台服务器或PC，安装并启动Nginx服务
- `/data/web`两个html文件`index.html`, `about.html`

1、`index.html`或`about.html`

```
    <!DOCTYPE html>
    <html>
        <head>
           <meta charset="utf-8">
           <title>nginx</title>
        </head>
        <body>
            通过nginx部署的第一个服务
        </body>
    </html>
```

2、修改`/etc/nginx/conf.d/web.conf`

```
    server { 
        listen 80; 
        server_name localhost; 
        root /data/web; 
        index index.html; 
    }
```

执行`nginx -t`确认配置文件修改没问题，再执行`nginx -s reload`重启Nginx，此时我们访问外网IP（默认80端口，下面默认都是访问80端口），可以看到

![1726294581061](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294581061.png)



这样我们的静态资源文件就部署好了，通过url访问资源：

- `http://xxxx/index.html`
- `http://xxxx/about.html`

## **三、部署单页面应用**

我们快速创建一个CRA单页面应用，修改App.js文件，这里使用`react-router-dom@6`

### **1、Hash模式**

```
    import { HashRouter, Routes, Route, Link } from "react-router-dom";
    import './App.css';

    function App() {
      return (
        <div className="App">
            <HashRouter basename="/">
              <div style={{marginBottom: 20}}>
                <Link style={{marginRight: 20}} to="/">Home</Link>
                <Link to="/about">About</Link>
              </div>
              <Routes>
                <Route path="/" element={<div>home</div>}></Route>
                <Route path="/about" element={<div>about</div>} />
              </Routes>
            </HashRouter>
        </div>
      );
    }

    export default App;
```

我们执行`yarn build`，然后将`build`目录下的文件迁移到`/data/web`下，再访问服务器IP，发现访问正常，路由切换也没有问题，即部署成功。

![1726294599377](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294599377.png)



### **2、History模式**

```
    import {
      BrowserRouter,
      ...
    } from "react-router-dom";
```

将Hash模式中的代码修改为BrowserRouter，运行本地项目，路由切换正常，该路由是History模式

![1726294614971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294614971.png)



同样执行`yarn build`生成`build`目录，将该目录下的文件迁移到上一步服务器的目录/data/web下，然后访问外网IP，发现渲染效果和上图一样，但是当我们点击About页面，然后刷新浏览器发现，出现了404。

![1726294624748](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294624748.png)



先说解决办法，然后解释下原因，修改Nginx配置`web.conf`

> 增加一行try_files配置，当请求的地址找不到时，重新指向index.html文件

```
    server { 
        listen 80; 
        server_name localhost; 
        root /data/www/; 
        
        location / { 
            try_files $uri $uri/ /index.html; 
            index index.html; 
        } 
    }
```

重启nginx `nginx -t`、`nginx -s reload` 再次刷新页面，发现页面访问正常了，切换也没有问题。

![1726294639745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294639745.png)



### **3、为什么hash模式不会出现404，而history模式会出现404？**

> 了解下这两种模式的区别就知道答案了

#### **1）Hash模式**

在hash路由模式下，URL中的Hash值（#后面的部分）用来表示应用的状态或路由信息。当用户切换路由时，只有Hash部分发生变化，并没有向服务器发出请求，就做到了浏览器对于页面路由的管理。

- Hash模式下，URL和路由路径由#号分隔：`http://example.com/#/about?query=abc`
- 当`#`后面的路径发生变化时，会触发浏览器的hashchange事件，通过hashchange事件监听到路由路径的变化，从而导航到不同的路由页面。
- Hash模式`#`后面的路径并不会作为URL出现在网络请求中。例如对于输入的**example.com/#/about**[1] ，实际上请求的URL是**example.com/**[2] ，所以不管输入的Hash路由路径是什么，实际网络请求的都是主域名或`IP:Port`

#### **2）History模式**

History路由模式下，调用浏览器HTML5中`history`API来管理导航。URL和路径是连接在一起的，路由的路径包含在请求的URL里面，路由路径作为URL的一部分一起发送。

- History模式下，URL路由格式为：`http://example.com/about&query=abc`
- 当我们向服务器发出请求时，服务器会请求对应的路径的资源

综上，当我们打开入口文件index.html的路径时，切换url此时是本地路由，访问正常，但是当我们处于非入口页面时，刷新浏览器，此时发出请求，由于服务器就找不到资源路径了，变成了404。

而对于Hash模式来说，总是请求的根路径，所以不会出现这种情况。

## **四、配置反向代理、负载均衡**

![1726294651545](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294651545.png)



### **1、反向代理**

反向代理的用途很多，这里我们看一个常用的，代理请求的接口。我们在发布时前端的域名和后端api服务的域名经常不一致，此时就可以使用Nginx配置反向代理来解决这个问题。

```
    server {
        location /api {
            proxy_pass http://backend1.example.com;
            proxy_set_header Host $host; 
            proxy_set_header X-Real-IP $remote_addr; 
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
```

代理的时候要注意添加必要的参数，帮助后端获取一些客户端的请求数据

- `proxy_set_header Host $host; `：客户端请求的主机名（Host），不加的话，后端无法获取主机名信息
- `proxy_set_header X-Real-IP $remote_addr;`：用户的真实IP(X-Real-IP)，如果不设置，后端只能拿到代理服务器的IP
- `proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`代理链路，如果用户中间经过了多个代理服务器，如果不加这个参数，那么后端服务将无法获取用户的真实来源

### **2、负载均衡**

Nginx可以作为负载均衡服务器使用，通过配置upstream来分发流量，同时可以配置一些参数：

- weight：分发权重
- ip_hash：配置始终将ip的请求始终转发到同一台后端服务器。
- max_fails: 将某个后台服务标记为不可用之前，允许请求失败的次数
- backup：标记当前服务为备用服务
- down：暂时不可用

```
    upstream api {
        ip_hash;
        server backend1.example.com;
        server backend2.example.com;
        # server backend1.example.com weight=5;
    }
    server {
        location /api {
            proxy_pass http://api;
            proxy_set_header Host $host; 
            proxy_set_header X-Real-IP $remote_addr; 
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
```

## **五、配置nginx日志**

Nginx日志也是很重要的一个内容，在我们请求资源出现问题时，要排查请求的资源是否到达Nginx，而且请求日志可以记录很多有用的信息。

```
   log_format  gzip  '$remote_addr - $remote_user [$time_local]  '
   : '"$request" $status $bytes_sent '
   : '"$http_referer" "$http_user_agent" "$gzip_ratio"';

   access_log  /var/logs/nginx-access.log  gzip  buffer=32k;
```

nginx日志主要涉及`access_log`，`log_format`

- `log_format`: 日志格式，通过nginx内置的变量来读取和排列，通常默认即可
- `access_log`: 日志输出的地址、是否压缩、buffer是否当日志大于32k后吸入磁盘

## **六、其他常用配置**

### **1、配置Gzip压缩**

**作为前端性能优化的一种方式，Gzip是简单且有效的，尽管目前前端对于静态资源会进行压缩，但Gzip依然可以在网络传输过程中对文件进行压缩**

下面这些字段可以放在`http、server、location`指令模块

```
    http {
        # 开启关闭
        gzip on;
        # 压缩的文件类型
        gzip_types text/plain text/css application/javascript;
        # 过小的文件没必要压缩
        gzip_min_length 1000; # 单位Byte
        gzip_comp_level 5; # 压缩比，默认1，范围时1-9，值越大压缩比最大，但处理最慢，所以设置5左右比较合理。
    }
```

### **2、配置请求头**

**允许客户端请求在http请求中添加以下划线格式命名的参数**

该字段可以放在`http`指令模块

```
    http {
        underscores_in_headers on;
    }
    ```

**允许客户端上传文件最大不超过1M，在开发上传接口时一定要注意，否则导致上传失败**

该字段可以放在`http、server、location`指令模块

http {
    client_max_body_size 1m;
}

### 3、浏览器缓存配置

**缓存也是前端优化的一个重点，合理的缓存可以提高用户访问速度**

该字段可以放在`http、server、location`指令模块

配置浏览器缓存的有三个地方

#### 1）后端服务，配置请求头

后端根据语言不同，配置关键字段即可

#### 2）代理服务器（Nginx）配置缓存请求头

​```nginx
location /static {
 # /static匹配到的资源有效期设置为1d;
 expires 1d;  
 # /设置资源有效期为一周;
 # expires max-age=604800; 
 
 # 设置浏览器可以被缓存，设置7天后资源过期
 add_header Cache-Control "public, max-age=604800";
 # 阻止浏览器缓存动态内容
 # add_header Cache-Control "no-cache, no-store, must-revalidate";
 # 禁用浏览器缓存
 # add_header Cache-Control "no-store, private, max-age=0";
}
```

我们发现响应头的过期时间更新了

![1726294726611](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726294726611.png)



#### **3）在前端资源中通过meta声明缓存信息**

```
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
<meta http-equiv="Expires" content="0">
```

### **4、跨域处理**

> 通过反向代理，已经处理了请求域名和端口不一致的跨域问题，但有局限性。Nginx有专门方法配置请求资源的跨域

该字段可以放在`server、location`指令模块，通过配置头部字段，做跨域处理

```
server {

    location / {
        # 允许所有来源的跨域请求
        add_header Access-Control-Allow-Origin *;
        
        # 允许特定的HTTP方法（GET、POST等）
        add_header Access-Control-Allow-Methods "GET, POST, OPTIONS, PUT, DELETE";

        # 允许特定的HTTP请求头字段
        add_header Access-Control-Allow-Headers "Origin, X-Requested-With, Content-Type, Accept";

        # 响应预检请求的最大时间
        add_header Access-Control-Max-Age 3600;

        # 允许携带身份凭证（如Cookie）
        add_header Access-Control-Allow-Credentials true;

        # 处理 OPTIONS 预检请求
        if ($request_method = 'OPTIONS') {
            add_header Access-Control-Allow-Methods "GET, POST, OPTIONS, PUT, DELETE";
            add_header Access-Control-Allow-Headers "Origin, X-Requested-With, Content-Type, Accept";
            add_header Access-Control-Max-Age 3600;
            add_header Access-Control-Allow-Credentials true;
            add_header Content-Length 0;
            add_header Content-Type text/plain;
            return 204;
        }
    }
}
```