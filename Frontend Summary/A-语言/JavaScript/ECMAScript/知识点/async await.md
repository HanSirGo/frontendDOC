# 7个Js async/await高级用法

JavaScript的异步编程已经从回调(Callback)演进到`Promise`，再到如今广泛使用的`async`/`await`语法。后者不仅让异步代码更加简洁，而且更贴近同步代码的逻辑与结构，大大增强了代码的可读性与可维护性。在掌握了基础用法之后，下面将介绍一些高级用法，以便充分利用`async`/`await`实现更复杂的异步流程控制。

## 1. `async/await`与高阶函数

当需要对数组中的元素执行异步操作时，可结合`async/await`与数组的高阶函数（如`map`、`filter`等）。

```
// 异步过滤函数
async function asyncFilter(array, predicate) {
    const results = await Promise.all(array.map(predicate));

    return array.filter((_value, index) => results[index]);
}

// 示例
async function isOddNumber(n) {
    await delay(100); // 模拟异步操作
    return n % 2 !== 0;
}

async function filterOddNumbers(numbers) {
    return asyncFilter(numbers, isOddNumber);
}

filterOddNumbers([1, 2, 3, 4, 5]).then(console.log); // 输出: [1, 3, 5]
```

## 2. 控制并发数

在处理诸如文件上传等场景时，可能需要限制同时进行的异步操作数量以避免系统资源耗尽。

```
async function asyncPool(poolLimit, array, iteratorFn) {
    const result = [];
    const executing = [];

    for (const item of array) {
        const p = Promise.resolve().then(() => iteratorFn(item, array));
        result.push(p);

        if (poolLimit <= array.length) {
            const e = p.then(() => executing.splice(executing.indexOf(e), 1));
            executing.push(e);
            if (executing.length >= poolLimit) {
                await Promise.race(executing);
            }
        }
    }

    return Promise.all(result);
}

// 示例
async function uploadFile(file) {
    // 文件上传逻辑
}

async function limitedFileUpload(files) {
    return asyncPool(3, files, uploadFile);
}
```

## 3. 使用`async/await`优化递归

递归函数是编程中的一种常用技术，`async/await`可以很容易地使递归函数进行异步操作。

```
// 异步递归函数
async function asyncRecursiveSearch(nodes) {
    for (const node of nodes) {
        await asyncProcess(node);
        if (node.children) {
            await asyncRecursiveSearch(node.children);
        }
    }
}

// 示例
async function asyncProcess(node) {
    // 对节点进行异步处理逻辑
}
```

## 4. 异步初始化类实例

在JavaScript中，类的构造器（constructor）不能是异步的。但可以通过工厂函数模式来实现类实例的异步初始化。

```
class Example {
    constructor(data) {
        this.data = data;
    }

    static async create() {
        const data = await fetchData(); // 异步获取数据
        return new Example(data);
    }
}

// 使用方式
Example.create().then((exampleInstance) => {
    // 使用异步初始化的类实例
});
```

## 5. 在`async`函数中使用`await`链式调用

使用`await`可以直观地按顺序执行链式调用中的异步操作。

```
class ApiClient {
    constructor() {
        this.value = null;
    }

    async firstMethod() {
        this.value = await fetch('/first-url').then(r => r.json());
        return this;
    }

    async secondMethod() {
        this.value = await fetch('/second-url').then(r => r.json());
        return this;
    }
}

// 使用方式
const client = new ApiClient();
const result = await client.firstMethod().then(c => c.secondMethod());
```

## 6. 结合`async/await`和事件循环

使用`async/await`可以更好地控制事件循环，像处理DOM事件或定时器等场合。

```
// 异步定时器函数
async function asyncSetTimeout(fn, ms) {
    await new Promise(resolve => setTimeout(resolve, ms));
    fn();
}

// 示例
asyncSetTimeout(() => console.log('Timeout after 2 seconds'), 2000);
```

## 7. 使用`async/await`简化错误处理

