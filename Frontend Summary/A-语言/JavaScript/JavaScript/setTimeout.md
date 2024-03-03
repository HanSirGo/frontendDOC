## setTimeout

```
异步
```

##### JS 中的计时器能做到精确计时吗?为什么?

```
不⾏.
因为:
	1. 计算机硬件没有原⼦钟，⽆法做到精确计时
	2. 操作系统的计时函数本身就有少量偏差，由于 JS 的计时器最终调⽤的是操作系统的函数，也就携带了这些偏差
	3. 按照 W3C 的标准，浏览器实现计时器时，如果嵌套层级超过 5 层，则会带有 4 毫秒的最少时间，这样在计时时间少于 4 毫秒时⼜带来了偏差
	4. 受事件循环的影响，计时器的回调函数只能在主线程空闲时运⾏，因此⼜带来了偏差
```

##### 如何实现准时的setTimeou

```
setTimeout 是不准的。因为 setTimeout 是一个宏任务，它的指定时间指的是：进入主线程的时间。

setTimeout(callback, 进入主线程的时间)
所以什么时候可以执行 callback，需要看 主线程前面还有多少任务待执行。

由此，才有了这个问题。

我们可以通过这个场景来进行演示：
```

![1709458439682](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458439682.png)

```
运行代码如下，通过一个计数器来记录每一次 setTimeout 的调用，而设定的间隔 * 计数次数，就等于理想状态下的延迟，通过以下例子来查看我们计时器的准确性
```

```js
function timer() { 
   var speed = 50, // 设定间隔 
   counter = 1,  // 计数 
   start = new Date().getTime(); 
    
   function instance() 
   { 
    var ideal = (counter * speed), 
    real = (new Date().getTime() - start); 
     
    counter++; 
    form.ideal.value = ideal; // 记录理想值 
    form.real.value = real;   // 记录真实值 
 
    var diff = (real - ideal); 
    form.diff.value = diff;  // 差值 
 
    window.setTimeout(function() { instance(); }, speed); 
   }; 
    
   window.setTimeout(function() { instance(); }, speed); 
} 
timer(); 
```

```js
而我们如果在 setTimeout 还未执行期间加入一些额外的代码逻辑，再来看看这个差值。

... 
window.setTimeout(function() { instance(); }, speed); 
for(var x=1, i=0; i<10000000; i++) { x *= (i + 1); } 
} 
... 
```

![1709458459351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458459351.png)

```
可以看出，这大大加剧了误差。

可以看到随着时间的推移， setTimeout 实际执行的时间和理想的时间差值会越来越大，这就不是我们预期的样子。类比真实的场景，对于一些倒计时以及动画来说都会造成时间的偏差都是不理想的。

这站图可以很好的描述以上问题：
```

![1709458497716](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458497716.png)

###### requestAnimationFrame

> window.requestAnimationFrame() 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。

```
该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行，回调函数执行次数通常是每秒60次，也就是每16.7ms 执行一次，但是并不一定保证为 16.7 ms。

我们也可以尝试一下将它来模拟 setTimeout。
```

```js
// 模拟代码 
function setTimeout2 (cb, delay) { 
    let startTime = Date.now() 
    loop() 
   
    function loop () { 
      const now = Date.now() 
      if (now - startTime >= delay) { 
        cb(); 
        return; 
      } 
      requestAnimationFrame(loop) 
    } 
} 
```

![1709458625026](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458625026.png)

```
发现由于 16.7 ms 间隔执行，在使用间隔很小的定时器，很容易导致时间的不准确。
```

![1709458670798](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458670798.png)

```js
再看看额外代码的引入效果。

... 
 window.setInterval2(function () { instance(); }, speed); 
} 
for (var x = 1, i = 0; i < 10000000; i++) { x *= (i + 1); } 
... 
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458713486.png" alt="1709458713486" style="zoom:50%;" />

```
略微加剧了误差的增加，因此这种方案仍然不是一种好的方案。
```

###### while

```
想得到准确的，我们第一反应就是如果我们能够主动去触发，获取到最开始的时间，以及不断去轮询当前时间，如果差值是预期的时间，那么这个定时器肯定是准确的，那么用 while 可以实现这个功能。

理解起来也很简单：
```

![1709458767647](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458767647.png)

```js
代码如下：

function timer(time) { 
    const startTime = Date.now(); 
    while(true) { 
        const now = Date.now(); 
        if(now - startTime >= time) { 
            console.log('误差', now - startTime - time); 
            return; 
        } 
    } 
} 
timer(5000); 
```

```
打印：误差 0

显然这样的方式很精确，但是我们知道 js 是单线程运行，使用这样的方式强行霸占线程会使得页面进入卡死状态，这样的结果显然是不合适的。
```

###### setTimeout 系统时间补偿

```
这个方案是在 stackoverflow 看到的一个方案，我们来看看此方案和原方案的区别
```

原方案

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458873305.png" alt="1709458873305" style="zoom:50%;" />

setTimeout系统时间补偿

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709458890246.png" alt="1709458890246" style="zoom:50%;" />

```
当每一次定时器执行时后，都去获取系统的时间来进行修正，虽然每次运行可能会有误差，但是通过系统时间对每次运行的修复，能够让后面每一次时间都得到一个补偿。
```

```js
function timer() { 
   var speed = 500, 
   counter = 1,  
   start = new Date().getTime(); 
    
   function instance() 
   { 
    var real = (counter * speed), 
    ideal = (new Date().getTime() - start); 
     
    counter++; 
 
    var diff = (ideal - real); 
    form.diff.value = diff; 
 
    window.setTimeout(function() { instance(); }, (speed - diff)); // 通过系统时间进行修复 
 
   }; 
    
   window.setTimeout(function() { instance(); }, speed); 
} 
```

```
因此通过系统的时间补偿，能够让我们的 setTimeout 变得更加准时，至此我们完成了如何让 setTimeout 准时的探索。
```

```
最后来总结一下4种方案的优缺点
while、Web Worker、requestAnimationFrame、setTimeout 系统时间补偿
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709459031924.png" alt="1709459031924" style="zoom:50%;" />

