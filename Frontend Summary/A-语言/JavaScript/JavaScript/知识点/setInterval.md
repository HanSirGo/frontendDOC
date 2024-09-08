# setInterval缺点

> **千万不要太相信定时器！有时候他会不执行，坑惨你！**``

最近组员遇到了一个 BUG，我们的需求是这样的：**前端需要通过轮询的方式，每隔一段时间去做一些逻辑处理，并向后端发送请求上报**

出现的 BUG 是：**后端发现前端有很长一段时间都没有向后端发起上报请求**

## **问题排查**

我们的的轮询是通过 `setInterval` 定时器去完成的，那么为啥定时器里的逻辑没执行呢？

通过向用户询问，我们得知了用户有很长一段时间没有去看这个前端页面，并且把这个页面给隐藏了

在这里我跟大家科普一个监听事件 `visibilitychange`，他可以用来监听当前标签页页面显隐的切换

![1725108147945](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108147945.png)



于是我需要复现一下这个 BUG ，我简单写了以下的代码，并且做这些事：

- 先在页面待一会
- 隐藏页面一段时间
- 再显示页面，并查看控制台的输出是否连续

![1725108227487](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108227487.png)



我先试着离开很短一段时间，再回去看，可以发现，控制台的输出还是很连续的，这说明离开很短时间，基本不会影响到定时器执行的连续性

![1725108243094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108243094.png)

第二次我离开了一两分钟时间，可以看到定时器执行的连续性不正常了，**这说明了当页面隐藏的时候，定时器的执行有点类似于节流的感觉，从而导致不太连续**

![1725108260736](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108260736.png)

## **定时器连续性的保证？**

其实浏览器这么做也是有他的道理的，毕竟当页面隐藏的时候，说明这个页面对于用户当前来说，不太重要了，所以适当减少一些代码逻辑的运行，节省性能，也是无可厚非的~

但是如果你想一直保证定时器的连续性，还是有办法做到的，可以用到我们的老朋友：**Web Worker**

![1725108308673](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108308673.png)

![1725108326237](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108326237.png)

现在就能保证定时器的连续性执行了~

![1725108338444](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725108338444.png)