错误处理是异步编程中的重要部分。通过`async/await`，可以将错误处理的逻辑更自然地集成到同步代码中。

```
async function asyncOperation() {
    try {
        const result = await mightFailOperation();
        return result;
    } catch (error) {
        handleAsyncError(error);
    }
}

async function mightFailOperation() {
    // 有可能失败的异步操作
}

function handleAsyncError(error) {
    // 错误处理逻辑
}
```

# 你不知道的 async、await 魔鬼细节

关于`promise、async/await`的使用相信很多小伙伴都比较熟悉了，但是提到**事件循环机制输出结果**类似的题目，你敢说都会？

试一试？

🌰1：

```
async function async1 () {
    await new Promise((resolve, reject) => {
        resolve()
    })
    console.log('A')
}

async1()

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

// 最终结果👉: B A C D
```

🌰2：

```
async function async1 () {
    await async2()
    console.log('A')
}

async function async2 () {
    return new Promise((resolve, reject) => {
        resolve()
    })
}

async1()

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

// 最终结果👉: B C D A
```

❓基本一样的代码为什么会出现差别，话不多说👇

## 1、async 函数返回值

在讨论 `await` 之前，先聊一下 `async` 函数处理返回值的问题，它会像 `Promise.prototype.then` 一样，会对返回值的类型进行辨识。

👉**根据返回值的类型，引起 js引擎 对返回值处理方式的不同**

> 📑结论：`async`函数在抛出返回值时，会根据返回值**类型**开启**不同数目的微任务**
>
> - return结果值：非`thenable`、非`promise`（不等待）
> - return结果值：`thenable`（等待 1个`then`的时间）
> - return结果值：`promise`（等待 2个`then`的时间）

🌰1：

```
async function testA () {
    return 1;
}

testA().then(() => console.log(1));
Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));

// (不等待)最终结果👉: 1 2 3
```

🌰2：

```
async function testB () {
    return {
        then (cb) {
            cb();
        }
    };
}

testB().then(() => console.log(1));
Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));

// (等待一个then)最终结果👉: 2 1 3

```

🌰3：

```
async function testC () {
    return new Promise((resolve, reject) => {
        resolve()
    })
}

testC().then(() => console.log(1));
Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3));
    
// (等待两个then)最终结果👉: 2 3 1




async function testC () {
    return new Promise((resolve, reject) => {
        resolve()
    })
} 

testC().then(() => console.log(1));
Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3))
    .then(() => console.log(4))

// (等待两个then)最终结果👉: 2 3 1 4
```

看了这三个🌰是不是对`上面的结论`有了更深的认识？

稍安勿躁，来试试一个经典面试题👇

```
async function async1 () {
    console.log('1')
    await async2()
    console.log('AAA')
}

async function async2 () {
    console.log('3')
    return new Promise((resolve, reject) => {
        resolve()
        console.log('4')
    })
}

console.log('5')

setTimeout(() => {
    console.log('6')
}, 0);

async1()

new Promise((resolve) => {
    console.log('7')
    resolve()
}).then(() => {
    console.log('8')
}).then(() => {
    console.log('9')
}).then(() => {
    console.log('10')
})
console.log('11')

// 最终结果👉: 5 1 3 4 7 11 8 9 AAA 10 6
```

👀做错了吧？

哈哈没关系

> 步骤拆分👇：
>
> 1. 先执行同步代码，输出`5`
>
> 2. 执行`setTimeout`，是放入宏任务异步队列中
>
> 3. 接着执行`async1`函数，输出`1`
>
> 4. 执行`async2`函数，输出`3`
>
> 5. `Promise`构造器中代码属于同步代码，输出`4`
>
>    > `async2`函数的返回值是`Promise`，等待`2`个`then`后放行，所以`AAA`暂时无法输出
>
> 6. `async1`函数**暂时**结束，继续往下走，输出`7`
>
> 7. 同步代码，输出`11`
>
> 8. 执行第一个`then`，输出`8`
>
> 9. 执行第二个`then`，输出`9`
>
> 10. 终于**等**到了两个`then`执行完毕，执行`async1`函数里面剩下的，输出`AAA`
>
> 11. 再执行最后一个微任务`then`，输出`10`
>
> 12. 执行最后的宏任务`setTimeout`，输出`6`

