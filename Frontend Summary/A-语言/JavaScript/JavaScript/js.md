## JavaScript

#### javascript

```
单线程
解释性语言（相对的还有 编译性语言）
```

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

## globalThis 全局对象

```js
globalThis对象提供了一种在不同环境下（包括浏览器和Node.js）访问全局对象的一致方式。

console.log(globalThis === window); // 在浏览器场景下: true
console.log(globalThis === global); // 在 Node.js 中: outputs: true
```

#### This指向问题

```
执行上下文对象 context

this 执行上下文对象，永远指向当前函数的调用者

1.全局环境下的this 是window  全局环境是 onload 方法执行产生的，而onload 的使用者是window
2.当一个函数使用的时候看前面有没有 . ,有指向.前边的对象，没有就是window
3.构造函数的this 指向 new 出来的实例对象
4.计时器中this 指向 window
5.箭头函数中没有 this 指向问题，指向父级函数的调用者
6.call bind apply 改变 this 指向
```

```js
const obj1 = {
	foo(){
		console.log(this)
	}
}
const fn = obj1.foo
fn() // window

const obj2 = {
    foo: function() {
        function bar() {
            console.log(this)
        }
        bar()
    }
}
obj2.foo() // window

```

```js
关于this的总结：
	
	1. 沿着作用域向上找最近的一个function (不是箭头函数),看这个function最终是怎样执行的;
	2. this的指向取决于所属function的调用方式，而不是定义;
	3. function调用一般分为以下几种情况:
	
		1>. 作为函数调用,即: foo()
			指向全局对象(globalThis,浏览器环境是window,node环境global);
			注意严格模式问题,严格模式下是undefined;
			
		2>. 作为方法调用,即: foo.bar()/foo.bar.baz()/foo['bar']/foo[0]()
			指向最终调用这个方法的对象
			
		3>. 作为构造函数调用,即: new Foo()
			指向新对象
			
		4>. 特殊调用,即: foo.call()/foo.apply()/foo.bind()
			参数指定成员
			
	4. 找不到所属的function,就是全局对象
```

```js
var length = 10
function fn () {
    console.log(this.length)
}

const obj = {
    length:5,
    method (fn) {
        fn()
        arguments[0]()
    }
}
obj.method(fn,1,2) // 10 3
```

```
nodejs 环境
	由于文件代码中最顶层的作用域实际上是一个伪全局环境,所以最顶层的this并不指向全局对象
严格模式
	严格模式下原本应该指向全局的this都会指向undefined
	
```

------

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

## 浏览器渲染原理

#### 渲染流⽔线

![1709368957397](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709368957397.png)

##### 1. 解析 HTML - Parse HTML

![1709369297855](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369297855.png)

![1709369323668](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369323668.png)

![1709369596810](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369596810.png)

###### HTML 解析过程中遇到 CSS 代码怎么办？

```
为了提⾼解析效率，浏览器会启动⼀个预解析器率先下载和解析 CSS
```

![1709369397683](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369397683.png)

###### HTML 解析过程中遇到 JS 代码怎么办？

```
渲染主线程遇到 JS 时必须暂停⼀切⾏为，等待下载执⾏完后才能继续预解析线程可以分担⼀点下载 JS 的任务
```

![1709369456249](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369456249.png)



##### 2. 样式计算 - Recalculate Style

![1709369669971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369669971.png)

##### 3. 布局 - Layout

![1709369704277](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369704277.png)

###### DOM 树 和 Layout 树不⼀定是⼀⼀对应的

![1709369790528](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369790528.png)

![1709369819566](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369819566.png)

![1709369838280](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369838280.png)

##### 4. 分层 - Layer

![1709369872897](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369872897.png)

##### 5. 绘制 - Paint

```
这⾥的绘制，是为每⼀层⽣成如何绘制的指令
```

![1709369929870](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369929870.png)

```
渲染主线程的⼯作到此为⽌，剩余步骤交给其他线程完成
```

![1709369958636](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709369958636.png)

##### 6. 分块 - Tiling

```
分块会将每⼀层分为多个⼩的区域
```

![1709370000856](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370000856.png)

```
分块的⼯作是交给多个线程同时进⾏的
```

![1709370034218](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370034218.png)

##### 7.光栅化 - Raster

```
光栅化是将每个块变成位图
优先处理靠近视⼝的块
```

![1709370080674](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370080674.png)

```
此过程会⽤到 GPU 加速
```

![1709370110244](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370110244.png)

##### 8. 画 - Draw

```
合成线程计算出每个位图在屏幕上的位置，交给 GPU 进⾏最终呈现
```

![1709370151406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370151406.png)

###### 完整过程

![1709370225415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370225415.png)

###### 浏览器是如何渲染页面的？

```
当浏览器的网络线程收到 HTML 文档后，会产生一个渲染任务，并将其传递给渲染主线程的消息队列。

在事件循环机制的作用下，渲染主线程取出消息队列中的渲染任务，开启渲染流程。
```

