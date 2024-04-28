### JWT
**JWT(Json Web Token)是一种可以跨域的认证方案。**

> JWT其实就是个token生成方式。我们得到了一个JWT方式的token。那这个token放哪里呢？ query参数吗？最好把JWT放在HTTP请求的Header Authorization，格式是Authorization: Bearer jwtStr。

**JWT 主要用于用户登录鉴权，所以我们从最传统的 session 认证开始说起。**
#### session认证
众所周知，http 协议本身是无状态的协议，那就意味着当有用户向系统使用账户名称和密码进行用户认证之后，下一次请求还要再一次用户认证才行。因为我们不能通过 http 协议知道是哪个用户发出的请求，所以如果要知道是哪个用户发出的请求，那就需要在服务器保存一份用户信息(保存至 session )，然后在认证成功后返回 cookie 值传递给浏览器，那么用户在下一次请求时就可以带上 cookie 值，服务器就可以识别是哪个用户发送的请求，是否已认证，是否登录过期等等。这就是传统的 session 认证方式。

session 认证的缺点其实很明显，由于 session 是保存在服务器里，所以如果分布式部署应用的话，会出现session不能共享的问题，很难扩展。于是乎为了解决 session 共享的问题，又引入了 redis，接着往下看。
#### token认证
这种方式跟 session 的方式流程差不多，不同的地方在于保存的是一个 token 值到 redis，token 一般是一串随机的字符(比如UUID)，value 一般是用户ID，并且设置一个过期时间。每次请求服务的时候带上 token 在请求头，后端接收到token 则根据 token 查一下 redis 是否存在，如果存在则表示用户已认证，如果 token 不存在则跳到登录界面让用户重新登录，登录成功后返回一个 token 值给客户端。

优点是多台服务器都是使用 redis 来存取 token，不存在不共享的问题，所以容易扩展。缺点是每次请求都需要查一下redis，会造成 redis 的压力，还有增加了请求的耗时，每个已登录的用户都要保存一个 token 在 redis，也会消耗 redis 的存储空间。

有没有更好的方式呢？接着往下看。

#### JWT使用流程

>  - 首先，前端通过Web表单将自己的用户名和密码发送到后端的接口。这一过程一般是一个HTTP POST请求。建议的方式是通过SSL加密的传输(https协议)，从而避免敏感信息被嗅探。
>  - 后端核对用户名和密码成功后，将用户的id等其他信息作为 JWT-Payload（负载)，将其与头部分别进行Base64编码拼接后签名形成一个JWT。形成的JWT就是一个形同111.zzz.xxx的字符串。
>  - 后端将JWT字符串作为登录成功的返回结果返回给前端。前端可以将返回的结果保存在localStorage或sessionStorage上，退出登录时前端删除保存的JWT即可。
>  - 前端在每次请求时将JWT放入HTTP Header中的Authorization位。(解决XSS和XSRF问题)
>  - 后端检查是否存在，如存在验证JWT的有效性。例如，检查签名是否正确；检查Token是否过期；检查Token的接收方是否是自己(可选)。
>  - 验证通过后后端使用JWT中包含的用户信息进行其他逻辑操作，返回相应结果。
>  ![1713768780503](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713768780503.png)
#### JWT的数据结构
JWT 一般是这样一个字符串，分为三个部分，以 “.” 隔开：
```javascript
xxxxx.yyyyy.zzzzz
```
![1713768795185](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713768795185.png)
##### 头部 Header
JWT 第一部分是头部分，它是一个描述 JWT 元数据的 Json 对象，通常如下所示。
```javascript
{
    "alg": "HS256",
    "typ": "JWT"
}
```
> alg 属性表示签名使用的算法，默认为 HMAC SHA256（写为HS256），typ 属性表示令牌的类型，JWT 令牌统一写为JWT。
> 最后，使用 Base64 URL 算法将上述 JSON 对象转换为字符串保存。
##### 载荷 Payload
JWT 第二部分是 Payload，也是一个 Json 对象，除了包含需要传递的数据，还有七个默认的字段供选择。
>  - iss (issuer)：签发人/发行人
>  - sub (subject)：主题
>  - aud (audience)：用户
>  - exp (expiration time)：过期时间
>  - nbf (Not Before)：生效时间，在此之前是无效的
>  - iat (Issued At)：签发时间
>  - jti (JWT ID)：用于标识该 JWT

