# Devtools

作为开发人员，平时用的最多的就是Chrome Devtools了，但是可能很多同学平时用的最多也就只是Console和Elements面板了。

# 前言

Chrome DevTools 是每位前端开发者的得力助手，它提供了丰富的功能和工具，帮助我们诊断和调试网页应用程序。本文将分享一些 Chrome DevTools 的调试技巧，在开发中提高效率。

# 模拟接口响应和网页内容

通过本地覆盖可以模拟接口返回值和响应头，无需 mock 数据工具，无需等待后端支持，快速复现在一些数据下的 BUG 等。在 DevTools 可以直接修改你想要的 Fetch/XHR 接口数据，还可以修改响应头，解决跨域等问题，不仅可以覆盖 Fetch/XHR，JS、CSS 等资源也可以。

本地覆盖其实在之前的版本就已经有了，需要在本地手动创建目录，步骤麻烦。**Chrome 117** 对本地覆盖功能简化，现在可以直接在 `Network` 面板快速模拟远程资源的内容和响应头！

设置本地覆盖步骤：

1. 打开 DevTools，导航至 `Network` 网络面板，右键单击要覆盖的请求，从下拉菜单中选择 Override content 或 Open in Sources panel。
2. 如果是首次使用，未设置过本地覆盖文件目录，DevTools 会在顶部的操作栏中提示您选择一个文件夹来存储覆盖文件，并 “允许” 授予 DevTools 对其的访问权限（在 window 下选择了系统盘的文件夹测试发现用不了，可能是权限问题，建议选个非系统盘的文件夹）。
3. 在 Sources 面板中修改数据，完成后按 Ctrl + S 保存，刷新页面，即可看见修改后的数据（被覆盖的资源在图标右下角会有个紫色的圆点）。
4. 若要恢复使用服务上的数据，请导航到 `Sources` > `Overrides`，可以点击取消 “Enable local overrides” 复选框，或者点击旁边的 Clear 图标，或者如上图演示中的单个删除。

它是怎么工作的？

- 当你在 DevTools 中进行更改时，DevTools 会将修改后的文件的副本保存到您指定的文件夹中。
- 当你重新加载页面时，DevTools 会提供本地修改后的文件，而不是网络资源。所以在旧版本支持 Override 的版本中，也可以手动创建一个文件来覆盖内容。

# 快速重发请求

在联调接口或者排查 BUG 的时候，经常需要重新再发一次请求，如果要重新操作一次复杂的交互、重新输入一大堆参数时，这种方式会显得比较麻烦。

这时候就可以通过 `Replay XHR` 来快速重发请求，如下图演示：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/U0WXqkJLdLSwibxHNUSoTagm3CiaxBt6c2o14yicAiamq4Mak2V4U3ZRrE2xTZErmfnKCghKbLjmEWPSUC4ciaaPygg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

操作步骤：

1. 导航至 `Network` 网络面板，右击一个 XHR 请求，可以点击 Fetch/XHR 过滤。
2. 点击 Replay XHR。

# 在 Console 中发请求

针对上面同样的场景，有时候我们需要修改请求头、入参再重新发起请求，那么 Replay XHR 就不支持了。

可以通过 Copy as fetch ，在控制台修改请求参数，发起请求，如下图演示：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/U0WXqkJLdLSwibxHNUSoTagm3CiaxBt6c25JibbARZgJrlwF7Cr3JqTae1tfOCG1rfscjJSLtf8lct1M74e1nK6Pw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

操作步骤：

1. 导航至 `Network` 网络面板，右击一个 XHR 请求，可以点击 Fetch/XHR 过滤。
2. 点击 Copy -> Copy as fetch。
3. 导航至 `Console` 面板，Ctrl + v 粘贴。
4. 修改内容，如接口 url、header、body， 然后按回车键即可发起请求。

# 条件断点

如果我们在某个循环中，希望循环到某个条件的时候在进入断点。
此时我们就可以使用 conditional breakpoint 如下图演示：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/U0WXqkJLdLSwibxHNUSoTagm3CiaxBt6c2tvwlvL1X2LIo4Y5BRWXqWMGxiaLibQBBP1MXyX93nKs308SVMhJvPhqw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

