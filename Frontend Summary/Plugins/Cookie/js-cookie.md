## js-cookie

> `js-cookie` 是一个开源的 JavaScript 库，设计用于简化在浏览器环境中对 Cookie 的操作。这个库提供了一套简洁的 API，使得开发者能够更容易地进行 Cookie 的读取、写入和删除操作。

#### 安装

```js
# • 使用 npm（Node Package Manager）安装，适合于基于模块化的项目：

npm install js-cookie --save

# • 或者直接通过 `<script>` 标签引入 CDN（Content Delivery Network）链接到你的 HTML 文件中：

<script src="https://cdn.jsdelivr.net/npm/js-cookie@version_number/dist/js.cookie.min.js"></script>
```

#### 使用

```js
# 在模块化项目中引用：

import Cookies from 'js-cookie';

# 使用 Cookies 对象的基本方法：

#设置（创建或更新）Cookie：
Cookies.set('key', 'value'); // 设置一个名为 "key" 的 Cookie，其值为 "value"
Cookies.set('key', 'value', { expires: 7 }); // 设置有效期为7天
Cookies.set('key', 'value', { path: '/' }); // 设置路径为整个网站
// 可以添加更多选项如 domain, secure, sameSite 等

# 获取Cookie：
const value = Cookies.get('key'); // 获取名为 "key" 的 Cookie 的值

# 删除Cookie：
Cookies.remove('key'); // 删除名为 "key" 的 Cookie
```

#### API示例

```js
# 设置带过期时间的Cookie：
Cookies.set('username', 'John Doe', { expires: 7 }); // 这个Cookie将在7天后过期

# 处理JSON对象（自动序列化和反序列化）：
const user = { id: 123, name: 'Jane' };
Cookies.set('user', user); // 自动转换为字符串存储
const storedUser = Cookies.get('user'); // 自动解析为对象
```

> 总之，`js-cookie` 提供了一个跨浏览器兼容且易于使用的接口来处理 Cookie，避免了原生 JavaScript 操作 Cookie 时可能遇到的一些复杂性和兼容性问题。

