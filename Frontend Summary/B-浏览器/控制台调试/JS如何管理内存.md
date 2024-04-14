# JavaScript 如何管理内存

内存影响性能。当你的应用程序代码消耗太多内存时，你会得到一个卡顿的应用程序，甚至可能会出现内存耗尽的崩溃。

当你在web浏览器中运行一个应用程序时，大部分应用程序内存存储在Javascript堆中，而这个堆位于计算机的RAM内。在 Vue 或者 React 应用程序的上下文中，存储在JS堆中的常见对象包括：组件实例、虚拟DOM，以及如果你使用状态管理工具（Vuex、Pinia、Redux、mobx）的话，包含 State、Action 等。

当你与一个应用程序进行交互时，JS堆可能会随着时间的推移而增长。你可以通过使用Chrome DevTools进行剖析时观察到这种增长。

![1712991485078](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991485078.png)

由于内存增长会影响应用程序的性能，因此清除未使用的内存以释放空间非常重要。Javascript引擎回收未使用内存的过程称为垃圾回收（GC）。与某些语言（如C和C++）不同，Javascript引擎会自动进行垃圾回收。

让我们从Chrome开始分析。

## **Javascript 与 Major GC 和 Minor GC**

### **Major GC 和 Minor GC**

> Major GC（ 主垃圾回收器 ）：主要负责老生代的垃圾回收；
>
> Minor GC（ 副垃圾回收器 ）：主要负责新生代的垃圾回收；

在浏览器中能找到他们（Major GC、Minor GC）之间的工作身影：

- JS HEAP SIZE 明显降低的时候，必是Major GC在工作

![1712991522344](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991522344.png)

- 反观 Minor GC，则没 Major工作这么明显，但是Minor GC工作会比Major频繁得多

![1712991552563](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991552563.png)

### **Javascript使用 Major GC 和 Minor GC 清理JS堆**

打开Chrome开发者工具，并导航到性能（Performance）选项卡，确保选中“内存（Memory）”复选框。

![1712991566578](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991566578.png)

这是Chrome分析器生成的配置文件：

![1712991583462](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991583462.png)

你可以看到堆的大小会增长和缩小。如果你仔细观察，你会发现堆大小的下降与垃圾回收活动是对应的。"Major GC" 表示正在进行一次主要的垃圾回收。

![1712991601580](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991601580.png)

当JS堆大小大幅下降时，你很可能会在任何网站上看到这种模式。事实上，如果你在单页应用程序中切换标签页时，JS堆的大小没有显著减少，但标签页的内容有所不同，这可能是内存泄漏的迹象。

在剖析中，你还会看到“Minor GC”帧。

这些 Major GC 和 Minor GC 帧向你揭示了一些关于Javascript中垃圾回收的信息。

- 垃圾回收有两种类型：Major GC 和 Minor GC。
- Minor GC 通常很短暂，而 Major GC 则需要更长的时间。
- 在“任务”下看到它们作为帧的事实表明它们都在主应用程序线程上运行。
- 因为它们在主线程上运行，它们会阻塞Javascript执行，因为浏览器只有一个主线程来运行你的Javascript代码。
- 由于它们阻塞了Javascript执行，过多的GC会影响应用程序的性能。

注意：GC 只有在某些时候会阻塞Javascript执行。它并不是一直阻塞，而是在显示为帧时。我们在Chrome分析器中无法看到这一点，但在Firefox分析器中可以看到。

现在让我们来看看Firefox分析器，它也揭示了Major GC和Minor GC之间的差异。

### **Major GC和Minor GC清理JS堆的两个不同部分**

我不是Firefox的用户，我之所以将Firefox下载到我的笔记本电脑上，仅仅是因为它的剖析功能。Firefox的剖析器是开源的，并且比Chrome的剖析器有更多的可视化功能。

录制一个剖析文件，并导航到“Marker Chart”可视化。Marker Chart本质上是所有不同类型活动的时间轴，包括垃圾回收活动。

![1712991620051](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991620051.png)

你可能会注意到的第一件事是，Minor GC 发生频率相当高，而 Major GC 则不是。这是有很好的原因的，因为如前面所述，Minor GC 很短暂，而 Major GC 则需要更长的时间。

如果你将鼠标悬停在一个 Minor GC 切片上，你会得到以下描述：

![1712991634298](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991634298.png)

因此，除了它们以不同的频率运行外，Major GC 和 Minor GC 还清理JS堆的不同部分。

如上所述，Minor GC 清除了所谓的“苗圃集合(`nursery collection`)”。 如果对象在 Minor GC 中幸存下来，它们就会从 Nursery 移动到“终身(`tenured`)”堆或“长期(`long-lived`)”堆上。 因此，Major GC 必定是清除永久堆的对象。

Major GC通过一种名为mark—and—sweep的算法来清理永久化的堆。

![1712991665357](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991665357.png)

现在我们知道 JS 堆至少有 2 个组件：

- 一个用于最近分配的对象的苗圃集合(`nursery collection`)
- 一个用于长期存在的对象的永久堆。(“终身(`tenured`)”堆或“长期(`long-lived`)”堆)

## **主要GC算法（增量标记和清除）**

Marker Chart 最后一行的屏幕截图中还有一个 GCSlide。 嗯，那是什么？

如果将鼠标悬停在 Major GC 矩形上，你会看到 Major GC 的描述：

![1712991686292](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991686292.png)

整个Major GC花费了580毫秒。然而，正如描述所说，主线程仅在个别切片上被阻塞。因此，最后一行的GCSlide指的是这些阻塞切片。再往下看，你会发现总切片时间是34.4毫秒，因此在Javascript主线程无法执行应用程序代码时，只有34.4毫秒的工作。

如果你将鼠标悬停在其中一个GC切片上，你会得到：

![1712991701477](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991701477.png)

请注意，两个工具提示都有“标记阶段”和“清除阶段”。这意味着Javascript垃圾回收有两个阶段：标记和清除。这个算法称为标记-清除。请注意，GC切片的描述中提到了“增量垃圾回收的一个切片”。增量GC是实现标记-清除算法的一种方式。

标记阶段通过遍历对象图来“标记”所有从根集合（例如全局变量、活动函数调用）可达的对象。然后，清除阶段“扫描”整个堆，收集并释放所有未标记的对象的内存，这些对象被视为不可达，因此是垃圾。

![1712991719593](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991719593.png)

增量意味着你在标记阶段以增量方式进行操作，从而最小化主线程被阻塞的时间。

## **Chrome vs. Firefox**

当涉及到Javascript中的垃圾回收时，我想提出一些细微差别。

Chrome（以及NodeJS）使用V8 Javascript引擎。

另一方面，Firefox使用一个名为SpiderMonkey的不同引擎。正是Javascript引擎带来了垃圾回收。V8使用术语“young generation”代替“nursery”，以及“old generation”代替“tenured heap”。然而，V8和SpiderMonkey都使用Minor GC来清理年轻对象和Major GC来清理老对象。

两者都实现了增量标记-清除算法用于Major GC。不过，具体的 **增量标记-清除算法** 在V8和SpiderMonkey之间有所不同。

总结一下：

![1712991742219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712991742219.png)