操作步骤：

1. 右击代码行，点击 Add conditional breakpoint，或者按住 Ctrl + 鼠标左点击代码行。
2. 输入条件表达式。

> Add log point 为条件日志，可以不用在代码中写 console.log。

# Console 中的 $

## `$0-$4`

当要在 Console 中在调试页面元素时，比如要获取元素的信息，此时就可以使用 4。

**$0：**当前选择的元素 ，**$1：**上一次的引用，**$2：**上上次的引用，一直到 **$4**

例如获得某个元素相对于其 **offsetParent**[2] 元素的顶部内边距的距离，演示如下：

![图片](https://mmbiz.qpic.cn/sz_mmbiz_gif/U0WXqkJLdLSwibxHNUSoTagm3CiaxBt6c2icyIM7vlgECicoObA30VRz7kxAiapXfsJ7iazu0n9CmACZv8pKZ1s5pTWw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

操作步骤：

1. 点击菜单栏第一个选择图标，或者使用快捷键 Ctrl + Shift + C 选择元素。
2. 导航至 `Console` 面板，现在就可以使用 4，例如选择了两次，第一次选择的元素可以使用 访问，第二次选择的元素使用0 访问。

## `$ 和 $$`

```
$(‘xxx’) 相当于 document.querySelector(‘xxx’)  $$(‘xxx’) 相当于 Array.form(document.querySelectorall(‘xxx’))
```

如下图演示：

![1726305964712](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726305964712.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

## `$_`

调试的过程中，经常需要打印一些变量值，但是如果想查看上一次执行的结果，使用 $_ 是对上次执行结果的引用。

![1726305908920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726305908920.png)

## `$i`

现在的前端开发过程，离不开各种 npm 包，如果我们要测试一个 npm 包，需要先在本地npm init，然后再 npm install xxx。

Console Importer 插件可以帮助我们，在控制台使用 $i(‘xxxx’) 进行装包，并进行调用。

![1726305932415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726305932415.png)

# Element 面板

## 隐藏 DOM Element

有时候我们想截图，但是想要隐藏图中的敏感信息，此时就可以隐藏元素，如下图演示：

![1726306011159](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726306011159.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

操作步骤：

```
1. 右键 DOM Element，点击 Hide Element。也可以使用键盘快捷键，点击选中 Element 后，按键盘 H 键。
```

## 一键展开所有 DOM

在调式 DOM Element 的时候，如果 DOM 层次比较深的情况下，一个个去展开就比较麻烦，我们可以使用快捷键 Alt + Click 一键展开该层下的所有 DOM，如下图演示：

![1726306043125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726306043125.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

## 拖拽移动 DOM Element

当我们想看页面某一部分元素在不同的位置显示效果的时候，可以直接拖拽 DOM 元素调整位置，也可以使用键盘快捷键 Ctrl + 上下箭头。如下图演示：

![1726306057166](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726306057166.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

# 截图

如果想要截取多屏很长的整个页面内容，系统自带的截图软件显然不支持，此时可以使用 command 命令截图，如下图演示：

![1726306078155](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726306078155.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)

操作步骤：

1. 按 Ctrl + Shift + P 调出 command 命令
2. 输入命令 capture full size screenshot 后回车。

截图 command：
截框选区域：**capture area screenshot**
截滚动全屏：**capture full size screenshot**
截选中的节点：**capture node screenshot**
截当前窗口内：**capture screenshot**

还有很多其他的命令，如切换主题 Switch to Dark theme，查看所有快捷键 Show shortcuts 等等。

# 总结

本文介绍了 Chrome DevTools 的调试技巧和最新版本 Chrome 117 中的新功能，包括模拟接口响应和网页内容、快速重发请求、在 Console 中发请求、Console 中的快捷命令、条件断点、Element 面板以及截图命令，这些调试技巧有助于我们在开发中提高效率。

> 原文地址：https://www.ruanchaomin.com/blog/posts/244