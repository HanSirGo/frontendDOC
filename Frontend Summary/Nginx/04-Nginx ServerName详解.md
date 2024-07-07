# Nginx ServerName详解

1）应用场景

如果我们有好几个域名（包含二级域名、泛域名），需要解析到相同的Server（虚拟主机站点）或者不同的Server，可以使用`server_name`指令实现。

> ❝
>
> **二级域名和泛域名区别？**
>
> **二级域名**是指位于主域名之下的域名层次。例如，如果`example.com`是一个主域名，那么`sub.example.com`就是一个二级域名，其中`sub`就是在主域名前添加的一个层次。
>
> **泛域名**又称为泛解析，是指使用通配符`*`进行的DNS解析记录设置，它能够匹配所有的子域名。例如，设置`*.example.com`指向某个IP地址或主机名，这样任何以`example.com`结尾的子域名请求都会被解析到同一目的地，不论是`blog.example.com`、`news.example.com`还是其他任意前缀的子域名。

Nginx的配置文件中，`server_name` 是一个指令，用于定义HTTP请求中的主机名（即`Host`头部）与特定的虚拟主机配置相匹配的规则。

当Nginx接收到一个HTTP请求时，它会根据请求中的`Host`头部信息去匹配`server_name`配置，从而确定由哪个虚拟主机配置来处理这个请求。支持以下几种匹配方式：

1. **精确匹配**：直接指定确切的域名或IP地址，如 `server_name example.com;`。

2. **通配符匹配**：使用星号`*`作为通配符，匹配任意字符串，如 `server_name *.example.com 或者 example.*;` 。

3. **正则表达式匹配**：在域名前加上波浪线`~`或`~*`（不区分大小写）来表示正则表达式匹配，如 `server_name ~^(www\.)?example\.com$;`。

   > **❝**

4. - `~`：表示进行正则表达式匹配，并且是区分大小写的。
   - `^`：匹配字符串的开始。
   - `(www\.)?`：这是一个可选的匹配组，表示前面的`www.`可以出现0次或1次（由`?`定义）。`\.`是对`.`的转义，因为`.`在正则表达式中是一个特殊字符，表示匹配任意单个字符，但在这里我们需要匹配实际的点字符`.`。
   - `example\.com`：精确匹配字符串`example.com`，`\.`用于转义点号，使其匹配字面上的点字符。
   - `$`：匹配字符串的结束。

如果请求的`Host`头部值与任何`server_name`配置都不匹配，Nginx将使用默认的服务器块来处理请求，如果没有明确设置默认服务器块，则可能由第一个列出的服务器块处理。

特殊值 `_` （下划线）在`server_name`中表示不匹配任何域名的请求，通常**用于捕获未指定的Host头请求**或作为默认的配置块（处理那些没有匹配到任何其他`server_name`配置的请求）。

2）Nginx的`server_name`匹配顺序遵循以下规则：

1. **精确匹配（Exact Match）**: 首先尝试精确匹配，即请求的`Host`头部与配置中的`server_name`完全一致。这是优先级最高的匹配方式。
2. **前缀通配符匹配（Prefix Wildcard Matching）**: 接着尝试匹配以`*`开头的最长通配符名称，如`*.example.org`。如果有多个这样的匹配项，最长的前缀优先。
3. **后缀通配符匹配（Suffix Wildcard Matching）**: 然后是匹配以`*`结尾的最长通配符名称，如`mail.*`。如果有多个这样的匹配项，最长的后缀优先。
4. **正则表达式匹配（Regular Expression Matching）**: 最后，如果上述都没有匹配到，Nginx会按照配置文件中出现的顺序检查正则表达式匹配项，第一个匹配成功的正则表达式胜出。正则表达式以波浪线`~`开头，例如`server_name ~ "^www\d+\.example\.net$"`。

如果没有任何`server_name`匹配成功，Nginx会使用默认的`server`块处理请求，该块通常监听一个默认的端口（如80或443），并且可能带有`default_server`或`default_host`标记。

记住，当配置多个`server_name`时，精确匹配始终优先，因此为了效率和清晰性，建议尽量使用精确匹配，并将精确匹配放在配置的前面。如果服务器配置了大量的域名或长域名列表，可能还需要调整`server_names_hash_max_size`和`server_names_hash_bucket_size`的值，以确保Nginx可以正确启动并高效地处理请求。

**注意：**server_name匹配分先后顺序，写在前面的匹配上后就不会继续往下匹配了。

## **精确匹配**

1）单一域名

`C:\Windows\System32\drivers\etc`下的hosts文件配置

![1720264888501](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264888501.png)

配置（单个站点）

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
        server_name  test.qiuyl.com;    # 该域名访问qiuyl01

        location / {
            root   /www/qiuyl01;
            index  index.html index.htm;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

浏览器访问

![1720264913495](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264913495.png)

配置（两个站点）

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
        server_name  test.qiuyl.com;   # 该域名访问qiuyl01站点

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
        server_name  test02.qiuyl.com;  # 该域名访问qiuyl02站点

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
}
```

没有匹配到任何域名时，从上到下访问第一个Server站点

![1720264944666](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264944666.png)

精准匹配域名，访问qiuyl01站点

![1720264981379](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264981379.png)

精准匹配域名，访问qiuyl02站点

![1720264991783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720264991783.png)

2）多域名

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
        server_name  test.qiuyl.com;

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
        server_name  test02.qiuyl.com test03.qiuyl.com;   # 这里的域名都能能访问到qiuyl02站点

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
}
```

