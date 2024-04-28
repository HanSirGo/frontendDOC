## JavaScript

#### javascript

```
单线程
解释性语言（相对的还有 编译性语言）
```

###### js单线程

```
JavaScript语言的一大特点就是单线程，即同一时间只能做一件事情。
```

> JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

###### 解释性语言&编译性语言

```
js需要翻译成计算机能读取的语言,这个翻译过程分为编译性语言和解释性语言;
编译性语言: 会将语言编译成另一个文件,交给计算机执行
	优点: 执行快
	缺点:	跨平台
解释性语言: 解释一行，计算机执行一行
	优点: 执行慢
	缺点:	不跨平台
```

###### 浏览器

```
组成:
	shell外核
	内核: 渲染引擎、js引擎
主流浏览器及内核
	ie:	trindent
	chrome: webkit/blink
	firfox: gecko
	opera: presto
	saferi: webkit
```

### JavaScript 是怎么运行起来的

`JavaScript` 的真正的工作原理，希望能够用最简单的描述让大家弄明白整个过程，主要分为下面几个部分：

- 解释型和编译型语言
- JavaScript 引擎
- EcmaScript 和 JavaScript 引擎的关系
- 运行时环境
- 为啥是单线程
- 调用堆栈的执行过程
- JavaScript 语言的解析过程

#### 解释型和编译型语言

大家可能之前都听说过，`JavaScript` 是一种解释型的编程语言，那么啥叫解释型语言呢？

编程语言是用来写代码的，代码是给人看的。计算机只看得懂机器代码（`01010101`），看不懂语言代码。将我们能看得懂的代码转换为计算机可读的机器代码有两种方式：解释和编译。

![1714205605831](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205605831.png)

##### 编译型语言

`编译型语言`直接可以转换为计算机处理器可以执行的机器代码，运行编译型语言需要一个 “`构建`” 的步骤，每次更新了代码你也要重新 “`构建`” 。

它们会比解释语言更快更高效地执行。也可以更好的控制硬件，例如内存管理和 CPU 使用率。但是，在完成整个编译的步骤需要花费额外的时间，生成的二进制代码对平台有一定的依赖性。

常见的编译型语言有 C、C ++、Erlang、Haskell、Rust 和 Go。

##### 解释型语言

`解释型语言` 是通过一个解释器逐行解释并执行程序的每个命令。

因为在运行时翻译代码的过程增加了开销，解释型语言曾经比编译型语言慢很多。但是，随着即时编译的发展，这种差距正在缩小。

但是，解释型语言更灵活一点，并且一般都能动态植入，程序也比较小。另外，因为是通过解释器自己执行源程序代码的，所以代码本身相对于平台是独立的。

常见的解释型语言有 PHP、Ruby、Python 和 JavaScript。

最后再来看看，谁来编译？谁来解释？谁来执行？

- 编译型：编译器来编译，系统执行。
- 解释型：解释器解释并执行。

#### JavaScript 引擎

`JavaScript` 是一种解释型的编程语言，所以源代码在执行之前没有被编译成二进制代码。那么计算机是怎么理解和执行纯文本脚本的呢？

这就是 `JavaScript` 引擎的工作，也就是我们上面提到的`解释器`。

`JavaScript` 引擎是一个执行 `JavaScript` 代码的计算机程序。基本上所有现代浏览器都内置了 `JavaScript` 引擎。当我们的浏览器中加载到 `JavaScript` 文件时，`JavaScript` 引擎会从上到下解析（将其转换为机器码）并执行文件的每一行。

![1714205635873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205635873.png)

每个浏览器都有自己的 `JavaScript` 引擎，其中最著名的引擎是 `Google` 的 `V8`。

`Google Chrome` 和 `Node.js` 的 `JavaScript` 引擎都是 `V8`。下面还有一些其他的常见引擎：

- `SpiderMonkey`：由 `Firefox` 开发，第一款 `JavaScript` 引擎，用于`Firefox`。
- `Chakra`：由微软开发，用于 `Microsoft Edge`。
- `JavaScriptCore`：由苹果开发，用于 `webkit` 型浏览器，比如 `Safari`

所有的 `JavaScript` 引擎都会包含一个调用栈和一个堆：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205647405.png" alt="1714205647405" style="zoom:67%;" />

- 内存堆 - 这是内存分配发生的地方，是一个非结构化的内存池，它存储我们应用程序需要的所有对象。
- 调用堆栈 - 是我们的代码实际执行的地方

#### EcmaScript 和 JavaScript 引擎的关系

