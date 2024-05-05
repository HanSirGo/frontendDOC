# 实现了一下 CSRF 原来不像背的那么简单

## **一、前言**

之前面试经常被问到 CSRF， 跨站请求伪造

> 大概流程比较简单， 大概就是用户登录了A页面，存下来登录凭证（cookie）， 攻击者有诱导受害者打开了B页面， B页面中正好像A发送了一个跨域请求，并把cookie进行了携带， 欺骗浏览器以为是用户的行为，进而达到执行危险行为的目的，完成攻击

上面就是面试时，我们通常的回答， 但是到底是不是真是这样呢? 难道这么容易伪造吗？于是我就打算试一下能不能实现

## **二、实现**

接下来，我们就通过node起两个服务 A服务（端口3000）和B服务（端口4000）， 然后通过两个页面 A页面、和B页面模拟一下CSRF。

我们先约定一下 B页面是正常的页面， 起一个 4000 的服务， 然后 A页面为伪造者的网站， 服务为3000

先看B页面的代码, B页面有一个登录，和一个获取数据的按钮， 模拟正常网站，需要登录后才可以获取数据

```html
<body>
    <div>
      正常 页面 B
      <button onclick="login()">登录</button>
      <button onclick="getList()">拿数据</button>
      <ul class="box"></ul>
      <div class="tip"></div>
    </div>
  </body>
  <script>
    async function login() {
      const response = await fetch("http://localhost:4000/login", {
        method: "POST",
      });
      const res = await response.json();
      console.log(res, "writeCookie");
      if (res.data === "success") {
        document.querySelector(".tip").innerHTML = "登录成功， 可以拿数据";
      }
    }

    async function getList() {
      const response = await fetch("http://localhost:4000/list", {
        method: "GET",
      });

      if (response.status === 500) {
        document.querySelector(".tip").innerHTML = "cookie失效，请先登录！";
        document.querySelector(".box").innerHTML = "";
      } else {
        document.querySelector(".tip").innerHTML = "";
        const data = await response.json();
        let html = "";
        data.map((el) => {
          html += `<div>${el.id} - ${el.name}</div>`;
        });
        document.querySelector(".box").innerHTML = html;
      }
    }
  </script>
```

在看B页面的服务端代码如下：

```js
const express = require("express");
const app = express();

app.use(express.json()); // json
app.use(express.urlencoded({ extends: true })); // x-www-form-urlencoded

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  // 允许客户端跨域传递的请求头
  res.header("Access-Control-Allow-Headers", "Content-Type");
  next();
});

app.use(express.static("public"));

app.get("/list", (req, res) => {
  const cookie = req.headers.cookie;
  if (cookie !== "user=allow") {
    res.sendStatus("500");
  } else {
    res.json([
      { id: 1, name: "zhangsan" },
      { id: 2, name: "lisi" },
    ]);
  }
});

app.post("/login", (req, res) => {
  res.cookie("user", "allow", {
    expires: new Date(Date.now() + 86400 * 1000),
  });
  res.send({ data: "success" });
});

app.post("/delete", (req, res) => {
  const cookie = req.headers.cookie;
  if (req.headers.referer !== req.headers.host) {
    console.log("should ban!");
  }
  if (cookie !== "user=allow") {
    res.sendStatus("500");
  } else {
    res.json({
      data: "delete success",
    });
  }
});

app.listen(4000, () => {
  console.log("sever 4000");
});
```

B 服务有三个接口， 登录、获取列表、删除。再触发登录接口的时候，会像浏览器写入cookie， 再删除或者获取列表的时候，都先检测有没有将指定的cookie传回，如果有就认为有权限

然后我们打开 `http://localhost:4000/B.html` 先看看B页面功能是否都正常

![1714809976870](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714809976870.png)

我们看到此时 B 页面功能和接口都是正常的， cookie 也正常进行了设置，每次获取数据的时候，都是会携带cookie到服务端校验的

那么接下来我们就通过A页面，起一个3000端口的服务，来模拟一下跨域情况下，能否完成获取 B服务器数据，调用 B 服务器删除接口的功能

A页面代码

```html
  <body>
    <div>
      伪造者页面 A
      <form action="http://localhost:4000/delete" method="POST">
        <input type="hidden" name="account" value="xiaoming" />
      </form>
      <script>
        // 这行可以放到控制台执行，便于观察效果
        // document.forms[0].submit();
      </script>
    </div>
    <ul class="box"></ul>
    <div class="tip"></div>
</body>
```