test02.qiuyl.com、test03.qiuyl.com域名都访问到qiuyl02站点

![1720265021149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265021149.png)

## **通配符匹配**

1）通配符开始匹配

```
 server {
        listen       80;
        server_name  test.qiuyl.com;

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
        server_name  *.qiuyl.com;   # 通配符开始匹配，这里的域名都能能访问到qiuyl02站点

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
```

test.qiuyl.com为精确匹配访问qiuyl01，其它为通配符匹配访问qiuyl02。

![1720265065461](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265065461.png)

![1720265076145](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265076145.png)

![1720265086622](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265086622.png)

2）通配符结束匹配

![1720265101393](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265101393.png)

```
server {
        listen       80;
        server_name  test.qiuyl.com;

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
        server_name  test1.qiuyl.*;   # 通配符结束匹配，这里的域名都能能访问到qiuyl02站点

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
```

test1.qiuyl开头域名访问qiuyl02站点

![1720265130884](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265130884.png)

## **正则表达式匹配**

1. **正则匹配以~开头**：在Nginx的`server_name`指令中，当使用正则表达式来匹配域名时，需要以`~`开头来指示这是一个区分大小写的正则匹配。如果要进行不区分大小写的匹配，则应使用`~*`。

2. **精确匹配与正则匹配**：如果不使用`~`或`~*`前缀，Nginx会将`server_name`后面的值视为精确字符串匹配。

3. **锚定符号`^`和`$`**：在正则表达式中，`^`表示字符串的开始位置，`$`表示字符串的结束位置。使用它们可以确保整个域名完全匹配指定的模式，而不是部分匹配。

   > ❝
   >
   > **例如：**
   >
   > `~^www\.example\.com$`会精确匹配"www.example.com"，而不匹配像"subdomain.www.example.com"这样的子域名。
   >
   > `^hello$`只会精确匹配整个字符串"hello"，而不是像"hello there"或者"hi hello"这样的字符串。

4. **点.的转义**：在正则表达式中，`.`是一个特殊字符，代表任何单个字符（除了换行符）。如果你要匹配字面上的点`.`），需要使用反斜线`\`进行转义。

5. **大括号{}的处理**：在Nginx配置文件中，大括号`{}`有特殊的含义，用于定义指令块。因此，如果正则表达式中包含大括号，并且这些大括号不是用于量词（例如`{2,3}`），为了防止解析错误，应该将整个正则表达式用双引号包围。例如，`server_name "~^(www\\.)?example\\.net(:[0-9]+)?$";` 这里的双引号使得大括号被正确识别为正则表达式的一部分，而不是Nginx的指令结构。

1）正则表达式术语解释

```
 匹配模式:
        ~* 表示不区分大小写的正则匹配。
        !~ 表示区分大小写的正则不匹配。
        !~* 表示不区分大小写的正则不匹配。

    基本元字符:
        . 匹配任何单个字符（不包括换行符）。
        \w 匹配字母、数字、下划线或汉字。
        \s 匹配任何空白字符，如空格、制表符。
        \d 匹配数字。
        \b 匹配单词边界。
        ^ 和 $ 分别匹配字符串的开始和结束。

    量词:
        *, +, ? 分别表示前面元素可出现0次或多次、1次或多次、0次或1次。
        {n}, {n,}, {n,m} 分别表示前面元素精确出现n次、至少n次、n到m次。
        *?, +?, ??, {n,m}? 等懒惰量词，尽可能少地匹配。

    特殊字符类:
        \W, \S, \D 分别匹配非字母数字下划线、非空白字符、非数字。
        \B 匹配非单词边界。

    字符组:
        [^x] 匹配除了x之外的任何字符。
        [^abc] 匹配除了abc之外的任何字符。
        
    匹配单个特定字符:    
        [abc] 匹配'a'、'b' 或 'c' 中的任意一个字符。
        
    范围匹配:    
        [a-z] 匹配任何小写字母从'a'到'z'。
        [0-9] 匹配任何数字从'0'到'9'。
        [A-Za-z0-9] 匹配任何大小写字母或数字。

    括号与捕获:
        (exp) 捕获表达式到自动编号的组中。
        (?:exp) 非捕获组，用于分组但不捕获内容。
        (?<name>exp) 或 (?'name'exp) 命名捕获组，通过名字引用捕获的内容。

    预查断言:
        (?=exp) 零宽度正向先行断言，匹配exp前面的位置。
        (?<=exp) 零宽度正向回顾断言，匹配exp后面的位置。
        (?!exp) 零宽度负向先行断言，匹配后面不跟着exp的位置。
        (?<!exp) 零宽度负向回顾断言，匹配前面不跟着exp的位置。

    注释:
        (?#comment) 在正则表达式中添加注释，提高可读性。
```

2）配置

```
server {
        listen       80;
        server_name  test.qiuyl.com;

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
        server_name  ~^[0-9]+\.qiuyl.com$;   # 这里的域名都能访问到qiuyl02站点

        location / {
            root   /www/qiuyl02;
            index  index.html index.htm;
        }
    }
```

![1720265768666](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265768666.png)

Host配置

浏览器访问：

![1720265810749](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720265810749.png)