❓是不是豁然开朗，欢迎点赞收藏！

## 2、await 右值类型区别

### 2.1、非 `thenable`

🌰1：

```
async function test () {
    console.log(1);
    await 1;
    console.log(2);
}

test();
console.log(3);
// 最终结果👉: 1 3 2
```

🌰2：

```
function func () {
    console.log(2);
}

async function test () {
    console.log(1);
    await func();
    console.log(3);
}

test();
console.log(4);

// 最终结果👉: 1 2 4 3
```

🌰3：

```
async function test () {
    console.log(1);
    await 123
    console.log(2);
}

test();
console.log(3);

Promise.resolve()
    .then(() => console.log(4))
    .then(() => console.log(5))
    .then(() => console.log(6))
    .then(() => console.log(7));

// 最终结果👉: 1 3 2 4 5 6 7
```

> Note:
>
> `await`后面接非 `thenable` 类型，会立即向微任务队列添加一个微任务`then`，**但不需等待**

### 2.2、`thenable`类型

```
async function test () {
    console.log(1);
    await {
        then (cb) {
            cb();
        },
    };
    console.log(2);
}

test();
console.log(3);

Promise.resolve()
    .then(() => console.log(4))
    .then(() => console.log(5))
    .then(() => console.log(6))
    .then(() => console.log(7));

// 最终结果👉: 1 3 4 2 5 6 7
```

> Note:
>
> `await` 后面接 `thenable` 类型，需要**等待一个 then 的时间之后**执行

### 2.3、`Promise`类型

```
async function test () {
    console.log(1);
    await new Promise((resolve, reject) => {
        resolve()
    })
    console.log(2);
}

test();
console.log(3);

Promise.resolve()
    .then(() => console.log(4))
    .then(() => console.log(5))
    .then(() => console.log(6))
    .then(() => console.log(7));

// 最终结果👉: 1 3 2 4 5 6 7
```

❓为什么表现的和非 `thenable` 值一样呢？为什么不等待两个 `then` 的时间呢？

> Note:
>
> - TC 39(ECMAScript标准制定者) 对`await` 后面是 `promise` 的情况如何处理进行了一次修改，**移除**了额外的两个微任务，在**早期版本**，依然会等待两个 `then` 的时间
> - 有大佬翻译了官方解释：**更快的 async 函数和 promises**[1]，但在这次更新中并没有修改 `thenable` 的情况

------

这样做可以极大的优化 `await` 等待的速度👇

```
async function func () {
    console.log(1);
    await 1;
    console.log(2);
    await 2;
    console.log(3);
    await 3;
    console.log(4);
}

async function test () {
    console.log(5);
    await func();
    console.log(6);
}

test();
console.log(7);

Promise.resolve()
    .then(() => console.log(8))
    .then(() => console.log(9))
    .then(() => console.log(10))
    .then(() => console.log(11));

// 最终结果👉: 5 1 7 2 8 3 9 4 10 6 11
```

> Note：
>
> `await` 和 `Promise.prototype.then` 虽然很多时候可以在**时间顺序**上能等效，但是它们之间有**本质的区别**。

> - `test` 函数中的 `await` 会等待 `func` 函数中所有的 `await` 取得 恢复函数执行 的命令并且整个函数执行完毕后才能获得取得 **恢复函数执行**的命令；
> - 也就是说，`func` 函数的 `await` 此时**不能在时间的顺序上等效** `then`，而要等待到 `test` 函数完全执行完毕；
> - 比如这里的数字`6`很晚才输出，**如果**单纯看成`then`的话，在下一个微任务队列执行时`6`就应该作为同步代码输出了才对。

------

所以我们可以合并两个函数的代码👇