```
整个渲染流程分为多个阶段，分别是： HTML 解析、样式计算、布局、分层、绘制、分块、光栅化、画

每个阶段都有明确的输入输出，上一个阶段的输出会成为下一个阶段的输入。

这样，整个渲染流程就形成了一套组织严密的生产流水线
```

```
渲染的第一步是**解析 HTML**。

解析过程中遇到 CSS 解析 CSS，遇到 JS 执行 JS。为了提高解析效率，浏览器在开始解析前，会启动一个预解析的线程，率先下载 HTML 中的外部 CSS 文件和 外部的 JS 文件。

如果主线程解析到`link`位置，此时外部的 CSS 文件还没有下载解析好，主线程不会等待，继续解析后续的 HTML。这是因为下载和解析 CSS 的工作是在预解析线程中进行的。这就是 CSS 不会阻塞 HTML 解析的根本原因。

如果主线程解析到`script`位置，会停止解析 HTML，转而等待 JS 文件下载好，并将全局代码解析执行完成后，才能继续解析 HTML。这是因为 JS 代码的执行过程可能会修改当前的 DOM 树，所以 DOM 树的生成必须暂停。这就是 JS 会阻塞 HTML 解析的根本原因。

第一步完成后，会得到 DOM 树和 CSSOM 树，浏览器的默认样式、内部样式、外部样式、行内样式均会包含在 CSSOM 树中。
```

```
渲染的下一步是**样式计算**。

主线程会遍历得到的 DOM 树，依次为树中的每个节点计算出它最终的样式，称之为 Computed Style。

在这一过程中，很多预设值会变成绝对值，比如`red`会变成`rgb(255,0,0)`；相对单位会变成绝对单位，比如`em`会变成`px`

这一步完成后，会得到一棵带有样式的 DOM 树。
```

```
接下来是**布局**，布局完成后会得到布局树。

布局阶段会依次遍历 DOM 树的每一个节点，计算每个节点的几何信息。例如节点的宽高、相对包含块的位置。

大部分时候，DOM 树和布局树并非一一对应。

比如`display:none`的节点没有几何信息，因此不会生成到布局树；又比如使用了伪元素选择器，虽然 DOM 树中不存在这些伪元素节点，但它们拥有几何信息，所以会生成到布局树中。还有匿名行盒、匿名块盒等等都会导致 DOM 树和布局树无法一一对应。
```

```
下一步是**分层**

主线程会使用一套复杂的策略对整个布局树中进行分层。

分层的好处在于，将来某一个层改变后，仅会对该层进行后续处理，从而提升效率。

滚动条、堆叠上下文、transform、opacity 等样式都会或多或少的影响分层结果，也可以通过`will-change`属性更大程度的影响分层结果。
```

```
再下一步是**绘制**

主线程会为每个层单独产生绘制指令集，用于描述这一层的内容该如何画出来。
```

```
完成绘制后，主线程将每个图层的绘制信息提交给合成线程，剩余工作将由合成线程完成。

合成线程首先对每个图层进行分块，将其划分为更多的小区域。

它会从线程池中拿取多个线程来完成分块工作。
```

```
分块完成后，进入**光栅化**阶段。

合成线程会将块信息交给 GPU 进程，以极高的速度完成光栅化。

GPU 进程会开启多个线程来完成光栅化，并且优先处理靠近视口区域的块。

光栅化的结果，就是一块一块的位图
```

```
最后一个阶段就是**画**了

合成线程拿到每个层、每个块的位图后，生成一个个「指引（quad）」信息。

指引会标识出每个位图应该画到屏幕的哪个位置，以及会考虑到旋转、缩放等变形。

变形发生在合成线程，与渲染主线程无关，这就是`transform`效率高的本质原因。

合成线程会把 quad 提交给 GPU 进程，由 GPU 进程产生系统调用，提交给 GPU 硬件，完成最终的屏幕成像。
```



###### 什么是 reflow(回流) 

```
reflow 的本质就是重新计算 layout 树。

当进行了会影响布局树的操作后，需要重新计算布局树，会引发 layout。

为了避免连续的多次操作导致布局树反复计算，浏览器会合并这些操作，当 JS 代码全部完成后再进行统一计算。所以，改动属性造成的 reflow 是异步完成的。

也同样因为如此，当 JS 获取布局属性时，就可能造成无法获取到最新的布局信息。

浏览器在反复权衡下，最终决定获取属性立即 reflow。
```

![1709370264620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370264620.png)

###### 什么是 repaint(重绘) 

```
repaint 的本质就是重新根据分层信息计算了绘制指令。

当改动了可见样式后，就需要重新计算，会引发 repaint。

由于元素的布局信息也属于可见样式，所以 reflow 一定会引起 repaint。
```

![1709370294744](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370294744.png)

