# HTTP 请求：GET、POST、PUT、DELETE 全解析

理解 HTTP 请求对于任何 Web 开发项目都至关重要，因为它们是客户端和服务器之间通信的桥梁。在这篇文章中，我们将深入探讨如何使用两种流行的 JavaScript 方法进行 HTTP 请求：Fetch API 和 XMLHttpRequest (XHR)。读完本文，你就能全面了解如何在自己的项目中使用这些技术。

## **JavaScript 中的 HTTP 请求简介**

HTTP 请求是 Web 交互的基础。它们允许客户端（例如 Web 浏览器）与服务器通信，根据需要获取或发送数据。与服务器交互的四种主要 HTTP 方法是：

- **GET：**  从服务器检索数据。
- **POST：**  将新数据发送到服务器。
- **PUT：**  更新服务器上的现有数据。
- **DELETE：**  从服务器中删除数据。

### **HTTP 方法详解**

#### **GET：从服务器检索数据**

GET 方法用于从指定资源请求数据。它是最常用的 HTTP 方法之一，通常用于从服务器检索数据，而无需对资源进行任何更改。发出 GET 请求时，任何参数通常都在 URL 中发送，服务器会以请求的数据进行响应。

#### **POST：将新数据发送到服务器**

POST 方法用于将数据发送到服务器以创建新资源。与 GET 请求不同，POST 请求在请求的正文中发送数据，而不是在 URL 中。此方法通常用于提交表单、上传文件或将用户生成的内容发送到服务器。服务器会处理数据，并通常返回一个响应，指示操作成功或失败。

#### **PUT：更新服务器上的现有数据**

PUT 方法用于更新服务器上的现有资源。与 POST 类似，PUT 请求在请求的正文中发送数据。但是，关键区别在于 PUT 是幂等的，这意味着发出多个相同的 PUT 请求将产生与发出单个请求相同的结果。此方法通常用于更新用户信息或修改服务器上的现有数据。

#### **DELETE：从服务器中删除数据**

DELETE 方法用于从服务器中删除指定的资源。发出 DELETE 请求时，服务器会删除该资源。此方法通常用于从数据库中删除记录或数据条目。与 PUT 一样，DELETE 也是幂等的，这意味着发出多个相同的 DELETE 请求将与发出单个请求具有相同的效果。

## **使用 Fetch API 进行 HTTP 请求**

Fetch API 是一个用于进行 HTTP 请求的现代接口。它是基于 Promise 的，简化了异步操作的处理，使代码更简洁易读。

### **Fetch API：GET 请求**

GET 请求从服务器检索数据。让我们了解如何使用 Fetch API 执行 GET 请求。

```
fetch('https://api.mocki.io/v1/b043df5a')
  .then(response => response.json()) // 将响应转换为 JSON
  .then(data => console.log(data)) // 将数据记录到控制台
  .catch(error => console.error('获取数据时出错：', error)); // 处理错误
```

- `fetch`  函数向指定的 URL 发起 GET 请求。
- `then`  方法处理响应，将其转换为 JSON 格式。
- 然后将数据记录到控制台。
- `catch`  方法将处理并记录遇到的任何错误。

### **Fetch API：POST 请求**

POST 请求将数据发送到服务器。让我们看看下面的代码并理解它。

```
fetch('https://api.mocki.io/v1/b043df5a', {
  method: 'POST', // 将请求方法指定为 POST
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john.doe@example.com'
  }), // 将数据转换为 JSON 字符串
  headers: {
    'Content-Type': 'application/json' // 将内容类型设置为 JSON
  }
})
  .then(response => response.json()) // 将响应转换为 JSON
  .then(data => console.log(data)) // 将数据记录到控制台
  .catch(error => console.error('发布数据时出错：', error)); // 处理错误
```

- `fetch`  函数向指定的 URL 发送 POST 请求。
- `body`  属性包含要发送的数据，这些数据已转换为 JSON 字符串。
- `headers`  属性指定内容类型为 JSON。
- 与 GET 请求示例一样，处理并记录响应。

### **Fetch API：PUT 请求**

PUT 请求更新服务器上的现有数据。以下是如何操作的。

```
fetch('https://api.mocki.io/v1/b043df5a/1', {
  method: 'PUT', // 将请求方法指定为 PUT
  body: JSON.stringify({
    name: 'Jane Doe',
    email: 'jane.doe@example.com'
  }), // 将数据转换为 JSON 字符串
  headers: {
    'Content-Type': 'application/json' // 将内容类型设置为 JSON
  }
})
  .then(response => response.json()) // 将响应转换为 JSON
  .then(data => console.log(data)) // 将数据记录到控制台
  .catch(error => console.error('更新数据时出错：', error)); // 处理错误
```

