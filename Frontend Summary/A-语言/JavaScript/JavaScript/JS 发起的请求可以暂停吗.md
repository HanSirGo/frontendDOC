# 前端中 JS 发起的请求可以暂停吗

在前端中，JavaScript（JS）可以使用XMLHttpRequest对象或fetch API来发起网络请求。然而，JavaScript本身并没有提供直接的方法来暂停请求的执行。一旦请求被发送，它会继续执行并等待响应。

尽管如此，你可以通过一些技巧或库来模拟请求的暂停和继续执行。下面是一种常见的方法：

### 1. 使用XMLHttpRequest对象

你可以在发送请求前创建一个XMLHttpRequest对象，并将其保存在变量中。然后，在需要暂停请求时，调用该对象的abort()方法来中止请求。当需要继续执行请求时，可以重新创建一个新的XMLHttpRequest对象并发起请求。

```js
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://example.com/api', true);
xhr.send();

// 暂停请求
xhr.abort();

// 继续请求
xhr = new XMLHttpRequest();
xhr.open('GET', 'https://example.com/api', true);
xhr.send();
```

### 2. 使用fetch API和AbortController

fetch API与AbortController一起使用可以更方便地控制请求的暂停和继续执行。AbortController提供了一个abort()方法，可以用于中止fetch请求。

```js
var controller = new AbortController();

fetch('https://example.com/api', { signal: controller.signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));

// 暂停请求
controller.abort();

// 继续请求
controller = new AbortController();

fetch('https://example.com/api', { signal: controller.signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));
```

请注意，这些方法实际上是通过**中止请求并重新发起新的请求来模拟暂停和继续执行的效果，并不能真正暂停正在进行的请求。**

### 3. 曲线救国

模拟一个**假暂停**的功能，在前端的业务场景上，需要对这些数据进行处理之后渲染在界面上，如果我们能在请求发起之前增加一个控制器，在请求回来时，如果控制器为暂停状态则不处理数据，等待控制器恢复后再进行处理，也可以达到暂停的效果。

```js
// 创建一个暂停控制器 Promise
function createPauseControllerPromise() {
  const result = {
    isPause: false, // 标记控制器是否处于暂停状态
    resolveWhenResume: false, // 表示在恢复时是否解析 Promise
    resolve(value) {}, // 解析 Promise 的占位函数
    pause() { // 暂停控制器的函数
      this.isPause = true;
    },
    resume() { // 恢复控制器的函数
      if (!this.isPause) return;
      this.isPause = false;
      if (this.resolveWhenResume) {
        this.resolve();
      }
    },
    promise: Promise.resolve(), // 初始为已解决状态的 Promise
  };

  const promise = new Promise((res) => {
    result.resolve = res; // 将解析函数与 Promise 关联
  });
  result.promise = promise; // 更新控制器中的 Promise 对象

  return result; // 返回控制器对象
}

function requestWithPauseControl(request) {
  const controller = createPauseControllerPromise(); // 创建暂停控制器对象

  const controlRequest = request() // 执行请求函数
    .then((data) => { // 请求成功回调
      if (!controller.isPause) controller.resolve(); // 如果控制器未暂停，则解析 Promise
      return data; // 返回请求结果
    })
    .finally(() => {
      controller.resolveWhenResume = true; // 标记在恢复时解析 Promise
    });

  const result = Promise.all([controlRequest, controller.promise]).then(
    (data) => {
      controller.resolve(); // 解析控制器的 Promise
      return data[0]; // 返回请求处理结果
    }
  );

  result.pause = controller.pause.bind(controller); // 将暂停函数绑定到结果 Promise 对象
  result.resume = controller.resume.bind(controller); // 将恢复函数绑定到结果 Promise 对象

  return result; // 返回添加了暂停控制功能的结果 Promise 对象
}
```

#### 为什么需要创建两个promise

在requestWithPauseControl函数中，需要等待两个Promise对象解析：一个是请求处理的Promise，另一个是控制器的Promise。通过使用Promise.all方法，可以将这两个Promise对象组合成一个新的Promise，该新的Promise会在两个原始Promise都解析后才会解析。这样做的目的是确保在处理请求结果之前，暂停控制器的resolve方法被调用，以便在恢复时解析Promise。

因此，将请求处理的Promise和控制器的Promise放入一个Promise数组，并使用Promise.all等待它们都解析完成，可以确保在两个Promise都解析后再进行下一步操作，以实现预期的功能。

**使用**

```js
const result = requestWithPauseControl(/*request fn*/).then((data) => {
    console.log(data)
})

if (Math.random() > 0.5) { result.pause() }

setTimeout(() => {
    result.resume()
}, 4000)
```



**本文转载于：https://juejin.cn/post/7310786521082560562**