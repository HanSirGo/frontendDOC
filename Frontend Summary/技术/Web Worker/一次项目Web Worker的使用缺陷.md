# 第一次弃用了 Web Worker ，因为它救不了我

> **第一次弃用了 Web Worker ，因为它救不了我**``

## **背景**

最近有一个客户那边提了一个很有意思的优化点

先说说背景吧，我们是一个 Vue3 的项目，客户那边习惯把同一个项目通过多个浏览 Tab 去打开，这样就能很方便去使用多个页面，比如下面这样（**简单例子，实际是两个长得很不一样的页面**）

![1723899813784](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899813784.png)



其实客户这么做也能理解，因为有些客户就是不喜欢在同一个 Tab 标签页中去切换菜单，他们觉得切来切去很麻烦

虽然可能这两个页面长得很不一样，但是有一小部分长得很相似的（但是还是有不同，所以没封装成组件）

![1723899823500](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899823500.png)



这部分相同的逻辑依赖的是同一个接口，同一个处理数据的逻辑，并且这两个过程都很耗时：

- 请求接口：3000ms，因为后端取数据逻辑很复杂，返回数据量很大！
- 数据处理：300ms，前端拿到数据得处理一下

所以客户那边提出了：**既然都是同一个逻辑，那么能不能将页面一的数据给页面二、页面三、页面四用啊？**

## **分析优化点**

先抛开客户提出的要求，我们先想想有什么优化点

`接口请求` 和 `数据处理` 都是比较耗时和数据量大的操作，所以我一开始是想放到 `WebWorker` 中去做的

但是当客户提出优化点后，我就发现不能用`WebWorker` 去做，原因如下：

- 1、每个 Tab 的`WebWorker`都是独立的，无法进行数据共享
- 2、就算用 `WebWorker + IndexedDB` 去做数据缓存共享，但是却很难共享`数据状态`

第一个原因很容易理解，那第二个原因可能有些人不理解，**为什么要共享数据状态呢？**

看下图就懂了，为什么要共享数据状态呢？因为你要让页面二、页面三、页面四这些页面知道你数据的状态，有点类似于 Promise，状态有 `未缓存、缓存中、已缓存`

![1723899846995](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899846995.png)



还是拿刚刚那个例子来说，两种情况：

![1723899860084](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899860084.png)



- **情况一：** 页面一打开并点击按钮进行数据请求处理和缓存，再去点击页面二的按钮，那么页面二肯定能拿到缓存的数据，这种情况没问题
- **情况二：** 页面一点击按钮1秒后，去点击页面二的按钮，这个时候页面二是不知道页面一的数据状态的，所以页面二不知道是要发起请求呢，还是要去等页面一请求完呢

所以**共享数据状态**很重要

## **SharedWorker**

最终抛弃了 `WebWorker`，选择了 `SharedWorker`

![1723899874547](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899874547.png)

`SharedWorker`是啥呢？你可以理解为： `SharedWorker` 类似于 `WebWorker` ，只不过`SharedWorker` 能让多个 Tab 标签页共享

多个 Tab 连同一个`SharedWorker` 的时候， `SharedWorker`会通过 port 来管理每一个 Tab，可以说：**一个 Tab 就是一个 port**

我们通过一个小案例来演示一下 `SharedWorker`，我们看看 `count`能不能被两个页面共享

![1723899903028](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899903028.png)



![1723899913470](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899913470.png)

可以看到 `count`被共享了~

![图片](https://mmbiz.qpic.cn/mmbiz_gif/TZL4BdZpLdhFeHET5GoE4l4Sgiabiaia2kRic1BtowTcuPzCZcz1bWSuBSE6iaTSt0pnN3hrEoRkFrPNh7pSRLWib5icg/640?wx_fmt=gif&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1)



现在我们把 `shared-worker.js` 里的逻辑改成请求数据和处理数据的代码

![1723899947194](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899947194.png)



![图片](https://mmbiz.qpic.cn/mmbiz_gif/TZL4BdZpLdhFeHET5GoE4l4Sgiabiaia2kRvBbwKMnvvIBZfIvJTFHbEvibxVTleCyOIKVTWmsonDx1aKicBwX1oVPA/640?wx_fmt=gif&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1)



但是不对吧，这样写的话共享不到数据状态啊！！这样相当于是请求了两次，跟没优化一样。。。页面一先点，后再点页面二，按理说如果共享了，应该是同时出现数据才对，显然现在还没实现最终效果

所以我们需要实现数据状态共享，其实很简单，利用`Promise`就行了~

![1723899973070](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723899973070.png)



这样才是最终效果，不同时点击，但是出现数据确是同时的

![1723900006214](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723900006214.png)