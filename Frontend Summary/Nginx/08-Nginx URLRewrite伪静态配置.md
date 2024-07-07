# Nginx URLRewrite伪静态配置

![1720267570158](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267570158.png)

1）Nginx的`rewrite`语法主要用于实现URL重写和重定向功能，它是`ngx_http_rewrite_module`模块的一部分，该模块默认包含在Nginx中。以下是`rewrite`指令的基本语法结构和一些关键点：

```
rewrite regex replacement [flag];
```

- **regex**: 是一个正则表达式，用于匹配请求的URI部分。

- **replacement**: 是一个字符串，用于替换匹配到的部分。

- **flag**（可选）: 控制重写操作的行为，常见的flag包括：

- - `last`: 表示完成当前的重写规则后，继续向下搜索并尝试匹配其他location块。
  - `break`: 如果当前重写规则匹配并执行后，停止执行当前的rewrite指令序列，并不再进行后续的rewrite规则匹配，不影响其它指令匹配。
  - `redirect`: 返回临时重定向状态码（默认为302），告诉客户端使用新的URL。
  - `permanent`: 返回永久重定向状态码（301），适合永久性的URL变更。

> 其他高级用法还包括使用条件语句`if`来控制`rewrite`的执行，以及使用变量等。例如，可以在`server`或`location`上下文中使用`if`来判断特定条件，然后基于这些条件应用重写规则。

2）rewrite指令和server_name指令使用正则表达式时，其语法和要求是不一样的？

**rewrite指令：**

- 默认匹配行为：在`rewrite`指令中，正则表达式默认就是按正则模式进行匹配的，无需在正则前加上特殊字符来表明这是一个正则表达式。Nginx会自动识别并处理正则匹配逻辑。
- 正则指示：尽管不是强制的，但你可以使用`~`（区分大小写）或`~*`（不区分大小写）来显式指定正则匹配的类型，这在需要强调匹配特性时很有用，但不是必需的。

**server_name指令：**

- 默认匹配行为：默认情况下，`server_name`指令支持精确匹配和使用通配符（如`*.example.com`），而不是正则表达式。为了启用正则匹配，必须明确指定。
- 正则指示：使用正则表达式时，必须在正则前加上`~`（区分大小写）或`~*`（不区分大小写）来明确告知Nginx这是一个正则匹配模式。没有这些前缀，字符串将被视为精确匹配或通配符匹配。

> 一句话总结：`rewrite`指令默认接受正则表达式且不强制要求特殊前缀，而`server_name`指令要使用正则表达式必须明确通过`~`或`~*`来指示，因为其默认处理非正则类型的匹配。

3）Rewrite的优缺点？

优点：掩藏真实的URL和URL中的参数，提高安全性。便于搜索引擎收录

缺点：降低效率，影响性能

## 配置

1）场景1

下列配置 `rewrite ^/test.html$ /index.html?testParam=3 break;`意思是，只要访问了`/test.html`，那么就相当于访问的`/index.html?testParam=3`路径，有助于隐藏传入的参数和SEO优化

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
       server localhost:8080;
    }

    server {
        listen       80;
        server_name  localhost;

        location / {
            rewrite ^/test.html$ /index.html?testParam=3 break;  # 重写URL
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

浏览器访问路径对比

> 可以看到访问`/test.html`路径，和访问`/index.html?testParam=3`路径效果一致，实现了我们隐藏传入的参数和SEO优化的目的

![1720267608929](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1720267608929.png)

2）场景2

咱们一般网页翻页时，需要传入对应参数，比如`/index,jsp?pageNum=0、/index,jsp?pageNum=25、/index,jsp?pageNum=50` 我们可以通过rewrite实现访问对应数字来实现翻页效果，如下指令。

rewrite 翻页效果指令：`rewrite ^/([0-9]+).html$ /index,jsp?pageNum=$1 break;`

> 在 Nginx 的 `rewrite` 指令中，`$1` 表示正则表达式中捕获的第一个匹配组。在你提供的规则中，正则表达式 `^/([0-9]+).html$` 匹配以数字开头，以 ".html" 结尾的 URI。
>
> - `^` 表示匹配字符串的开始。
> - `([0-9]+)` 是一个捕获组，表示匹配一个或多个数字。
> - `\.html$` 表示匹配以 ".html" 结尾的字符串。
> - `()` 内的部分就是捕获组，在这里用来捕获数字。
>
> 如果请求的 URI 符合这个规则，`$1` 就会被捕获的数字替换，然后将请求重写为 `/index.jsp?pageNum=捕获的数字`。这样就实现了将类似 `http://example.com/123.html` 这样的 URI 重写为 `http://example.com/index.jsp?pageNum=123`。

由于没有对应项目，没法测试哈（偷懒一波）

