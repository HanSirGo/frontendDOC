## 事件循环

##### 浏览器的进程模型 

###### 何为进程？ 

```
程序运⾏需要有它⾃⼰专属的内存空间，可以把这块内存空间简单的理解为进程
```

![1709363890154](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709363890154.png)

```
每个应⽤⾄少有⼀个进程，进程之间相互独⽴，即使要通信，也需要双⽅同意。
```

###### 何为线程？

```
有了进程后，就可以运⾏程序的代码了。
运⾏代码的「⼈」称之为「线程」。
⼀个进程⾄少有⼀个线程，所以在进程开启后会⾃动创建⼀个线程来运⾏代码，该线程称之为主线程。
如果程序需要同时执⾏多块代码，主线程就会启动更多的线程来执⾏代码，所以⼀个进程中可以包含多个线程。
```

![1709364003533](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709364003533.png)

###### 浏览器有哪些进程和线程？

```
浏览器是⼀个多进程多线程的应⽤程序
浏览器内部⼯作极其复杂。
为了避免相互影响，为了减少连环崩溃的⼏率，当启动浏览器后，它会⾃动启动多个进程。
```

![1709364087871](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709364087871.png)

> 可以在浏览器的任务管理器中查看当前的所有进程

```
其中，最主要的进程有：
1. 浏览器进程
主要负责界⾯显示、⽤户交互、⼦进程管理等。浏览器进程内部会启动多个线程处理不同的任务。
2. ⽹络进程
负责加载⽹络资源。⽹络进程内部会启动多个线程来处理不同的⽹络任务。
3. 渲染进程
渲染进程启动后，会开启⼀个渲染主线程，主线程负责执⾏ HTML、CSS、JS 代码。
默认情况下，浏览器会为每个标签⻚开启⼀个新的渲染进程，以保证不同的标签⻚之间不相互影响。
```

> 将来该默认模式可能会有所改变，可参⻅ chrome官⽅说明⽂档

###### 渲染主线程是如何⼯作的？

```
渲染主线程是浏览器中最繁忙的线程，需要它处理的任务包括但不限于：
	解析 HTML
	解析 CSS
	计算样式
	布局
	处理图层
	每秒把⻚⾯画 60 次
	执⾏全局 JS 代码
	执⾏事件处理函数
	执⾏计时器的回调函数
	......
```

> 为什么渲染进程不适⽤多个线程来处理这些事情？

```
要处理这么多的任务，主线程遇到了⼀个前所未有的难题：如何调度任务？
⽐如：
	我正在执⾏⼀个 JS 函数，执⾏到⼀半的时候⽤户点击了按钮，我该⽴即去执⾏点击事件的处理函数吗？
	我正在执⾏⼀个 JS 函数，执⾏到⼀半的时候某个计时器到达了时间，我该⽴即去执⾏它的回调吗？
	浏览器进程通知我“⽤户点击了按钮”，与此同时，某个计时器也到达了时间，我应该处理哪⼀个呢？
	......
渲染主线程想出了⼀个绝妙的主意来处理这个问题：排队
```

![1709364520684](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709364520684.png)

```
1. 在最开始的时候，渲染主线程会进⼊⼀个⽆限循环
2. 每⼀次循环会检查消息队列中是否有任务存在。如果有，就取出第⼀个任务执⾏，执⾏完⼀个后进⼊下⼀次循环；如果没有，则进⼊休眠状态。
3. 其他所有线程（包括其他进程的线程）可以随时向消息队列添加任务。新任务会加到消息队列的末尾。在添加新任务时，如果主线程是休眠状态，则会将其唤醒以继续循环拿取任务

这样⼀来，就可以让每个任务有条不紊的、持续的进⾏下去了。
整个过程，被称之为事件循环（消息循环）
```

##### 何为异步？

```
代码在执⾏过程中，会遇到⼀些⽆法⽴即处理的任务，⽐如：
	计时完成后需要执⾏的任务 —— setTimeout 、 setInterval
	⽹络通信完成后需要执⾏的任务 -- XHR 、 Fetch
	⽤户操作后需要执⾏的任务 -- addEventListener
	
如果让渲染主线程等待这些任务的时机达到，就会导致主线程⻓期处于「阻塞」的状态，从⽽导致浏览器「卡死」
```

![1709365613427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709365613427.png)

```
渲染主线程承担着极其重要的⼯作，⽆论如何都不能阻塞！
因此，浏览器选择异步来解决这个问题
```

![1709365677839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709365677839.png)

