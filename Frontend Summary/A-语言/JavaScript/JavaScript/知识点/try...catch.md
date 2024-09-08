## try...catch

#### 同步执行

> `try...catch`不能异步捕获代码错误，因为它本身就是一个同步代码块。
>
> **try...catch只能捕捉到同步执行代码块中的错误**

以下代码是否可以捕获到错误。如果不可以，请说明原因，以及如何修改

```js
try {
  setTimeout(() => {
    throw new Error('err')
  }, 200);
} catch (err) {
  console.log(err);
}

// -------

try {
  Promise.resolve().then(() => {
    throw new Error('err')
  })
} catch (err) {
  console.log(err);
}
```

```
try...catch 是同步执行的，这就意味着 它无法处理异步的错误。但是在 setTimeout 和 Promise.resolve() 又都是典型的异步任务，所以说try...catch无法捕获。
```

```js
// 那么既然明白了问题的原因，想要处理就非常简单了。

// 我们只需要同步任务中使用 try...catch 捕获即可。

new Promise((resolve, reject) => {
  setTimeout(() => {
    try {
      throw new Error('err');
    } catch (err) {
      reject(err);
    }
  }, 200);
})
  .then(() => {
    // Handle successful execution
  })
  .catch((err) => {
    console.log(err); // Error is caught here
  });
```

![1709456306623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709456306623.png)