```
async function test () {
    console.log(5);

    console.log(1);
    await 1;
    console.log(2);
    await 2;
    console.log(3);
    await 3;
    console.log(4);
    await null;
    
    console.log(6);
}

test();
console.log(7);

Promise.resolve()
    .then(() => console.log(8))
    .then(() => console.log(9))
    .then(() => console.log(10))
    .then(() => console.log(11));

// 最终结果👉: 5 1 7 2 8 3 9 4 10 6 11

```

------

因为将原本的函数融合，此时的 `await` 可以等效为 `Promise.prototype.then`，又完全可以等效如下代码👇

```
async function test () {
    console.log(5);
    console.log(1);
    Promise.resolve()
        .then(() => console.log(2))
        .then(() => console.log(3))
        .then(() => console.log(4))
        .then(() => console.log(6))
}

test();
console.log(7);

Promise.resolve()
    .then(() => console.log(8))
    .then(() => console.log(9))
    .then(() => console.log(10))
    .then(() => console.log(11));

// 最终结果👉: 5 1 7 2 8 3 9 4 10 6 11
```

------

以上三种写法在时间的顺序上完全等效，所以你 **完全可以将 await 后面的代码可以看做在 then 里面执行的结果**，又因为 `async` 函数会返回 `promise` 实例，所以还可以等效成👇

```
async function test () {
    console.log(5);
    console.log(1);
}

test()
    .then(() => console.log(2))
    .then(() => console.log(3))
    .then(() => console.log(4))
    .then(() => console.log(6))

console.log(7);

Promise.resolve()
    .then(() => console.log(8))
    .then(() => console.log(9))
    .then(() => console.log(10))
    .then(() => console.log(11));

// 最终结果👉: 5 1 7 2 8 3 9 4 10 6 11
```

可以发现，`test` 函数全是走的同步代码...

所以👉：**`async/await` 是用同步的方式，执行异步操作**

## 3、🌰

🌰1：

```
async function async2 () {
    new Promise((resolve, reject) => {
        resolve()
    })
}

async function async3 () {
    return new Promise((resolve, reject) => {
        resolve()
    })
}

async function async1 () {
    // 方式一：最终结果：B A C D
    // await new Promise((resolve, reject) => {
    //     resolve()
    // })

    // 方式二：最终结果：B A C D
    // await async2()

    // 方式三：最终结果：B C D A
    await async3()

    console.log('A')
}

async1()

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

```

> 大致思路👇：
>
> - 首先，**`async`函数的整体返回值永远都是`Promise`，无论值本身是什么**
> - 方式一：`await`的是`Promise`，无需等待
> - 方式二：`await`的是`async`函数，但是该函数的返回值本身是**非`thenable`**，无需等待
> - 方式三：`await`的是`async`函数，且返回值本身是`Promise`，需等待两个`then`时间

🌰2：

```
function func () {
    console.log(2);

    // 方式一：1 2 4  5 3 6 7
    // Promise.resolve()
    //     .then(() => console.log(5))
    //     .then(() => console.log(6))
    //     .then(() => console.log(7))

    // 方式二：1 2 4  5 6 7 3
    return Promise.resolve()
        .then(() => console.log(5))
        .then(() => console.log(6))
        .then(() => console.log(7))
}

async function test () {
    console.log(1);
    await func();
    console.log(3);
}

test();
console.log(4); 

```

> 步骤拆分👇：
>
> - 方式一：
>
> - - 同步代码输出`1、2`，接着将`log(5)`处的`then1`加入微任务队列，`await`拿到确切的`func`函数返回值`undefined`，将后续代码放入微任务队列（`then2`，可以这样理解）
>   - 执行同步代码输出`4`，到此，所有同步代码完毕
>   - 执行第一个放入的微任务`then1`输出`5`，产生`log(6)`的微任务`then3`
>   - 执行第二个放入的微任务`then2`输出`3`
>   - 然后执行微任务`then3`，输出`6`，产生`log(7)`的微任务`then4`
>   - 执行`then4`，输出`7`
>
> - 方式二：
>
> - - 同步代码输出`1、2`，`await`拿到`func`函数返回值，但是并未获得**具体的结果**（由`Promise`本身机制决定），暂停执行当前`async`函数内的代码（跳出、让行）
>   - 输出`4`，到此，所有同步代码完毕
>   - `await`一直等到`Promise.resolve().then...`执行完成，再放行输出`3`

