## Promise

##### Promise.allSettled()

```
Promise.allSettled() 方法返回一个 Promise，该 Promise 在所有给定的 Promise 已经 resolve 或 reject 后 resolve，提供每个 Promise 的结果数组。
```

```js
const promises = [
  Promise.resolve('Resolved'),
  Promise.reject('Rejected')
];

Promise.allSettled(promises)
  .then(results => {
    console.log(results);
  });
// [{ status: "fulfilled", value: "Resolved" }, { status: "rejected", reason: "Rejected" }]
```