`ECMAScript` 指的是 `JavaScript` 的语言标准及语言版本，比如 ES6 表示语言（标准）的第 6 版。它由一个推动 `JavaScript` 发展的委员会制定，这个委员会指的是技术委员会（ `Technical Committee` ）第 `39` 号，我们一般简称 `TC39`，由各个主流浏览器厂商的代表以及一些互联网大厂构成。

`JavaScript` 引擎的核心就是实现 `ECMAScript` 标准，此外还提供一些额外的机制（例如 `V8` 提供的垃圾回收器）。

一些最新的 `ECMAScript` 提案，到达 `stage3` 或 `stage4` 后，就会被 `JavaScript` 引擎实现，例如 `v8` 会把它的一些对语言标准的实现更新在它的博客上：`https://v8.dev/`

#### 运行时环境

`JavaScript` 引擎并不能孤立运行，它需要一个好的运行时环境才能发挥更大的作用，例如 `Node.js` 就是一个 `JavaScript` 运行时环境，各种浏览器也是 `JavaScript` 的运行时环境。

这些运行时环境往往会提供诸如：`事件处理`、`网络请求 API`、`回调队列或消息队列`、`事件循环` 这样的附加能力。

![1714205715392](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205715392.png)

那么 `JavaScript` 引擎怎么配合这些能力在运行时环境中发挥作用呢？我们拿 `Chrome` 来举个例子。

![1714205725538](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205725538.png)

`Chrome` 是一个多进程的架构，我们打开一个浏览器时会启动多个不同的进程协助浏览器将页面为我们呈现出来：

- 浏览器进程：浏览器最核心的进程，负责管理各个标签页的创建和销毁、页面显示和功能（前进，后退，收藏等）、网络资源的管理，下载等。
- 插件进程：负责每个第三方插件的使用，每个第三方插件使用时候都会创建一个对应的进程、这可以避免第三方插件crash影响整个浏览器、也方便使用沙盒模型隔离插件进程，提高浏览器稳定性。
- GPU进程：负责3D绘制和硬件加速
- 渲染进程：浏览器会为每个窗口分配一个渲染进程、也就是我们常说的浏览器内核，这可以避免单个 page crash 影响整个浏览器。

我们常说的浏览器内核，比如 `webkit` 内核，就是浏览器的渲染进程，从接收下载文件后再到呈现整个页面的过程，由浏览器渲染进程负责。浏览器内核是多线程的，在内核控制下各线程相互配合以保持同步，一个浏览器内核通常由以下常驻线程组成：

- GUI 渲染线程：负责渲染浏览器界面 `HTML` 元素,当界面需要重绘(`Repaint`)或由于某种操作引发回流(reflow)时,该线程就会执行。
- 定时触发器线程：浏览器定时计数器并不是由 `JavaScript` 引擎计数的, 因为 `JavaScript` 引擎是单线程的, 如果处于阻塞线程状态就会影响记计时的准确, 因此通过单独线程来计时并触发定时是更为合理的方案。
- 事件触发线程：当一个事件被触发时该线程会把事件添加到待处理队列的队尾，等待JS引擎的处理。这些事件可以是当前执行的代码块如定时任务、也可来自浏览器内核的其他线程如鼠标点击、`AJAX` 异步请求等，但由于JS的单线程关系所有这些事件都得排队等待JS引擎处理。
- 异步http请求线程：`XMLHttpRequest` 在连接后是通过浏览器新开一个线程请求， 将检测到状态变更时，如果设置有回调函数，异步线程就产生状态变更事件放到 `JavaScript` 引擎的处理队列中等待处理。
- `JavaScript` 引擎线程：解释和执行 JavaScript 代码。

> `GUI` 渲染线程与 `JavaScript` 引擎为互斥的关系，当 `JavaScript` 引擎执行时 `GUI` 线程会被挂起， `GUI` 更新会被保存在一个队列中等到引擎线程空闲时立即被执行。

`JavaScript` 是一种单线程编程语言，所以在浏览器内核中只有一个 `JavaScript` 引擎线程。

但是，在 `JavaScript` 的一个运行环境中，因为可能有多个渲染进程，所以可能有多个 `JavaScript` 引擎线程。

