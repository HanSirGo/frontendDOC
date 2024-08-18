# 在WebSocket中加入Token

在WebSocket通信中加入Token主要是为了实现身份验证和授权，确保只有经过验证的用户可以建立WebSocket连接。由于WebSocket API本身不支持直接在连接时设置HTTP头部，因此需要采用一些变通的方法来传递Token。以下是几种常见的方法：

1、通过URL参数传递Token：

在WebSocket的URL中直接携带Token参数。这种方法简单直接，但安全性较低，因为Token会暴露在URL中，容易被截获。

```
const socket = new WebSocket('wss://example.com/socket?authorization=' + YOUR_TOKEN);
```

2、在连接建立后发送Token：

在WebSocket连接建立后，通过onopen事件监听器发送Token。这种方法相对安全，因为Token不会在握手阶段暴露。

```js
const socket = new WebSocket('wss://example.com/socket');
socket.addEventListener('open', (event) => {
  socket.send('Authorization: Bearer ' + YOUR_TOKEN);
});
```

3、使用WebSocket子协议（Sec-WebSocket-Protocol）：

利用WebSocket的子协议特性传递Token。这种方法需要服务器端支持并正确处理子协议。

```js

const token = localStorage.getItem('token');

const socket = new WebSocket('wss://example.com/socket', [token]);
```

4、使用JWT（JSON Web Token）：

JWT是一种无状态的、可自验证的令牌，可以安全地在客户端和服务器之间传递。客户端在登录后获取JWT，然后在WebSocket连接时将其作为Token传递。

```js
const jwtToken = jwt.sign(payload, secretKey, { expiresIn: '1h' });
// 生成JWT Token
const socket = new WebSocket('wss://example.com/socket');
  socket.addEventListener('open', (event) => {
  socket.send(jwtToken);
});
```

5、使用服务器端的认证中间件：

在服务器端，可以使用认证中间件（如Express.js的passport.js）来处理WebSocket连接。这样，服务器可以在握手阶段验证Token的有效性。

```js
// 服务器端示例（使用Express.js和socket.io）
io.use((socket, next) => {
const token = socket.handshake.query.authorization;
// 验证Token逻辑
next();
});
```

在实际应用中，选择哪种方法取决于你的具体需求和安全要求。通常，建议使用JWT结合HTTPS来确保Token的安全传输，同时在服务器端进行严格的Token验证。