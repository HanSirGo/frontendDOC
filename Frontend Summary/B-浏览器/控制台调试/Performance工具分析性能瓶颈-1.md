# 使用Performance工具分析性能瓶颈，解决页面卡顿！

对于前端的性能优化，优化手段其实是非常多的，但是不能盲目的进行优化，一定要先分析出项目的性能瓶颈，否则只会做无用功。那么如何才能更好的分析出项目的瓶颈呢？其实最关键的就是要分析页面的整个加载过程，找出有问题的地方再进行优化。使用谷歌浏览器自带的 Performance 工具可以帮我们解决这个问题，下面通过一个例子来进行分析优化！

在优化之前，我们先要了解一些知识，比如浏览器的渲染帧、Performance 工具的使用，这样才有助于更好地理解优化的过程！

## **浏览器的渲染帧**

对于渲染，我们首先需要了解一个概念：设备刷新率。设备刷新率是设备屏幕渲染的频率，通俗一点就是，把屏幕当作墙，设备刷新率就是多久重新粉刷一次墙面。基本我们平常接触的设备，如手机、电脑，它们的默认刷新频率都是 60FPS，也就是屏幕在 1s 内渲染 60次，约 16.7ms 渲染一次屏幕。这就意味着，我们的浏览器最佳的渲染性能就是所有的操作在一帧 16.7ms 内完成，能否做到一帧内完成直接决定着渲染性，影响用户交互。

浏览器的 fps 指浏览器每一秒的帧数，fps 越大，每秒的画面就越多，浏览器的显示就越流畅。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978726017.png)

标准渲染帧：在一个标准帧渲染时间 16.7ms之内，浏览器需要完成 Main 线程的操作，并 commit 给 Compositor 进程

![1709978736483](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978736483.png)

丢帧：主线程里操作太多，耗时长，commit 的时间被推迟，浏览器来不及将页面 draw 到屏幕，这就丢失了一帧

![1709978753460](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978753460.png)

在浏览器的一个渲染帧（16.7ms）里，会存在一段时间，叫做空闲时间（idle period），如果完成各种任务的执行以及页面渲染的工作等的时间少于 16.7 ms，那么这一帧就会存在空闲时间，可以把一些耗时操作拆分开来，然后在每一帧的空闲时间中去执行。

![1709978770728](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978770728.png)

所谓的页面卡顿、首屏加载慢，根本原因都是执行长任务，使得页面的渲染时机推后，在每一帧里得不到渲染，从而造成用户的不好体验。要想解决用户体验差的问题，那么就需要知道浏览器渲染过程中的每一帧都干了些啥任务，是啥原因导致渲染时机推后，这个时候我们就需要借助浏览器性能检测工具 Performance 来进行分析，然后再做针对性的优化，下面我们来了解下该工具的具体使用。

## **Performance工具的使用**

![1709978792258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978792258.png)

**简单的使用教程：**

在 Chrome 浏览器中，按下 F12 键或右键点击页面并选择 "检查"，打开开发者工具面板。

在开发者工具中，切换到 "Performance"（性能）选项卡，你会看到一个记录性能数据的界面。

![1709978810667](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978810667.png)

点击页面顶部的 "Record"（录制）按钮，开始记录性能数据，刷新页面或执行你想要分析的操作。

![1709978826952](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978826952.png)

在你完成操作后，点击 "Stop"（停止）按钮，停止记录性能数据。此时，会看到一个包含了各种性能数据的时间轴图表。

![1709978849777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978849777.png)

时间轴图表将显示页面加载期间的各种事件，如 JavaScript 执行、网络请求、绘制等。可以缩放和选择特定时间段来深入分析。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978872823.png)



## **性能优化分析的例子**

谷歌官方提供的一个 Demo：**googlechrome.github.io/devtools-sa…**[2]

左侧有一些按钮，点击 Stop 小球停止运动，点击 Add、Subtract 可以控制小球数量的增减，比较有意思的一个点是，当小球的数量越来越多，页面会越来越卡顿，如果点击 Optimize（优化），那么页面就会恢复正常。

### **优化前的效果展示**