```
使⽤异步的⽅式，渲染主线程永不阻塞
```

###### JS 的异步

```
JS是⼀⻔单线程的语⾔，这是因为它运⾏在浏览器的渲染主线程中，⽽渲染主线程只有⼀个。
⽽渲染主线程承担着诸多的⼯作，渲染⻚⾯、执⾏ JS 都在其中运⾏。
如果使⽤同步的⽅式，就极有可能导致主线程产⽣阻塞，从⽽导致消息队列中的很多其他任务⽆法得到执⾏。这样⼀来，⼀⽅⾯会导致繁忙的主线程⽩⽩的消耗时间，另⼀⽅⾯导致⻚⾯⽆法及时更新，给⽤户造成卡死现象。
所以浏览器采⽤异步的⽅式来避免。具体做法是当某些任务发⽣时，⽐如计时器、⽹络、事件监听，主线程将任务交给其他线程去处理，⾃身⽴即结束任务的执⾏，转⽽执⾏后续代码。当其他线程完成时，将事先传递的回调函数包装成任务，加⼊到消息队列的末尾排队，等待主线程调度执⾏。
在这种异步模式下，浏览器永不阻塞，从⽽最⼤限度的保证了单线程的流畅运⾏。

```

###### JS为何会阻碍渲染？

```js
// 先看代码

<h1>awesome!</h1>
<button>change</button>
<script>
var h1 = document.querySelector('h1');
var btn = document.querySelector('button');
// 死循环指定的时间
function delay(duration) {
    var start = Date.now();
    while (Date.now() - start < duration) {}
}
btn.onclick = function () {
    h1.textContent = '很帅！';
    delay(3000);
};
</script>
// 点击按钮后，会发⽣什么呢？
```

##### 任务有优先级吗？

```js
任务没有优先级，在消息队列中先进先出
但消息队列是有优先级的

根据 W3C 的最新解释:
	每个任务都有⼀个任务类型，同⼀个类型的任务必须在⼀个队列，不同类型的任务可以分属于不同的队列。在⼀次事件循环中，浏览器可以根据实际情况从不同的队列中取出任务执⾏。
	浏览器必须准备好⼀个微队列，微队列中的任务优先所有其他任务执⾏https://html.spec.whatwg.org/multipage/webappapis.html#perform-a-microtask-checkpoint

随着浏览器的复杂度急剧提升，W3C 不再使⽤宏队列的说法

在⽬前 chrome 的实现中，⾄少包含了下⾯的队列：
	延时队列：⽤于存放计时器到达后的回调任务，优先级「中」
	交互队列：⽤于存放⽤户操作后产⽣的事件处理任务，优先级「⾼」
	微队列：⽤户存放需要最快执⾏的任务，优先级「最⾼」

添加任务到微队列的主要⽅式主要是使⽤ Promise、MutationObserver
// 例如：
// ⽴即把⼀个函数添加到微队列
Promise.resolve().then(函数)
```

##### ⾯试题：阐述⼀下 JS 的事件循环

```
事件循环⼜叫做消息循环，是浏览器渲染主线程的⼯作⽅式。
在 Chrome 的源码中，它开启⼀个不会结束的 for 循环，每次循环从消息队列中取出第⼀个任务执⾏，⽽其他线程只需要在合适的时候将任务加⼊到队列末尾即可。
过去把消息队列简单分为宏队列和微队列，这种说法⽬前已⽆法满⾜复杂的浏览器环境，取⽽代之的是⼀种更加灵活多变的处理⽅式。
根据 W3C 官⽅的解释，每个任务有不同的类型，同类型的任务必须在同⼀个队列，不同的任务可以属于不同的队列。不同任务队列有不同的优先级，在⼀次事件循环中，由浏览器⾃⾏决定取哪⼀个队列的任务。但浏览器必须有⼀个微队列，微队列的任务⼀定具有最⾼的优先级，必须优先调度执⾏。
```

##### ⾯试题：JS 中的计时器能做到精确计时吗?为什么?

```
不⾏.
因为:
	1. 计算机硬件没有原⼦钟，⽆法做到精确计时
	2. 操作系统的计时函数本身就有少量偏差，由于 JS 的计时器最终调⽤的是操作系统的函数，也就携带了这些偏差
	3. 按照 W3C 的标准，浏览器实现计时器时，如果嵌套层级超过 5 层，则会带有 4 毫秒的最少时间，这样在计时时间少于 4 毫秒时⼜带来了偏差
	4. 受事件循环的影响，计时器的回调函数只能在主线程空闲时运⾏，因此⼜带来了偏差
```