如果自定义字段，可以这样定义：
```javascript
{
    //默认字段
    "sub":"主题123",
    //自定义字段
    "name":"java技术爱好者",
    "isAdmin":"true",
    "loginTime":"2021-12-05 12:00:03"
}
```
> 需要注意的是，默认情况下 JWT 是未加密的，任何人都可以解读其内容，因此一些敏感信息不要存放于此，以防信息泄露。、
> JSON 对象也使用 Base64 URL 算法转换为字符串后保存，是可以反向反编码回原样的，这也是为什么不要在 JWT 中放敏感数据的原因。

##### 签名 Signature
```javascript
header (base64URL 加密后的)

payload (base64URL 加密后的)

secret
```
JWT 第三部分是签名。是这样生成的，首先需要指定一个 secret，该 secret 仅仅保存在服务器中，保证不能让其他用户知道。这个部分需要 base64URL 加密后的 header 和 base64URL 加密后的 payload 使用 . 连接组成的字符串，然后通过header 中声明的加密算法 进行加盐secret组合加密，然后就得出一个签名哈希，也就是Signature，且无法反向解密。

那么 Application Server 如何进行验证呢？可以利用 JWT 前两段，用同一套哈希算法和同一个 secret 计算一个签名值，然后把计算出来的签名值和收到的 JWT 第三段比较，如果相同则认证通过。
##### JWT的优点

>  - json格式的通用性，所以JWT可以跨语言支持，比如Java、JavaScript、PHP、Node等等。
>  - 可以利用Payload存储一些非敏感的信息。
>  - 便于传输，JWT结构简单，字节占用小。
>  - 不需要在服务端保存会话信息，易于应用的扩展。
##### JWT的缺点
>  - 安全性没法保证，所以 jwt 里不能存储敏感数据。因为 jwt 的 payload 并没有加密，只是用 Base64 编码而已。
>  - 无法中途废弃。因为一旦签发了一个 jwt，在到期之前始终都是有效的，如果用户信息发生更新了，只能等旧的 jwt 过期后重新签发新的 jwt。
>  - 续签问题。当签发的 jwt 保存在客户端，客户端一直在操作页面，按道理应该一直为客户端续长有效时间，否则当  jwt有效期到了就会导致用户需要重新登录。那么怎么为 jwt 续签呢？最简单粗暴就是每次签发新的  jwt，但是由于过于暴力，会影响性能。如果要优雅一点，又要引入 Redis 解决，但是这又把无状态的 jwt 硬生生变成了有状态的，违背了初衷。
#### 注意事项
>  - Header 和 Payload 只是简单的利用 Base64 编码，是可逆的，因此不要在 Payload 中存储敏感信息
>  - Signature 使用的是不可逆的加密算法，无法解码出原文，它的作用是校验 Token 有没有被篡改。该算法需要我们自己指定一个密钥，这个密钥存储在服务端，不能泄露 
>  - 尽量避免在 JWT 中存储大量信息，因为一些服务器接收的 HTTP 请求头最大不超过 8KB
#### JWT和Session的比较
>  - **1、无状态：**

token 自身包含了身份验证所需要的所有信息，使得我们的服务器不需要存储 Session 信息，这显然增加了系统的可用性和伸缩性，大大减轻了服务端的压力。
也导致了它最大的缺点：当后端在token 有效期内废弃一个 token 或者更改它的权限的话，不会立即生效，一般需要等到有效期过后才可以。另外，当用户 Logout 的话，token 也还有效。除非，我们在后端增加额外的处理逻辑。
>  - **2、避免CSRF 攻击：**

攻击者就可以通过让用户误点攻击链接，达到攻击效果。防止误触操作，避免请求直接获取本地的session值进行请求访问。
>  - **3、适合移动端应用**

使用 Session 进行身份认证的话，需要保存一份信息在服务器端，而且这种方式会依赖到 Cookie（需要 Cookie 保存 SessionId），所以不适合移动端。
但是，使用 token 进行身份认证就不会存在这种问题，因为只要 token 可以被客户端存储就能够使用，而且 token 还可以跨语言使用。
>  - **4、单点登录友好**

使用 Session 进行身份认证的话，实现单点登录，需要我们把用户的 Session 信息保存在一台电脑上，并且还会遇到常见的 Cookie 跨域的问题。但是，使用 token 进行认证的话， token 被保存在客户端，不会存在这些问题。