详情可以见这篇文章：[浏览器是如何调度进程和线程的？](http://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651562935&idx=2&sn=951311d793a7c61702a5b04848d2d6ab&chksm=80257476b752fd604d58f584b00432faba2bf43751cc7931ba8bc48b33a884125c52f12020d7&scene=21#wechat_redirect)

#### 为啥是单线程

那么，为什么 `JavaScript` 不设计成多个线程呢？这样不是效率更高？

作为浏览器脚本语言， `JavaScript` 的主要用途是与用户互动，以及操作 `DOM`。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定 `JavaScript` 同时有两个线程，一个线程在某个 `DOM` 节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生， `JavaScript` 就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

那么既然 `JavaScript` 本身被设计为单线程，为何还会有像 `WebWorker` 这样的多线程 `API` 呢？我们来看一下 `WebWorker` 的核心特点就明白了：

- 创建 `Worker` 时， JS 引擎向浏览器申请开一个子线程（子线程是浏览器开的，完全受主线程控制，而且不能操作 `DOM`）
- JS 引擎线程与 `Worker` 线程间通过特定的方式通信（`postMessage API`，需要通过序列化对象来与线程交互特定的数据）

所以 `WebWorker` 并不违背 `JS引擎是单线程的` 这一初衷，其主要用途是用来减轻 `cpu` 密集型计算类逻辑的负担。

在单线程上运行代码非常容易，你不必处理多线程环境中出现的复杂场景 — 例如死锁。

#### 调用堆栈的执行过程

`JavaScript` 是一种单线程编程语言，这意味着它有一个调用堆栈，一次只能做一件事。

调用堆栈是一种数据结构，它基本上记录了我们在程序中的位置。如果我们执行一个函数，它放会放在栈顶。如果我们从一个函数返回，其会从栈顶弹出，这就是调用堆栈的执行过程。下面这个动图很好的解释了整个运行过程：

![图片](https://mmbiz.qpic.cn/mmbiz_gif/e5Dzv8p9XdT9LX8nbIUxyBcticAicHwIg5KpWyyhGr8fA8phVd5CXEFrgXb3oyn2xLEnEtLtibOiaIgM0oLCgaf2Jw/640?wx_fmt=gif&tp=wxpic&wxfrom=5&wx_lazy=1)

![1714205798958](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205798958.png)

![1714205815476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205815476.png)

调用堆栈中的每个条目被称为 `堆栈帧`。当调用堆栈中的一个 `堆栈帧` 需要大量时间才能被处理时，就会产生卡顿，因为浏览器没法做其他事情了。

#### JavaScript 代码的执行过程

我们从宏观上看到了 `JavaScript` 调用堆栈是怎么执行的，那么具体到每段代码上是怎么解析执行的呢？

下面我们就以 `V8` 为例，来看看一段 `JavaScript` 代码的解析执行过程。

![1714205830609](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205830609.png)

上面的图展示了 `V8` 大体的工作流程，画的很复杂，我们简化一下，其实核心模块是下面三个：

- 解析器（`Parser`）：负责将 `JavaScript` 代码转换成 `AST` 抽象语法树。
- 解释器（`Ignition`）：负责将 `AST` 转换为字节码，并收集编译器需要的优化编译信息。
- 编译器（`TurboFan`）：利用解释器收集到的信息，将字节码转换为优化的机器码。

在执行 `JavaScript` 代码时，首先解析器会将源码解析为 `AST` 抽象语法树，解释器会将 `AST` 转换为字节码，一边解释一边执行。然后编译器根据解释器的反馈信息，优化并编译字节码，最后生成优化的机器码，这就是 `V8` 大体的工作流程。

#### 词法分析和语法分析

我们常常提到的词法分析和语法分析的过程就是发生在解析器（`Parser`）执行阶段。

`词法分析`就是将字符序列转换为标记（`token`）序列的过程。

所谓 `token` ，就是源文件中不可再进一步分割的一串字符，类似于英语中单词，或汉语中的词。

一般来说程序语言中的 `token` 有：常数（整数、小数、字符、字符串等），操作符（算术操作符、比较操作符、逻辑操作符），分隔符（逗号、分号、括号等），保留字，标识符（变量名、函数名、类名等）等。

比如下面这段代码：

```
const 公众号 = '微信公号名称';
```

经过词法分析后，会被转换为下面这些 `token`：

- `const`（保留字）
- `公众号`（变量名）
- `=（`赋值操操作算符）
- `'``微信公号名称``'`（字符串常数）

`语法分析` 将这些 `token` 根据语法规则转换为 `AST`：

```js
{
  "type": "Program",
  "start": 0,
  "end": 23,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 23,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 6,
          "end": 22,
          "id": {
            "type": "Identifier",
            "start": 6,
            "end": 9,
            "name": "公众号"
          },
          "init": {
            "type": "Literal",
            "start": 12,
            "end": 22,
            "value": "微信公号名称",
            "raw": "'微信公号名称'"
          }
        }
      ],
      "kind": "const"
    }
  ],
  "sourceType": "module"
}
```

在生成 AST 的同时，还会为代码生成执行上下文，在解析期间，所有函数体中声明的变量和函数参数，都被放进作用域中，如果是普通变量，那么默认值是 `undefined`，如果是函数声明，那么将指向实际的函数对象。

#### 字节码和机器码

有了 `AST` 和执行上下文，解释器会将 `AST` 转换为字节码并执行，那么字节码和机器码的区别是啥呢？

![1714205859976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714205859976.png)

- 机器码(machine code)，学名机器语言指令，有时也被称为原生码（Native Code），是电脑的 CPU 可直接解读的数据(计算机只认识0和1)。
- 字节码（byte code）是一种包含执行程序、由一序列 `OP代码(操作码)/数据对` 组成的二进制文件。字节码是一种中间码，它比机器码更抽象，需要直译器转译后才能成为机器码的中间代码。

相比机器码，字节码不仅占用内存少，而且生成字节码的时间很快，提升了启动速度。那么机器码什么时候用到呢？我们在文章开头提到，随着即时编译的发展，解释型语言和编译型语言的运行速度的差距正在缩小。

同时采用了解释执行和编译执行这两种方式，这种混合使用的方式就称为 `JIT` (即时编译)，V8 采用的就是这种技术。

在解释器执行字节码的过程中，如果发现有热点代码，比如一段代码被重复执行多次，这种就称为热点代码，那么后台的编译器就会把该段热点的字节码编译为高效的机器码，然后当再次执行这段被优化的代码时，只需要执行编译后的机器码就可以了，这样就大大提升了代码的执行效率。

## 最后

当然，想要了解更详细的执行机制，可以去看看 `V8` 源码，这篇文章主要带大家捋清楚各种概念，让你能够知道运行一段 `JavaScript` 背后的工作原理，想要更深入的了解，可以看看下面这些文章：。

- https://zhuanlan.zhihu.com/p/383959486
- https://www.zhihu.com/question/268303059
- https://blog.devgenius.io/how-javascript-works-behind-the-scenes-88c546173f32

------



#### 逻辑赋值运算符

```
JavaScript 引入了逻辑赋值运算符（&&=、||= 和 ??=），以简洁的方式将逻辑操作与赋值结合起来。

let x = false;
x ||= true;

console.log(x); // Output: true
```



------

#### 闭包

> 闭包是指有权访问另一个函数作用域内变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以 访问到当前函数的局部变量。

```js
闭包有两个常用的用途。

1. 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。
2. 函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。
```

```js
function a(){
    var n = 0;
    function add(){
       n++;
       console.log(n);
    }
    return add;
}
var a1 = a(); //注意，函数名只是一个标识（指向函数的指针），而（）才是执行函数；
a1();    //1
a1();    //2  第二次调用n变量还在内存中


// 其实闭包的本质就是作用域链的一个特殊的应用，只要了解了作用域链的创建过程，就能够理解闭包的实现原理。
```

```
需要人为销毁（赋值 null）

闭包的优点是：
1.变量被保存起来没有被销毁，随时可以被调用
2.只有函数内部的子函数才能读取局部变量,可以避免全局污染
缺点是：
如果闭包使用不当，就会导致变量不会被垃圾回收机制回收，造成内存泄露
```

#### 垃圾回收机制

------



#### 时间获取

```js
js_date_time(dates) {
            var date = new Date(dates)
            var YY = date.getFullYear() + '-'
            var MM = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
            var DD = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()
            var hh = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':'
            var mm = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
            var ss = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
            return YY + MM + DD + ' ' + hh + mm + ss
        }
```

#### xss攻击

#### 拷贝

```
1.浅拷贝与深拷贝

浅拷贝是创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以如果其中一个对象改变了这个地址，就会影响到另一个对象。
深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且修改新对象不会影响原对象。

浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。

var a1 = {b: {c: {}};

var a2 = shallowClone(a1); // 浅拷贝方法
a2.b.c === a1.b.c // true 新旧对象还是共享同一块内存

var a3 = deepClone(a3); // 深拷贝方法
a3.b.c === a1.b.c // false 新对象跟原对象不共享内存


2.赋值和深/浅拷贝的区别 - (前提都是针对引用类型)

// 对象赋值
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj2 = obj1;
obj2.name = "阿浪";
obj2.arr[1] =[5,6,7] ;
console.log('obj1',obj1) // obj1 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }
console.log('obj2',obj2) // obj2 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

// 浅拷贝
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj3=shallowClone(obj1)
obj3.name = "阿浪";
obj3.arr[1] = [5,6,7] ; // 新旧对象还是共享同一块内存
// 这是个浅拷贝的方法
function shallowClone(source) {
    var target = {};
    for(var i in source) {
        if (source.hasOwnProperty(i)) {
            target[i] = source[i];
        }
    }
    return target;
}
console.log('obj1',obj1) // obj1 { name: '浪里行舟', arr: [ 1, [ 5, 6, 7 ], 4 ] }
console.log('obj3',obj3) // obj3 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

// 深拷贝
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj4=deepClone(obj1)
obj4.name = "阿浪";
obj4.arr[1] = [5,6,7] ; // 新对象跟原对象不共享内存
// 这是个深拷贝的方法
function deepClone(obj) {
    if (obj === null) return obj; 
    if (obj instanceof Date) return new Date(obj);
    if (obj instanceof RegExp) return new RegExp(obj);
    if (typeof obj !== "object") return obj;
    let cloneObj = new obj.constructor();
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        // 实现一个递归拷贝
        cloneObj[key] = deepClone(obj[key]);
      }
    }
    return cloneObj;
}
console.log('obj1',obj1) // obj1 { name: '浪里行舟', arr: [ 1, [ 2, 3 ], 4 ] }
console.log('obj4',obj4) // obj4 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

3.浅拷贝的实现方式
(1)Object.assign()
Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。
let obj1 = { person: {name: "kobe", age: 41},sports:'basketball' };
let obj2 = Object.assign({}, obj1);
obj2.person.name = "wade";
obj2.sports = 'football'
console.log(obj1); // { person: { name: 'wade', age: 41 }, sports: 'basketball' }
(2)函数库lodash的_.clone方法
该函数库也有提供_.clone用来做 Shallow Copy,后面我们会再介绍利用这个库实现深拷贝。
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.clone(obj1);
console.log(obj1.b.f === obj2.b.f);// true
(3)展开运算符...
展开运算符是一个 es6 / es2015特性，它提供了一种非常方便的方式来执行浅拷贝，这与 Object.assign ()的功能相同。
let obj1 = { name: 'Kobe', address:{x:100,y:100}}
let obj2= {... obj1}
obj1.address.x = 200;
obj1.name = 'wade'
console.log('obj2',obj2) // obj2 { name: 'Kobe', address: { x: 200, y: 100 } 
(4)Array.prototype.concat()
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2 = arr.concat();    
arr2[2].username = 'wade';
console.log(arr); //[ 1, 3, { username: 'wade' } ]
(5)Array.prototype.slice()
let arr = [1, 3, {
    username: ' kobe'
    }];
let arr3 = arr.slice();
arr3[2].username = 'wade'
console.log(arr); // [ 1, 3, { username: 'wade' } ]
4.深拷贝的实现方式
(1)JSON.parse(JSON.stringify())
let arr = [1, 3, {
    username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)

①　缺陷
这也是利用JSON.stringify将对象转成JSON字符串，再用JSON.parse把字符串解析成对象，一去一来，新的对象产生了，而且对象会开辟新的栈，实现深拷贝。
这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则，因为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）了。
比如下面的例子：
let arr = [1, 3, {
    username: ' kobe'
},function(){}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)

(2)函数库lodash的_.cloneDeep方法
该函数库也有提供_.cloneDeep用来做 Deep Copy
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);// false

(3)jQuery.extend()方法
jquery 有提供一個$.extend可以用来做 Deep Copy
$.extend(deepCopy, target, object1, [objectN])//第一个参数为true,就是深拷贝
var $ = require('jquery');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = $.extend(true, {}, obj1);
console.log(obj1.b.f === obj2.b.f); // false
(4)手写递归方法
递归方法实现深度克隆原理：遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝。
有种特殊情况需注意就是对象存在循环引用的情况，即对象的属性直接的引用了自身的情况，解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
let obj = { name: 1, address: { x: 100 } };
obj.o = obj; // 对象存在循环引用的情况
let d = deepClone(obj);
obj.address.x = 200;
console.log(d);

深拷贝：递归函数
/**
 * This is just a simple version of deep copy
 * Has a lot of edge cases bug
 * If you want to use a perfect deep copy, use lodash's _.cloneDeep
 * @param {Object} source
 * @returns {Object}
 */
function deepClone(source) {
  if (!source && typeof source !== 'object') {
    throw new Error('error arguments', 'deepClone')
  }
  const targetObj = source.constructor === Array ? [] : {}
  Object.keys(source).forEach(keys => {
    if (source[keys] && typeof source[keys] === 'object') {
      targetObj[keys] = deepClone(source[keys])
    } else {
      targetObj[keys] = source[keys]
    }
  })
  return targetObj
}
```

#### 递归

```
1.递归
(1)缺陷
随着递归深度的增加，创建的堆栈越来越多，导致爆炸堆栈
(2)递归实现九九乘法表
function mul (num) {
If(num == 1) {
Console.log(‘1x1=1’)
} else {
mul(num - 1);
for(var i =1, str = ‘’; i <= num; i ++) {
str += i + ‘x’ + num + ‘=’ + i*num + ‘ ‘
}
Console.log(str)
}
}
mul(9)
2.逆递归（尾递归）
尾递归基于函数的尾调用每个级别的调用都直接返回函数的返回值以更新调用栈，而不创建新的调用栈。
了解尾递归之前，先了解一下尾调用。
尾调用是指一个函数里的最后一个动作是一个函数调用的情形：即这个调用的返回值直接被当前函数返回的情形。这种情形下该调用位置为尾位置。
function foo(data) {
    a(data);
    return b(data);
}   
这里的a(data)和b(data)都是函数调用，但是b(data)是函数返回前的最后运行的东西，所以也是所谓的尾位置。

若一个函数在尾位置调用本身（或是一个尾调用本身的其他函数等），则称这种情况为尾递归，是递归的一种特殊情形。而形式上只要是最后一个return语句返回的是一个完整函数，它就是尾递归。这里注意：尾调用不一定是递归调用，但是尾递归一定是尾调用。
阶乘：
常规的计算阶乘的方法可能是这样的：递归
funtion fac(int n) {
    if (n == 1) {
        return 1;
    }
    return fac(n-1) * n;
}

尾递归的算法是这样的：
function tailfac(int n,int sum) {
    if (n == 1) {
        return sum;
    }
    return fac(n-1, n * sum);
}

缺陷：存在的问题尾部递归优化很好，如果递归深度超过1000,则会报告错误
```

#### 0.1+0.2≠0.3

```js
0.1+0.2的结果不是0.3，而是0.3000000000000000004，JS中两个数字相加时是以二进制形式进行的，当十进制小数的二进制表示的有限数字超过52位时，在JS里是不能精确储存的，这个时候就存在舍入误差。
```

#### 上拉加载下拉刷新

```
上拉加载、下拉刷新 better-scroll
import BScroll from 'better-scroll'
mounted(){
	// 获取 ref 为wrapper的dom
	let wrapper = this.$refs.wrapper
	let scroll = new BScroll(wrapper,{
		//配置项
		//0:默认值，不触发事件
		//1:屏幕滑动超过一定时间后触发scroll事件
		//2:屏幕在滑动过程中触发scroll事件
		//3:屏幕滑动过程中和momentum滚动动画都会触发scroll事件
		//momentum滚动动画：我们用力滑动时，手放开后屏幕还在滑动
		probeType:1,
		//上拉页面，请求更多数据时，配置
		pullUpLoad:true,
		//解决 每项的点击事件不起作用的问题
		click:true
	})
	scroll.on('scroll',eve=>{})
	scroll.on('pullingUp',()=>{ 
		//'上拉加载' ,但只能执行一次
		//可以多次上拉加载
		setTimeout(()=>{
			scroll.finisPullUp()
		},2000)
	 })
}
```

#### 上传

```
接口：export function postInfoList(params: any) {
  console.log(params)
  return serveHttp.post({ url: `${Api.UploadImages}`, params })
}

上传：
handler: async (files: File[]) => {
              console.log(files)
              const formData = new FormData()
              // 'formFiles' 必须和后端 需求的一致，不然返回不出正确数据
              formData.append('formFiles', files[0])
              console.log(formData)
              // You can use any AJAX library you like
              let res = await postInfoList(formData)
              console.log(res)
            },

上传时的name与后端返回的name 一致时
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174351384.png" alt="1709174351384" style="zoom:50%;" />![1709174357269](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174357269.png)

```
上传时的name与后端返回的name 不一致时
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174452173.png" alt="1709174452173" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174467685.png" alt="1709174467685" style="zoom:50%;" />

#### 上传文件并获取内容

```
let reader = new FileReader();
        // readAsText第一个参数不是文件而是选择文件后得到的原生文件对象
        reader.readAsText(fileList._rawValue[0].originFileObj)
        reader.onload = function(e) {
          let data = e.target.result;
          // console.log(data)
          valueRef.value = data
          loading.value = false
          // createMessage.success('导入成功!');
        }
```

#### 读文件FileReader()用法

```
HTML5定义了FileReader作为文件API的重要成员用于读文件，根据W3C的定义，FileReaderr接口提供了读取文件的方法和包含读取 结果的事件模型。
FileReader的方法使用比较简单，可以按照以下步骤创建FileReader对象并调用其他的方法。
1.检测浏览器对FileReader的支持。
例：

```

![1709174730970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174730970.png)



```

2.调用FileReader对象的方法
FileReader的实例拥有四种方法，其中三个用于读取文件，用一个来中断读取，下面的表格列出了这些方法以及他们的参数和功能，需要注意的是，无论读取成功或着失败，方法并不会返回读取结果，这一结果存储在result中。
例：
```

![1709174765141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174765141.png)



```
readAsText：该方法有两个参数，其中第二个参数是文本的编码方式，默认值为 UTF­8。这个方法非常容易理解，将文件以文本方式 读取，读取的结果即是这个文本文件中的内容。
readAsBinaryString：该方法将文件读取为二进制字符串，通常我们将它传送到后端，后端可以通过这段字符串存储文件。
readAsDataURL：这是例子程序中用到的方法，该方法将文件读取为一段以 data: 开头的字符串，这段字符串的实质就是 Data URL， Data URL是一种将小文件直接嵌入文档的方案。这里的小文件通常是指图像与 html 等格式的文件。
3.处理事件
FileReader包含了一套完整的事件模型，用于捕获读取文件时的状态，下面的这个表格归纳了这些事件。
例：
```

![1709174794947](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174794947.png)



```
文件一旦开始读取，无论成功或失败，实例的result都会被填充。如果读取失败，则result的值问Null值，否则就是读取成功的结果，绝大多数的程序都会在成功读取文件的时候，抓取这个值。
例：
```

![1709174817623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174817623.png)

```
下面通过一个上传图片预览和带进度条上传来展示FileReader的使用。
例：
```

![1709174841066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174841066.png)

```
如果要上传文件的类型，可以通过文件选择器获取文件对象并通过Type属性来检查文件类型。
例：
```

![1709174871008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174871008.png)

```
不难发现这个检测是基于正则表达式的，因此可以进行各种复杂的匹配。
```

#### 函数防抖与节流

```js
函数防抖和函数节流：优化高频率执行js代码的一种手段，js中的一些事件如浏览器的resize、scroll，鼠标的mousemove、mouseover，input输入框的keypress等事件在触发时，会不断地调用绑定在事件上的回调函数，极大地浪费资源，降低前端性能。为了优化体验，需要对这类事件进行调用次数的限制。
﻿
1.函数防抖
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
// 这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。
var timer; // 维护同一个timer
function debounce(fn, delay) {
    clearTimeout(timer);
    timer = setTimeout(function(){
        fn();
    }, delay);
}

在上面的代码中，会出现一个问题，var timer只能在setTimeout的父级作用域中，这样才是同一个timer，并且为了方便防抖函数的调用和回调函数fn的传参问题，我们应该用闭包来解决这些问题。
function debounce(fn, delay) {
    var timer; // 维护一个 timer
    return function () {
        var _this = this; // 取debounce执行作用域的this
        var args = arguments;
        // 如果此时存在定时器的话，则取消之前的定时器重新记时
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(function () {
            fn.apply(_this, args); // 用apply指向调用debounce的对象，相当于_this.fn(args);
        }, delay);
    };
}
﻿
2.函数节流
每隔一段时间，只执行一次函数。
// 函数节流 是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。
function throttle(fn, delay) {
    var timer;
    return function () {
        var _this = this;
        var args = arguments;
        if (timer) {
            return;
        }
        timer = setTimeout(function () {
            fn.apply(_this, args);
            timer = null; // 在delay后执行完fn之后清空timer，此时timer为假，throttle触发可以进入计时器
        }, delay)
    }
}

函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，定时器实现节流函数通过使用定时任务，延时方法执行。在延时的时间内，方法若被触发，则直接退出方法。从而，实现函数一段时间内只执行一次。
﻿
3.异同比较
相同点：
都可以通过使用 setTimeout 实现。
目的都是，降低回调执行频率。节省计算资源。
不同点：
函数防抖，在一段连续操作结束后，处理回调，利用clearTimeout 和 setTimeout实现。函数节流，在一段连续操作中，每一段时间只执行一次，频率较高的事件中使用来提高性能。
函数防抖关注一定时间连续触发的事件只在最后执行一次，而函数节流侧重于一段时间内只执行一次。
﻿
4.常见应用场景
函数防抖的应用场景
连续的事件，只需触发一次回调的场景有：
搜索框搜索输入。只需用户最后一次输入完，再发送请求
手机号、邮箱验证输入检测
窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。
函数节流的应用场景
间隔一段时间执行一次回调的场景有：
滚动加载，加载更多或滚到底部监听
谷歌搜索框，搜索联想功能
高频点击提交，表单重复提交
```

> 《轻松理解 JS 函数节流和函数防抖》
>
> 《JavaScript 事件节流和事件防抖》
>
> 《JS 的防抖与节流》

#### Eventloop

```
1.Eventloop
Eventloop即事件循环，是指浏览器或者node的一种解决javascript单线程运行时不会阻塞的一种机制，也是我们经常使用的异步的原理。
无论是同步还是异步任务，都是在主线程执行
2.宏任务（macrotask）与微任务（microtask）
JS代码中的异步任务可进一步分为宏任务（macrotask）与微任务（microtask）。 
宏任务包括：script代码、setTimeout、setInterval、I/O、UI render 
微任务包括：promise.then、Object.observe(已废弃)、MutationObserver
3.执行顺序
1.同步任务加入同步任务队列，放到主线程中执行，宏任务和微任务会被加入各自的队列中。 
2.当主线程执行完毕后，浏览器会先去清空微任务队列，依次取出微任务队列中的微任务执行，执行过程中如果产生新的微任务，则追加到当前微任务队列的末尾等待执行。 
3.当微任务队列清空后，浏览器会从宏任务队列中取出一个宏任务执行。
4.宏任务执行完毕再去清空微任务队列，微任务队列清空后再取出一个宏任务来执行。
5.如此反复，直至宏任务队列和微任务队列全部清空。
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


需要注意的是：
宏任务队列和微任务队列都遵循先进先出原则，先被加入队列的任务优先被取出执行
Promise对象的resolve部分不是微任务，then和catch部分才是，即
new Promise((resolve,reject) => {
  console.log('同步');
  resolve(); 
}).then(() => {
  console.log('异步');
}) 
微任务执行过程中产生的新的微任务追加到当前微任务队列队尾等待本轮事件循环执行
await方法返回的是一个Promise对象，因此await方法执行完毕，后续代码都应归入微任务队列(可结合题4理解)
Node环境的事件循环与浏览器不完全一致
```

#### 跨页面通信

```

```

#### new 过程

> `new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。`new` 关键字会进行如下的操作：
>
> 1. 创建一个空的简单JavaScript对象（即{}）；
> 2. 链接该对象（即设置该对象的构造函数）到另一个对象 ；
> 3. 将步骤1新创建的对象作为this的上下文 ；
> 4. 如果该函数没有返回对象，则返回this。

```js
function Dog(name, color, age) {
  this.name = name;
  this.color = color;
  this.age = age;
}

Dog.prototype={
  getName: function() {
    return this.name
  }
}

var dog = new Dog('大黄', 'yellow', 3)
```

```js
// 我们来看最后一行带new关键字的代码，按照上述的1,2,3,4步来解析new背后的操作。

//第一步：创建一个简单空对象

var obj = {}

//第二步：链接该对象到另一个对象（原型链）
// 设置原型链
obj.__proto__ = Dog.prototype

// 第三步：将步骤1新创建的对象作为 this 的上下文
// this指向obj对象
Dog.apply(obj, ['大黄', 'yellow', 3])

// 第四步：如果该函数没有返回对象，则返回this
// 因为 Dog() 没有返回值，所以返回obj
var dog = obj
dog.getName() // '大黄'

// 要注意的是如果 Dog() 有 return 则返回 return的值

var rtnObj = {}
function Dog(name, color, age) {
  // ...
  //返回一个对象
  return rtnObj
}

var dog = new Dog('大黄', 'yellow', 3)
console.log(dog === rtnObj) // true
```

```js
//接下来我们将以上步骤封装成一个对象实例化方法，即模拟new的操作：

function objectFactory(){
    var obj = {};
    //取得该方法的第一个参数(并删除第一个参数)，该参数是构造函数
    var Constructor = [].shift.apply(arguments);
    //将新对象的内部属性__proto__指向构造函数的原型，这样新对象就可以访问原型中的属性和方法
    obj.__proto__ = Constructor.prototype;
    //取得构造函数的返回值
    var ret = Constructor.apply(obj, arguments);
    //如果返回值是一个对象就返回该对象，否则返回构造函数的一个实例对象
    return typeof ret === "object" ? ret : obj;
}
```

#### webworker

#### setTimeout、Promise、async/await 三者之间异步解决方案的区别？

#### JavaScript 继承的几种方式及优缺点？

#### fetch 是否可以共享 Cookie?两个 then 分别对应着什么？

#### async/await的优缺点



#### 闭包、垃圾回收和内存泄漏、数组方法、数组乱序, 数组扁平化、事件委托、事件监听、事件模型

#### 前端安全（CSRF、XSS）