当小球数量很多时，页面会非常卡顿：

![1709978892510](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978892510.png)



### **使用工具分析性能**

接下来借助 Performance 来分析页面卡顿的原因：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978913181.png)

录制大概 4 秒钟，可以看到该页面的性能确实存在很大的问题，我们首先分析这张图里面的一些内容：

**总览区：** 可以看到每个阶段的具体耗时，这里很明显是渲染占据了 90% 的时间，而 JS 脚本的执行、页面绘制并不耗时，现在已经可以定位到是渲染存在问题。

![1709978930323](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978930323.png)

**帧：** 绿色代表该帧正常，黄色表示丢帧

![1709978946825](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978946825.png)

**主线程：**

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978959242.png)

以其中的一个 Task 为例：标红代表该任务是长任务（一般认为超过 50ms 的任务是长任务），往下是该任务具体的细节，比如这个 Task 里主要执行了 Animation Frame Fired 方法，它里面调用了 Function Call，Function Call 里面调用了 app.update 的方法，一层一层往下调用执行，然后在 app.update 下面我们可以看到很多紫色的线条，**紫色代表回流重绘**。

![1709978978674](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978978674.png)

现在可以初步下结论：频繁的回流重绘导致页面卡顿，后面还要再进行分析才能确定。

接下来点击其中的一个任务，观察 Call Tree，每个方法的执行时间都能看到，以及时间的占比

![1709978999168](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709978999168.png)

我们的分析目标主要是寻找花费时间长的任务，依次点开，可以发现 90% 的时间是花费在 Layout，点击右侧进入源码：

![1709979014554](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709979014554.png)

![1709979035487](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709979035487.png)

分析这段代码我们已经可以知道问题出在哪里了，**读取offsetTop会触发回流重绘**，这里用了个 for 循环，所以当小球的数量越来越多的时候，不断的读取 offsetTop 属性，导致频繁的触发回流重绘，最终页面卡顿。

### **频繁的回流重绘导致卡顿**

我们需要解答两个问题：

1. 为什么频繁的回流重绘会导致卡顿？

**计算复杂度：** 回流涉及到重新计算元素的位置和几何属性，这可能需要遍历整个DOM树，并重新计算样式。这个计算过程比较复杂，尤其是在大型、复杂的页面上。

**渲染的停顿：** 当发生回流时，浏览器可能需要停止渲染，重新计算布局，然后再重新绘制，这可能导致页面的停顿或闪烁。

**频繁触发：** 如果在用户与页面交互的过程中频繁地触发回流和重绘，可能会导致性能问题。比如，在滚动页面时，如果频繁改变元素的样式，可能会引起多次回流和重绘，从而影响流畅度。

也就是说，频繁的回流重绘可以看做是耗时严重的任务，阻碍了页面的渲染，从而导致卡顿！

1. 为什么读取 offsetTop 属性会触发回流重绘？

这与浏览器的优化机制有关：由于每次回流与重绘都会带来额外的计算消耗，为了优化这个过程，大多数浏览器采用了队列化修改并批量执行的策略。浏览器会将修改操作添加到队列中，直至一定时间段过去或操作达到阈值时，才会清空队列。

然而，当需要获取布局信息时，浏览器会强制刷新队列。这意味着，当你读取元素的布局信息如 offsetTop、offsetLeft 等时，需要返回最新的布局信息，因此浏览器不得不清空队列，触发回流和重绘操作以返回正确的值。

### **如何进行优化**

那么如何进行优化呢？知道是 **offsetTop** 的问题后，不用这个属性就行了，我们看下这个例子的处理方式：

![1709979055122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709979055122.png)

使用 style.top 属性取代 offsetTop 即可，当然这两个属性也有一定的区别，这里不再展开，这样就能完美的解决页面卡顿的问题！

总的来说，其实优化方法很简单，最关键的是要找出页面的性能瓶颈，到底是哪里影响了性能。因此，前端开发者想要提升自己的能力、提升自己对性能优化的理解，就必须具备熟悉浏览器的渲染原理、使用 Performance 工具、分析项目的性能瓶颈等方面的能力！