------

# Event - Loop

## 什么是 Event Loop？

**Event Loop**，即事件循环，是 JavaScript 异步编程的核心机制。它是一个持续运行的进程，负责监听执行栈和消息队列（任务队列），确保代码的执行顺序和异步任务的准确执行。

通俗来讲，它就是一个规则，代码会按照这个规则执行下去，不至于被某个需要等待的任务阻塞。

## 为什么会担心任务阻塞 ？

JS 设计之初是为了实现与用户动态交互的网页，为了能有更好的用户体验，不需占用太多资源，让交互更流畅，从而设计成了**单线程**。

这样的设计简化了编程模型，减少了许多复杂的并发问题，但也引入了一个潜在的问题：同步任务执行时间过长可能导致**任务阻塞**。

## 如何避免任务阻塞

为了解决任务阻塞的问题，JS 引入了 **异步任务** 和 **事件循环机制** 。异步任务通常包括定时器、事件监听器等。当遇到异步任务时，它会被放入任务队列中，而不会立即执行。主线程在执行完当前的同步任务后，会检查任务队列，将等待执行的异步任务取出来执行。

## 执行栈与任务队列

### 执行栈

当我们调用一个方法的时候，JavaScript 会生成一个与这个方法对应的执行环境，又叫执行上下文(context)。

这个执行环境中保存着该方法的私有作用域、上层作用域(作用域链)、方法的参数，以及这个作用域中定义的变量和 this 的指向，而当一系列方法被依次调用的时候。由于 JavaScript 是单线程的，这些方法就会按顺序被排列在一个单独的地方，这个地方就是所谓**执行栈**。

### 任务队列

主线程会不停的从执行栈中读取事件，**同步代码则立即执行**。当遇到一个异步事件后，并不会一直等待异步事件返回结果，而是会将这个事件放置另一个队列中，即**任务队列**(Task Queue)。

当主线程将执行栈中所有的代码执行完之后，主线程将会去查看任务队列是否有任务。如果有，那么事件队列便依次将任务压入执行栈中运行

### 异步任务

想象一下你在餐馆点餐。如果你点了一个需要等待一段时间才能做好的菜，而你不想傻坐在那里等，你可能会继续看菜单，聊天，或者做其他事情。当你点的菜做好了，服务员会把它端到你的桌子上。在这个等待的过程中，你并不需要一直盯着厨房等待，可以去做其他的事情，等待结束后再回来处理这个菜。

在编程中，异步任务就是这样的一个概念。当你执行一个异步任务时，不需要等待它完成，而是继续执行后面的代码。当异步任务完成时，系统会通知你，然后你可以处理它的结果。这样可以充分利用时间，不让程序在等待某些操作的同时停滞不前。

异步任务又分为 **宏任务（macrotask）** 和 **微任务（microtask）** ，它们分别代表着不同的异步执行上下文。

#### 1. 宏任务

常见的宏任务有：

> - script 代码
> - setTimeout
> - setInterval
> - I/O 操作
> - UI 渲染
> - postMessage
> - MessageChannel
> - setImmediate(Node.js 环境)
> - ······

#### 2. 微任务

常见的微任务有：

> - Promise.then() *注：Promise本身是同步任务*
> - Async/await
> - MutationObserver 监听DOM变化
> - process.nextTick
> - ······

### 事件循环的执行顺序

1. 当JS引擎拿到要执行的代码时，就算进入第一次事件循环，开始执行同步代码（这属于宏任务，因为script内的代码都属于宏任务），同步代码被推入执行栈，并执行；
2. 当执行栈为空，查看微任务队列中是否有待执行的任务；
3. 如果微任务队列中有任务，将微任务从队列中取出，推入执行栈，开始执行；
4. 如果有需要，会渲染页面；
5. 查看宏任务队列，推入执行栈，执行宏任务，开始下一轮事件循环。

### 举个栗子🌰

下面代码的打印顺序是怎样的呢？

```
console.log('start');setTimeout(()=>{    console.log('setTimeout');    setTimeout(()=>{        console.log('inner');    })    console.log('end');},1000)new Promise((resolve,reject)=>{    console.log('Promise');    resolve()}).then(()=>{    console.log('then1');}).then(()=>{    console.log('then2');})
```

【答案】（点击展开）

> start
> Promise
> then1
> then2
> setTimeout
> end
> inner

首先我们对各任务进行划分：

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

