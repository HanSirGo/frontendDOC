# 【HTTP】精讲HTTP协议常见请求类型！图解超赞超详细！！！

**1. HTTP 基本知识**

​        HTTP 是超文本传输协议，用来定义客户端与服务器数据传输的规范。

​        HTTP 服务端默认端口为 80，HTTPS 默认端口为 443，客户端的端口是动态分配的。

​       HTTP 请求方式一共有 9 种，来表明Request-URL指定的资源不同的操作方式。分别为 POST 、GET 、HEAD、PUT 、PATCH 、 OPTIONS 、DELETE 、CONNECT 、 TRACE 。

![1719144794100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144794100.png)

​        **HTTP1.0** 定义了前三种请求方法: POST 、GET 、HEAD 

​     **HTTP 1.1** 新增了后六种三种请求方法: PUT 、PATCH 、 OPTIONS 、DELETE 、CONNECT 、 TRACE 

​        **其中最常用的四种请求方法：GET, POST, PUT, DELETE**



**2. HTTP 常见请求类型**

**2.1 GET请求**

**GET**：可以理解为 取 的意思，对应select操作

​       用来获取数据的，只是用来查询数据，不对服务器的数据做任何的修改，新增，删除等操作。表示请求指定的页面信息，并返回实体内容。最常见的一种请求方式，主要用于向指定的URL（URI）请求资源，请求参数和对应的值附加在URL后面。例如，/index.jsp?id=100&op=bind,。

​        这种方式不适合传送私密数据。由于不同的浏览器对地址的字符限制也有所不同，一般最多只能识别1024个字符，所以如果需要传送大量数据的时候，也不适合使用GET方式。

​        GET不会改变资源状态，是等幂的。即：一次请求和多次请求，资源的状态是一样。GET、HEAD、DELETE、PUT也是等幂的。

说明：

​        \1. GET请求会把请求的参数附加在URL后面，这样是不安全的，在处理敏感数据时不用，或者参数做加密处理。

​        \2. GET请求其实本身HTTP协议并没有限制它的URL大小，但是不同的浏览器对其有不同的大小长度限制

举例：

https://www.tapd.cn/company/my_take_part_in_projects_list?project_id=20085821&t=1655176334048&from=left_tree

![1719144821190](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144821190.png)



**2.2 POST请求**

​       POST可以理解为 贴 的意思。
​       表示数据发送到服务器以创建或更新资源，侧重于更新数据，对应update操作。

​      主要是向指定的URL（URI）提交数据, 通常用于表单发送。POST方法可以允许客户端给服务器提供信息较多。POST方法将请求参数封装在HTTP请求数据中，可以传输大量数据，而且也不会显示在URL中。POST不会限制发送给服务器的信息的大小，而且POST请求不能保证是幂等的。

说明：
        POST请求的请求参数都是请求body中
举例：
https://www.tapd.cn/20085821/bugtrace/buglists/query/1/created/desc?query_token=%……&&**

![1719144833736](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144833736.png)



**2.3 HEAD请求**

​      HEAD就像GET，只不过服务端接受到HEAD请求后只返回响应头，而不会发送响应内容。当我们只需要查看某个页面的状态的时候，使用HEAD是非常高效的。



**2.4 PUT请求**

​     PUT：可以理解为 放 的意思
​     表示数据发送到服务器以创建或更新资源，侧重于创建数据，对应insert操作

​      HTTP PUT方法：功能跟POST相似，用来将信息放到请求的URL（URI）上，PUT方法是幂等方法， POST非幂等方法，PUT在请求时容易造成数据冗余，POST 则不然。



**2.5 DELETE请求**

​      delete：字面意思删除，即删除数据，对应delete操作
​      用来删除指定的资源，它会删除URI给出的目标资源的所有当前内容

​      Http delete方法：用于删除请求URL上的某个资源， 该请求返回状态有3.

​      200：表示删除请求被成功执行，返回被删除的资源

​      202：表示删除请求被接受，但还没有被执行

​      204：表示删除请求被执行，但没有返回被删除的资源

​      HTTP提供了一个与PUT方法对应的DELETE方法。一个DELETE请求将需要从Web服务器删除的内容指定为请求行中的资源部分。

​      DELETE方法唯一有趣的地方在于当你接收了一个标识为200 OK的响应的时候，那并不意味着指定的资源已经被删除了。那仅仅说明服务器接收到了删除资源的命令。这一例外允许了出于安全考虑的人为的干预。



**2.6 OPTIONS请求**

​       用来描述了目标资源的通信选项，返回服务器针对特定资源所支持的HTTP请求方法，也可以利用向web服务器发送‘*’的请求来测试服务器的功能性!

​     该请求方法的响应不能缓存

OPTIONS请求方法的主要用途有两个：

​     1、获取服务器支持的HTTP请求方法；也是黑客经常使用的方法。

​     2、用来检查服务器的性能。例如：AJAX进行跨域请求时的预检，需要向另外一个域名的资源发送一个HTTP OPTIONS请求头，用以判断实际发送的请求是否安全。

举例：

https://imgservice.csdn.net/direct/v1.0/image/upload?type=blog&rtype=markdown&x-image-template=standard&x-image-app=direct_blog&x-image-dir=direct&x-image-suffix=png

![1719144851719](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144851719.png)

**2.7 TRACE请求**

​        TRACE_Method是HTTP（超文本传输）协议定义的一种协议调试方法，该方法会使服务器原样返回任意客户端请求的任何内容。

​        TRACE和TRACK是用来调试web服务器连接的HTTP方式。支持该方式的服务器存在跨站脚本漏洞，通常在描述各种浏览器缺陷的时候，把"Cross-Site-Tracing"简称为XST。攻击者可以利用此漏洞欺骗合法用户并得到他们的私人信息。

​        如何关闭Apache的TRACE请求：

​        虚拟主机用户可以在.htaccess文件中添加如下代码过滤TRACE请求:

​        RewriteEngine on

​       RewriteCond %{REQUEST_METHOD} ^(TRACE|TRACK)

​        RewriteRule .* - [F]

服务器用户在httpd.conf尾部添加如下指令后重启apache即可:

​        TraceEnable off



**2.8 CONNECT请求**

​       CONNECT这个方法的作用就是把服务器作为跳板，让服务器代替用户去访问其它网页，之后把数据原原本本的返回给用户。这样用户就可以访问到一些只有服务器上才能访问到的网站了，这就是HTTP代理。所以首先要让服务器监听一个端口来接收CONNECT方法的请求。CONNECT方法是需要使用TCP直接去连接的，所以不适合在网页开发中使用。



**2.9 TRACE 请求**

​        TRACE ：回显服务器收到的请求，主要用于测试和诊断。



**一图总览HTTP协议9种请求类型**

![1719144865616](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719144865616.png)