- `fetch`  函数向指定的 URL 发送 PUT 请求。
- `body`  属性包含 JSON 格式的更新数据。
- 像之前的示例一样处理并记录响应。

### **Fetch API：DELETE 请求**

DELETE 请求从服务器中删除数据。以下是代码示例：

```
fetch('https://api.mocki.io/v1/b043df5a/1', {
  method: 'DELETE' // 将请求方法指定为 DELETE
})
  .then(() => console.log('数据已成功删除')) // 记录成功消息
  .catch(error => console.error('删除数据时出错：', error)); // 处理错误
```

- `fetch`  函数向指定的 URL 发送 DELETE 请求。
- 如果请求成功，则记录成功消息。
- 捕获并记录遇到的任何错误。

## **使用 XMLHttpRequest 进行 HTTP 请求**

XMLHttpRequest (XHR) 是一种较旧但仍在广泛使用的 HTTP 请求方法。它使用事件监听器来处理响应和错误。

### **XMLHttpRequest：GET 请求**

以下是如何使用 XMLHttpRequest 执行 GET 请求。

```
const getData = (url, callback, errorCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('GET', url, true); // 初始化 GET 请求
  xhr.onload = () => callback(xhr.responseText); // 处理成功响应
  xhr.onerror = () => errorCallback(xhr); // 处理错误
  xhr.send(); // 发送请求
};

getData('https://api.mocki.io/v1/b043df5a', console.log, console.error);
```

- 首先创建 XMLHttpRequest 对象。
- `open`  方法初始化对指定 URL 的 GET 请求。
- `onload`  事件处理程序处理成功响应。
- `onerror`  事件处理程序处理任何错误。
- `send`  方法将请求发送到服务器。

### **XMLHttpRequest：POST 请求**

以下是如何发送 POST 请求。

```
const postData = (url, data, callback, errorCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('POST', url, true); // 初始化 POST 请求
  xhr.setRequestHeader('Content-Type', 'application/json'); // 将内容类型设置为 JSON
  xhr.onload = () => callback(xhr.responseText); // 处理成功响应
  xhr.onerror = () => errorCallback(xhr); // 处理错误
  xhr.send(JSON.stringify(data)); // 将数据转换为 JSON 字符串并发送
};

postData(
  'https://api.mocki.io/v1/b043df5a',
  {
    name: 'John Doe',
    email: 'john.doe@example.com'
  },
  console.log,
  console.error
);
```

- `open`  方法初始化对指定 URL 的 POST 请求。
- `setRequestHeader`  方法将内容类型设置为 JSON。
- `send`  方法将数据转换为 JSON 字符串并将其发送到服务器。

### **XMLHttpRequest：PUT 请求**

使用 XMLHttpRequest 发出 PUT 请求更新数据如下所示：

```
const updateData = (url, data, callback, errorCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('PUT', url, true); // 初始化 PUT 请求
  xhr.setRequestHeader('Content-Type', 'application/json'); // 将内容类型设置为 JSON
  xhr.onload = () => callback(xhr.responseText); // 处理成功响应
  xhr.onerror = () => errorCallback(xhr); // 处理错误
  xhr.send(JSON.stringify(data)); // 将数据转换为 JSON 字符串并发送
};

updateData(
  'https://api.mocki.io/v1/b043df5a/1',
  {
    name: 'Jane Doe',
    email: 'jane.doe@example.com'
  },
  console.log,
  console.error
);
```

- `open`  方法初始化对指定 URL 的 PUT 请求。
- `setRequestHeader`  方法将内容类型设置为 JSON。
- `send`  方法将更新后的数据转换为 JSON 字符串并将其发送到服务器。

### **XMLHttpRequest：DELETE 请求**

让我们使用 XMLHttpRequest 发出 DELETE 请求：

```
const deleteData = (url, callback, errorCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open('DELETE', url, true); // 初始化 DELETE 请求
  xhr.onload = () => callback(xhr.responseText); // 处理成功响应
  xhr.onerror = () => errorCallback(xhr); // 处理错误
  xhr.send(); // 发送请求
};

deleteData(
  'https://api.mocki.io/v1/b043df5a/1',
  () => console.log('数据已成功删除'),
  console.error
);
```

- `open`  方法初始化对指定 URL 的 DELETE 请求。
- `send`  方法将请求发送到服务器。
- 如果请求成功，则记录成功消息。
- 捕获并记录遇到的任何错误。

## **总结**

无论你是选择现代的 Fetch API 还是可靠的 XMLHttpRequest，JavaScript 都提供了强大的方法来执行 HTTP 请求。Fetch API 因其基于 Promise 的结构而提供了更直观、更简洁的方法。相比之下，XMLHttpRequest 仍然是一个稳健的选择，尤其是对于旧版浏览器或现有代码库。通过掌握这些 API，你可以有效地与 Web 服务器交互并轻松处理数据。