方式二没太明白❓

继续👇

```
function func () {
    console.log(2);

    return Promise.resolve()
        .then(() => console.log(5))
        .then(() => console.log(6))
        .then(() => console.log(7))
}

async function test () {
    console.log(1);
    await func()
    console.log(3);
}

test();
console.log(4);

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

// 最终结果👉: 1 2 4    B 5 C 6 D 7 3
```

还是没懂？

继续👇

```
async function test () {
    console.log(1);
    await Promise.resolve()
        .then(() => console.log(5))
        .then(() => console.log(6))
        .then(() => console.log(7))
    console.log(3);
}

test();
console.log(4);

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

// 最终结果👉: 1 4    B 5 C 6 D 7 3

```

> Note:
>
> 综上，`await`一定要等到右侧的表达式有**确切的值**才会放行，否则将一直等待（阻塞当前`async`函数内的后续代码），不服看看这个👇
>
> - ```
>   function func () {
>    return new Promise((resolve) => {
>      console.log('B')
>      // resolve() 故意一直保持pending
>    })
>   }
>   ```
>
> async function test () {
>  console.log(1);
>  await func()
>  console.log(3);
> }
>
> test();
> console.log(4);
> // 最终结果👉: 1 B 4 (永远不会打印3)
>
>
> // ---------------------或者写为👇-------------------
> async function test () {
>  console.log(1);
>  await new Promise((resolve) => {
>      console.log('B')
>      // resolve() 故意一直保持pending
>  })
>  console.log(3);
> }
>
> test();
> console.log(4);
> // 最终结果👉: 1 B 4 (永远不会打印3)
>
> ```
> 
> ```

🌰3：

```
async function func () {
    console.log(2);
    return {
        then (cb) {
            cb()
        }
    }
}

async function test () {
    console.log(1);
    await func();
    console.log(3);
}

test();
console.log(4);

new Promise((resolve) => {
    console.log('B')
    resolve()
}).then(() => {
    console.log('C')
}).then(() => {
    console.log('D')
})

// 最终结果👉: 1 2 4 B C 3 D
```

> 步骤拆分👇：
>
> - 同步代码输出`1、2`
> - `await`拿到`func`函数的具体返回值`thenable`，将当前`async`函数内的后续代码放入微任务`then1`(但是需要等待一个`then`时间)
> - 同步代码输出`4、B`，产生`log(C)`的微任务`then2`
> - 由于`then1`滞后一个`then`时间，直接执行`then2`输出`C`，产生`log(D)`的微任务`then3`
> - 执行原本滞后一个`then`时间的微任务`then1`，输出`3`
> - 执行最后一个微任务`then3`输出`D`

## 4、总结

> `async`函数返回值
>
> - 📑结论：`async`函数在抛出返回值时，会根据返回值**类型**开启**不同数目的微任务**
>
> - - return结果值：非`thenable`、非`promise`（不等待）
>   - return结果值：`thenable`（等待 1个`then`的时间）
>   - return结果值：`promise`（等待 2个`then`的时间）
>
> `await`右值类型区别
>
> - 接非 `thenable` 类型，会立即向微任务队列添加一个微任务`then`，**但不需等待**
>
> - 接 `thenable` 类型，需要**等待一个 then 的时间之后**执行
>
> - 接`Promise`类型(有确定的返回值)，会立即向微任务队列添加一个微任务`then`，**但不需等待**
>
> - - TC 39 对`await` 后面是 `promise` 的情况如何处理进行了一次修改，**移除**了额外的两个微任务，在**早期版本**，依然会等待两个 `then` 的时间

