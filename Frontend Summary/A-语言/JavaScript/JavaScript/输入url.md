# 从输入URL到页面显示中间经历了什么？

**一个经典的问题：** 从输入URL到页面显示中间经历了什么？

在分析整个流程前先了解下浏览器的整体架构

![1709967278124](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967278124.png)image.png

## **浏览器进程**

主要负责界面显示、用户交互、子进程管理，同时提供存储等功能。

## **渲染进程**

核心任务是将 HTML、CSS 和 JavaScript 转换为用户可以与之交互的网页，排版引擎 Blink 和 JavaScript 引擎 V8 都是运行在该进程中，一般一个 tab 页对应一个渲染进程。

## **GPU 进程**

GPU 的使用初衷是为了实现 3D CSS 的效果，随后网页、Chrome 的 UI 界面都选择采用 GPU 来绘制，这使得 GPU 成为浏览器普遍的需求。最后，Chrome 在其多进程架构上也引入了 GPU 进程。

## **网络进程**

主要负责页面的网络资源加载。

## **插件进程**

主要是负责插件的运行，因插件易崩溃，所以需要通过插件进程来隔离，以保证插件进程崩溃不会对浏览器和页面造成影响。

# **流程分析**

回到正题，我们将用户输入一个 url 到页面渲染完成拆分成几个阶段来看。

## **处理用户输入**

主要是接受用户的信息输入，一般分以下两种情况

- 关键字搜索，例如查询 **字节跳动 ,** chrome 会自动拼成完整的查询 URL。**www.google.com/search?q=字节…**[1]

- 输入网址:

- - **bytedance.net**[2]

## **网络进程获取资源**

该阶段主要获取渲染所需资源，一般入口是 html 文件，随后根据 html 文件的内容拉取其他类型资源。

- 缓存: 浏览器缓存、服务端缓存等，从缓存中读取文件可大大降低渲染时间。

- 根据域名查询 DNS 获取服务 IP 地址，建立 TCP 连接。

- 可能存在的 3xx 重定向

- 不同的 Content-Type 触发不同的动作，例如

- - text/html, html 文件，触发渲染流程
  - application/octet-stream 触发下载流程

## **资源解析处理**

### **Dom 解析**

解析 html 文件内容，生成 DOM 树结构。

![1709967329128](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967329128.png)

### **样式计算（Calculate Style）**

1. 把 css 转换为浏览器能理解的结构: styleSheets （可通过 document.styleSheets 查看)

1. 属性值标准化, 例如 background: black => background: rgb(0,0,0)

1. 计算出 DOM 树中每个节点的具体样式（继承&层叠）

![1709967375891](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967375891.png)

![1709967358613](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967358613.png)

### **布局阶段（Layout Tree）**

通过上面的处理我们得到了结构、样式相关的两棵树，接下来需要将二者结合，生成布局树

1. 去除 DOM 中包含的不可见节点，如 head、link 和设置了 display:none 的节点

1. 计算各个节点的具体布局位置

![1709967542637](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967542637.png)

### **分层（Layout Tree）**

生成 Layout Tree 后并不会立刻开始渲染流程，考虑到页面会有一些复杂的 3D 效果、页面滚动和动画等，需要为某些节点生成单独的图层，并生成一棵对应的图层树 (LayerTree), 就像 PS 中的图层一样，正是这些图层叠加在一起构成了最终的页面图像。

在 Chrome 开发者工具 -> Layer 面板可查看当前页面的分层情况。

![1709967564390](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967564390.png)

上面是 baidu 首页的 layer 情况，可以看到页面的图层数量，点击图层可查看详情信息，包括生成图层的原因。

**Q:** 哪些情况下会生成 Layer ？
**A:** 一般涉及 动画、滚动、或者产生层叠上下文时会生成图层，具体原因可在面板中查看
**Q:** 为什么要生成 Layer？
**A:** 可以减少repaint、reflow，提升性能， Layer上的动画在合成线程上执行，不受主线程上任务干扰，更加流畅; 举个🌰，下面是分别通过 js 脚本和 css animation 实现的动画效果,可以看到在性能上是有明显差异的

![1709967631435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967631435.png)

![1709967650209](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967650209.png)

Tips：Layer 的生成和维护也会占用资源，生成过多的 layer 会适得其反。

### **图层绘制**

在完成图层树的构建之后，渲染引擎会对图层树中的每个图层进行绘制，试想一下，如果给你一张纸，让你先把纸的背景涂成蓝色，然后在中间位置画一个红色的圆，最后再在圆上画个绿色三角形。你会怎么操作呢？

1. 绘制蓝色背景

1. 在中间绘制一个红色的圆

1. 再在圆上绘制绿色三角形

------

**渲染引擎实现图层的绘制与之类似，会把一个图层的绘制拆分成很多小的绘制指令，然后再把这些指令按照顺序组成一个待绘制列表。**

![1709967693905](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967693905.png)

### **栅格化（raster）**

1. 提交绘制指令到合成线程

   ![1709967704551](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967704551.png)

2. 分块(tile)

通常一个页面可能很大，但是用户只能看到其中的一部分，我们把用户可以看到的这个部分叫做视口（viewport）。在有些情况下，有的图层可以很大，比如有的页面你使用滚动条要滚动好久才能滚动到底部，但是通过视口，用户只能看到页面的很小一部分。所以在这种情况下，要绘制出所有图层内容的话，就会产生太大的开销，而且也没有必要。基于这个原因，合成线程会将图层划分为图块（tile），这些图块的大小通常是 **256x256** 或者 **512x512** **。**

![1709967735999](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967735999.png)

3. 合成位图

合成线程按照视口附近的图块来优先生成位图，实际生成位图的操作是由栅格化来执行的，栅格化就是指将图块转换为位图。通常，栅格化过程都会使用 GPU 来加速生成，使用 GPU 生成位图的过程叫快速栅格化，或者 GPU 栅格化，生成的位图被保存在 GPU 内存中。

![1709967760811](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967760811.png)

### **合成显示**

等到所有的图块光栅化都完成时触发绘制命令，将页面内容绘制到内存中并最终显示到屏幕上。

### **总结**

![1709967780055](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967780055.png)

# **Repaint & Reflow**

最后再聊聊两个概念，前端小伙伴们应对 repaint 和 reflow 不陌生，其实就是修改了节点的一些属性如背景颜色，宽高、字体大小等，触发了页面的重新渲染，只是 repaint 和 reflow 的处理过程不太一样。

## **Repaint**

修改了元素的背景颜色，那么布局阶段将不会被执行，因为并没有引起几何位置的变换，所以就直接进入了绘制阶段，然后执行之后的一系列子阶段，这个过程就叫**重绘**。

![1709967799158](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967799158.png)

## **Reflow**

通过 JavaScript 或者 CSS 修改元素的几何位置属性，例如改变元素的宽度、高度等，那么浏览器会触发重新布局，解析之后的一系列子阶段，这个过程就叫**重排**。

![1709967822049](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709967822049.png)