进入第一次事件循环，开始执行同步任务，这时候start和Promise的打印分别进栈并打印，随后出栈，宏任务进入宏任务队列，微任务进入微任务队列。

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

同步任务执行完后，栈为空，将微任务队列的任务依次推入执行栈执行，分别打印then1，then2。

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

微任务执行完毕，将宏任务推入执行栈执行，也算第二次事件循环的开始。

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

第二次事件循环开始，重复上述步骤，同步任务先执行，分别打印setTimeout和end。

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

微任务队列为空，直接执行宏任务，开始第三次事件循环，打印同步任务console.log('inner')。

![图片](data:image/svg+xml,<%3Fxml version='1.0' encoding='UTF-8'%3F><svg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'><title></title><g stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'><g transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'><rect x='249' y='126' width='1' height='1'></rect></g></g></svg>)image.png

所以最终打印结果为：

> start
> Promise
> then1
> then2
> setTimeout
> end
> inner

```
console.log("Start");

setTimeout(() => {
  console.log("Timeout1");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
}).then(() => {
  for(let i = 0 ; i < 9999; i++){
      console.log(`我是for循环的第${i}个`)
  }
});

setTimeout(() => {
  console.log("Timeout2");
},0);


console.log("End");
```

**分析过程**

1. `console.log("Start");`：同步任务会立即执行，在控制台输出 "Start"。

2. `setTimeout(() => { console.log("Timeout1"); }, 0);`：会被放入宏任务队列中。

3. `Promise.resolve().then(() => { console.log("Promise 1"); })`：会被放入微任务队列，因为Promise.resolve()为同步执行，会返回一个已解析（resolved）的 Promise 对象，Promise对象调用then中的回调函数会被放入微任务队列中。

4. `.then(() => { for(let i = 0 ; i < 9999; i++){ console.log(`我是for循环的第${i}个`) } });`： 会被放置微任务队列，因为then中的回调函数会被放入微任务队列中。（秉承着队列的“先进先出”的属性，大量的 "我是for循环的第i个" 会在`Promise1`之后输出）

5. `setTimeout(() => { console.log("Timeout2"); }, 0);`：会被放入宏任务，此前宏任务中已有`Timeout1`,宏任务也是队列，一样是先进先出，所以`Timeout2`会在之前进入的`Timeout1`之后输出。

6. `console.log("End");`：同步任务立即执行，在控制台输出 "End"。

   ```js
   Start
   End
   Promise1
   我是for循环的第0个
   我是for循环的第1个
   .............
   我是for循环的第9998个
   Timeout1
   Timeout2
   
   ```

   注意:微任务始终优先于宏任务执行，而在微任务队列中的任务会一直执行完毕，直到队列为空，然后才会执行下一个宏任务。这种执行机制是事件循环的关键特性。

## 循环的多久循环一次

答案很简单，不确定，是不是很奇怪的答案，但是我们能从上面的跑步的例子中理解这个答案，假如这个跑者的体力恒定，速度恒定，那么当他跑一圈的速度就恒定，这样我们就知道了跑一圈要多久了对吧，然而事实却是跑一圈的过程中会出现很多障碍，而且障碍的难易程度还不确定，这样就导致了我们不知道跑者跑一圈要多久

带入到事件循环中，我们并不知道在一次循环中会出现哪些任务需要执行器执行，这些任务的执行时间也不知道是多少，例如这次的任务只是执行一个这个👇，可能执行时间不到一毫秒

```
console.log(123)
```

而下次的任务是执行这个👇

```js
for(let i = 1; i < 30; i++){ 
  console.log(fib(i))
}
function fib(n) {
if(n < 3) return 1
  return fib(n - 1) + fib(n - 2)
}
```

可能要执行好几秒，这就导致了事件循环执行完一圈的时间不固定的原因，我们可以认为执行一圈的时间与任务的难易程度和电脑的性能有直接关系

## 1.5事件循环的开始和结束

事件循环在标签页打开的一瞬间就已经开始，直到关闭当前标签页才会停止，中间没有任何原因会暂停

## 1.6事件循环的任务执行逻辑

这里我们主要分析js的执行任务

我们可以简单的将事件循环中的js任务分为同步任务和异步任务，而异步任务又分为宏任务和微任务 他们的执行逻辑大多数情况下遵循以下逻辑:

- 在同时存在同步任务和异步任务时，优先执行同步任务
- 在仅有异步任务时，如果同时有需要执行的宏任务和微任务，优先执行微任务

然后，我们需要给自己灌输一个观点，在执行过程中，有以下几个模块：

- 任务执行池: 里面是一条条待执行的js任务，按照任务进入的顺序先进入的先执行，类似队列的先进先出
- 异步任务池：里面存放的是已经过期的异步任务，当当前任务执行池中的任务执行完毕时，就会去异步任务池中拿取任务到任务执行池中，注意，`当一个微任务和一个宏任务的过期时间相同时，优先执行微任务`
- webAPI: 这里是浏览器的一些能力，负责对微任务和宏任务的过期时间进行倒计时，抵达过期时间时将对应任务放到异步任务池中

这里有一个**网站**[1]大家可以在这个网站里直观的感受一下事件循环的工作流程，关于上面提到的`宏任务和微任务`展开来讲有很多可以说的，这里先挖个坑~

接下来我们开看一段代码，试着分析一下当button按钮被点击时 控制台的输出是什么：

```js
const el = document.getElementById("btn")
el.addEventListener("click", () => {
  Promise.resolve().then(() => console.log("microtask 1"));
  console.log("1");
});
el.addEventListener("click", () => {
  Promise.resolve().then(() => console.log("microtask 2"));
  console.log("2");
});
```



最终答案是**1→reslove1→2→resolve2**！

不知道有没有人答对呢，又有多少人的答案是**1→reslove1→2→resolve2**呢 这里可以试着用下列步骤理解js的执行步骤：

1. 当js主线程执行任务的时候，先时发现了两个同步的事件监听绑定任务，这是一次对dom进行事件监听绑定绑定完成后，js任务栈为空，此时按钮已经被绑定了两个事件回调
2. 按钮被点击时，两个鼠标点击事件回调函数被被放入异步任务中的宏任务等待执行队列中，并且把第一个入队的按钮回调函数放入js调用栈中执行，并打印了`1`
3. 当回调函数执行完后，js栈为空了，这时该去异步任务内拿任务了，这里关键来了，`此时会去拿Promise的微任务！`，这里注意我们之前说的，当同时有宏任务和微任务等待执行时，优先执行微任务！所以 接下来打印的就是`reslove1`
4. 然后重复上面步骤，打印`2→resolve2`

通过以上描述，我们就再次强化了一个观念，在事件循环中，每次只会执行一个任务或者一行代码，且当前任务执行完毕后，才会执行下一个任务

大家可以消化一下上述例子，接下来，我们再来看看这个！

```js
const el = document.getElementById("btn")
el.addEventListener("click", () => {
  Promise.resolve().then(() => console.log("microtask 1"));
  console.log("1");
});
el.addEventListener("click", () => {
  Promise.resolve().then(() => console.log("microtask 2"));
  console.log("2");
});
el.click()
```

看到这段代码和之前那段代码的不同了吗，之前是通过用户鼠标点击按钮触发事件回调函数执行，而这段代码是通过js触发dom的clik事件，大家再来试试这段代码会输出什么呢，这里先卖个关子，大家心中有答案的可以在评论区一起交流哦~

> 提示一下：当js调用栈内在执行`document.getElementById("btn").click()`时，按照我们上文的理解，js调用栈内的任务在执行过程中，必须要把当前这条任务执行完毕后，才会执行下一条一人，那么按照这个理解，是不是应该把click的所有事件处理函数执行完毕后，才能将click任务弹栈呢，带着这个理解去思考上面的控制台输出，可能会有不一样的结果哦

# 2事件循环与dom的关系

在事件循环中，与dom相关的任务有很多，例如：

- JavaScript执行时操作DOM
- 异步任务执行时操作DOM
- 在检测到DOM更新后，针对新的style树、html树进行重绘和重排
- 根据新的style树、html树渲染ui视图

## 2.1多久渲染一次dom

通常情况下，我们认为浏览器会每16.6ms~16.7ms渲染一次，这个数字是这样得来的：1000ms(一秒) / 60(普通屏幕一秒的刷新率)，当然在这只是一个大概的数字，为了方便理解，后面我们统一用16ms来表示每一帧的渲染间隔时长

## 2.2不确定的渲染时间

但是还是有例外，例如在某一帧的渲染过程中，js执行了一个计算量超级大的任务，导致js执行过程中消耗了20ms才执行完，这就导致了本来该当前帧执行的任务，由于js占用了过多的时间，导致当前帧的渲染被滞后了，就像这样：

![1709988149185](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709988149185.png)本来每一帧之间的渲染间是16ms，但是在上面这个例子中，由于第三帧渲染之间js执行了一个很耗时的任务，直接导致了第三帧的渲染没有在16ms之内出现，导致了第三帧的渲染滞后了

而由于第三帧的渲染滞后了4ms，导致第三帧与第四帧之间的渲染间隔时间由正常的16ms变为了12ms

这就导致对于肉眼的直观感受就是：在第二帧渲染完成过后，出现了一点卡顿才渲染出第三帧，而且第三帧和第四帧之间的渲染间隔明显变短

这也就是有一些老的js动画库由于历史原因实在没有办法，只能通过`setTimeout(cb, 16.6)`这样的形式来渲染动画，而由于每一帧的渲染时间不确定性导致，本来应该是一个线性匀速的渲染，变成了一个时快时慢的非匀速渲染，就像在这样：

![1709988165490](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709988165490.png)

## 2.3一次dom渲染都干了些什么

![1709988179628](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709988179628.png)上图中每一项任务都有可能消耗很长的时间，这也是导致每一帧dom渲染之间的时间间隔不一致的原因之一

## 2.4一次dom渲染等于几次事件循环

如果我们还记得`1.2`中的内容，我们就会发现，一个完整的、充实的一次事件循环里所做的事情大概就是一次dom渲染做的事情

这样就很容易让我们产生一种误解：一次dom渲染就是一次事件循环

然而真实情况是，一次dom渲染中，`准确来说，是两次dom渲染之间`，事件循环的次数是不确定的，不能被事件循环中存在dom渲染这个任务而被其误导



```
题1：
 console.log(1);
 setTimeout(() => {
   console.log(2);
   Promise.resolve().then(() => {
     console.log(3);
   });
 }, 0);
 new Promise((resolve, reject) => {
   console.log(4);
   resolve(5);
 }).then((data) => {
   console.log(data);
 });
 setTimeout(() => {
   console.log(6);
 }, 0);

答案是：1 4 5 2 3 6
题2：
 setTimeout(function(){
   console.log('1')
 });
 new Promise(function(resolve){
   console.log('2');
   resolve();
 }).then(function(){
   console.log('3')
 });
 console.log('4');

答案：2 4 3 1
Promise对象的resolve部分的代码是当前主线程/宏任务的一部分，并不是微任务，Promise对象的then和catch代码段才是微任务。因此最先输出的是2和4，然后才是微任务队列中的3，最后是宏任务中的1。

题3：
 async function async1() {
   console.log(1); 
   const result = await async2(); 
   console.log(3); 
 } 
 async function async2() { 
   console.log(2); 
 } 
 Promise.resolve().then(() => { 
   console.log(4); 
 }); 
 setTimeout(() => { 
   console.log(5); 
 }); 
 async1(); 
 console.log(6);

答案：1 2 6 4 3 5
该题需要注意的是，由于await方法返回的是一个Promise对象，因此await方法执行完毕后续的代码都应该归入微任务队列，因此**console.log(3)**应该被加入微任务队列等待执行。
题4：
 function sleep(time) {
   let startTime = new Date()
   while (new Date() - startTime < time) {}
   console.log('1s over')
 }
 setTimeout(() => {
   console.log('setTimeout - 1')
   setTimeout(() => {
     console.log('setTimeout - 1 - 1')
     sleep(1000)
   })
   new Promise(resolve => {
    console.log('setTimeout - 1 - resolve')
     resolve() 
   }).then(() => {
     console.log('setTimeout - 1 - then')
     new Promise(resolve => resolve()).then(() => {
         console.log('setTimeout - 1 - then - then')
     })
   })
   sleep(1000)
})
setTimeout(() => {
   console.log('setTimeout - 2')
   setTimeout(() => {
     console.log('setTimeout - 2 - 1')
     sleep(1000)
   })
   new Promise(resolve => resolve()).then(() => {
     console.log('setTimeout - 2 - then')
     new Promise(resolve => resolve()).then(() => {
         console.log('setTimeout - 2 - then - then')
    })
   })
   sleep(1000)
})

浏览器输出为：
setTimeout - 1
setTimeout - 1 - resolve
1s over
setTimeout - 1 - then
setTimeout - 1 - then - then 
setTimeout - 2
1s over
setTimeout - 2 - then
setTimeout - 2 - then - then
setTimeout - 1 - 1
1s over
setTimeout - 2 - 1
1s over
该题需要注意的是微任务执行过程中产生的新的微任务也是追加在当前微任务队列末尾等待执行。
```