###### 减少页面重绘和回流

```
1. 尽量减少使用 CSS 属性的快捷方式：例如，使用 border-width、border-style 和 border-color 而不是 border。CSS 属性的快捷方式会将所有值初始化为“初始值”，因此避免使用它们可以最小化重绘和回流（在实际工作中，CSS 快捷方式的性能影响微乎其微，并且使用快捷方式可以简化样式并解决一些样式覆盖问题）。
2. 在 GPU 上渲染动画：浏览器已经优化了 CSS 动画，使其适用于触发动画属性的重绘（因此也包括回流）。为了提高性能，将具有动画效果的元素移动到 GPU 上。可以触发 GPU 硬件加速的 CSS 属性包括 transform、filter、will-change 和 position:fixed。动画将在 GPU 上处理，提高性能，特别是在移动设备上（但避免过度使用，因为可能会导致性能问题）。
3. 使用 will-change CSS 属性来提高性能：它通知浏览器元素需要修改的属性，使浏览器能够在实际更改之前进行优化（但避免过度使用 will-change；在动画中遇到性能问题时考虑使用它）。
4. 通过更改 className 批量修改元素样式。
5. 将复杂的动画元素定位为 fixed 或 absolute 以防止回流。
6. 避免使用表格布局：因为在表格元素上触发回流会导致其中所有其他元素的回流。
7. 使用 translate 而不是修改 top 属性来上下移动 DOM 元素。
8. 创建多个 DOM 节点时，使用 DocumentFragment 进行一次性创建。
9. 必要时为元素定义高度或最小高度：没有显式高度，动态内容加载可能会导致页面元素移动或跳动，从而导致回流（例如，为图像定义宽度和高度以防止布局变化并减少回流）。
10. 尽量减少深度嵌套或复杂选择器的使用，以提高 CSS 渲染效率。
11. 对元素进行重大样式更改时，暂时使用 display:none 隐藏它们，进行更改，然后将它们设置回 display:block。这样可以最小化回流，只需两次即可。
12. 使用 contain CSS 属性将元素及其内容与文档流隔离，防止其边界框外意外副作用的发生。
```



###### 为什么 transform 效率⾼

```
因为 transform 既不会影响布局也不会影响绘制指令，它影响的只是渲染流程的最后一个「draw」阶段

由于 draw 阶段在合成线程中，所以 transform 的变化几乎不会影响渲染主线程。反之，渲染主线程无论如何忙碌，也不会影响 transform 的变化。
```

![1709370338573](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370338573.png)

![1709370350303](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709370350303.png)

#### 逻辑赋值运算符

```
JavaScript 引入了逻辑赋值运算符（&&=、||= 和 ??=），以简洁的方式将逻辑操作与赋值结合起来。

let x = false;
x ||= true;

console.log(x); // Output: true
```



#### 闭包

```
闭包：一个函数执行完毕后，返回一个匿名函数，把这个匿名函数赋值给外部变量，

这个外部变量与匿名函数产生引用关系，使这个函数无法被销毁，一直存在内存中，

就形成了闭包。需要人为销毁（赋值 null）

闭包的优点是：
1.变量被保存起来没有被销毁，随时可以被调用
2.只有函数内部的子函数才能读取局部变量,可以避免全局污染
缺点是：
如果闭包使用不当，就会导致变量不会被垃圾回收机制回收，造成内存泄露
```

#### 垃圾回收机制

------



#### JSON的方法

```
1.JSON.parse()
2.JSON.stringify()
3.缺点
```

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

```
函数防抖和函数节流：优化高频率执行js代码的一种手段，js中的一些事件如浏览器的resize、scroll，鼠标的mousemove、mouseover，input输入框的keypress等事件在触发时，会不断地调用绑定在事件上的回调函数，极大地浪费资源，降低前端性能。为了优化体验，需要对这类事件进行调用次数的限制。
﻿
1.函数防抖
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
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

#### 异步 async与defer

```
有时会js先执行，就会阻塞dom渲染，使用异步; 如果依赖dom加载，就用deder,否则就用async

defer在解析html文档的同时加载脚本文件并且不会阻塞html文档的解析,html文档解析完成再去执行脚本程序;
async在解析html文档的同时加载脚本文件并且不会阻塞html文档的解析,等待脚本文化加载完毕之后会立即执行并且阻塞html文档的解析;

区别:
	defer加载完毕脚本文件不会立即执行会等待html文档解析完毕
	async则不会等待文档加载完毕，在脚本文件加载完成的时候会立即执行
```

#### 跨页面通信

```

```

#### new 过程

```
1.创建一个this对象，在空对象上添加属性 
2.隐式返回 this对象 , 
如果构造函数return 一个引用数据类型{}/[]，new 得到的就是这个引用数据 
3.this指向new 构造函数的实例 
4.实例的__proto__指向其构造函数的prototype
```

