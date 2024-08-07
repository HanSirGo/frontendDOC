## Cookies

Cookies 是一个用于获取和设置 HTTP(S) cookie的node.js模块。它的特点如下：

- 允许使用Keygrip来签署cookie，以防止篡改；
- 延迟验证cookie，以降低成本；
- 不允许通过不安全的套接字发送安全cookies；
- 默认情况下，所有cookie都仅适用于HTTP，并且通过SSL发送的cookie是安全的；
- 允许其他库在不知道签名机制的情况下访问 cookie。

```js
var http = require('http')
var Cookies = require('cookies')

// Optionally define keys to sign cookie values
// to prevent client tampering
var keys = ['keyboard cat']

var server = http.createServer(function (req, res) {
  // Create a cookies object
  var cookies = new Cookies(req, res, { keys: keys })

  // Get a cookie
  var lastVisit = cookies.get('LastVisit', { signed: true })

  // Set the cookie to a value
  cookies.set('LastVisit', new Date().toISOString(), { signed: true })

  if (!lastVisit) {
    res.setHeader('Content-Type', 'text/plain')
    res.end('Welcome, first time visitor!')
  } else {
    res.setHeader('Content-Type', 'text/plain')
    res.end('Welcome back! Nothing much changed since your last visit at ' + lastVisit + '.')
  }
})

server.listen(3000, function () {
  console.log('Visit us at http://127.0.0.1:3000/ !')
})
```

**GitHub：**https://github.com/pillarjs/cookies