A页面服务端代码

```html
 <body>
    <div>
      伪造者页面 A
      <form action="http://localhost:4000/delete" method="POST">
        <input type="hidden" name="account" value="xiaoming" />
      </form>
      <script>
        // 这行可以放到控制台输入
        // document.forms[0].submit();
      </script>
      <script src="http://localhost:4000/list"></script>
    </div>
  </body>
```

于是在我们 访问 `http://localhost:3000/A.html` 页面的时候发现， 发现list列表确实，请求到了， 控制台输入 `document.forms[0].submit()` 时发现，确实删除也发送成功了， 是不是说明csrf就成功了呢， 但是其实还不是， 关键的一点是， 我们在B页面设置cookie的时候， domain设置的是 `localhost` 那么其实在A页面， 发送请求的时候cookie是共享的状态， 真实情况下，肯定不会是这样， 那么为了模拟真实情况， 我们把 `http://localhost:3000/A.html` 改为 `http://127.0.0.1:3000/A.html`, 这时发现，以及无法访问了， 那么这是怎么回事呢， 说好的，cookie 会在获取过登录凭证下， 再次访问时可以携带呢。

![1714810015576](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714810015576.png)

于是，想了半天也没有想明白， 难道是浏览器限制严格进行了限制， 限制规避了这个问题？难道我们背的面试题是错误的？

有知道的小伙伴，欢迎下方讨论

## **三、 sameSite 设置**

其实上面的问题无法实现 CSRF 是由于 SameSite 的设置问题， 其实关键是要设置 `SameSite=None` 简单看一下下面三个设置的区分

- None: 通常用于处理第三方提供的资源或服务，白话就是不受域的限制，不同域间可以传递身份验证凭证（可以做单点登录）， 但是要配合`Secure` 标志， 即实用 HTTPS 协议
- Strict: 严格模式，完全禁止，只有自己网站的请求发送cookie
- Lax：浏览器的默认值， 用户从外部导航到你的网站时，可以发送cookie

接下来我们就把上面的 B 服务进行改造， 再设置cookie的时候， 同时设置 sameSite 和 Secure

```js
app.post("/login", (req, res) => {
  res.cookie("user", "allow", {
    expires: new Date(Date.now() + 86400 * 1000),
    sameSite: "none",
    secure: true,
  });
  res.send({ data: "success" });
});
```

然后再次打开 `http://127.0.0.1:3000/A.html` 会发现 cookie 正常设置到了浏览器，

![1714810043511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714810043511.png)

我们再次在控制台 输入 `document.forms[0].submit()` 发现删除操作也正常执行了

![1714810064371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714810064371.png)

如图，我们就完成了 CSRF 攻击

## **四、总结**

CSRF —— 跨域请求伪造， 顾名思义，攻击是发生在第三方页面， 利用了浏览器在跨域的情况下，在一些跨域的请求时（如 script 、 img、 form 表单提交、 a标签等）可以携带cookie的特性， 使得一些伪造的构建请求可以执行成功， 进而对正常网页造成危害的行为。我们通过代码也能看出来， 其实现代浏览器对跨域cookie的限制比较多了， 但是在一些情况下，依然可以发生， 比如设置 `sameSite: None` 时，虽然会增加安全风险， 但是 samesite为 None 又有一些场景需要跨域下可以共享cookie，比如单点登录、内嵌广告、资源时， 所以无法完全禁止， 由此我们就可以总结一些避免CSRF攻击的手段了，

1. 使用同源策略， 限制不同来源直接资源共享
2. 检查 Refer 头部， 设置白名单（并不保险，比如 a标签可以设置 rel="noreferrer" 绕开refer检查
3. 使用 CSRF Token 每个用户会话设置一个不可预测 唯一的 token，每次请求校验
4. 尽量使用 POST 和 其他安全的HTTP方法， 不在 GET 请求中实现数据写入操作
5. 敏感操作，增加验证码二次验证
6. 服务端把 Cookie 的SameSite 属性设置为 Lax
7. 表单提交增加 csrfToken 隐藏字段

额外：由于CSRF攻击原理是浏览器自动携带 Cookie， 如果放开跨站Cookie有CSRF 风险，但是不放开又无法做单点登录， 所以对于SPA应用来说， JWT 认证的方式更好一些，token 会在 `Authorization` 头部进行传递