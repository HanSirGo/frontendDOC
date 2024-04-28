# 选择第三方 NPM 包

每个开发人员都使用过 `npm install` 安装依赖。截止目前 NPM 平台上已经托管超过 190w 个包了，面对茫茫多的 package，在选择第三方 NPM 包时应该关注些什么？

## 1. 检查开源许可证（License）

开源许可证是一种法律许可。通过它，版权拥有人明确允许，用户可以免费地使用、修改、共享版权软件。

版权法默认禁止共享，也就是说，没有许可证的软件，就等同于保留版权，虽然开源了，用户只能看看源码，不能用，一用就会侵犯版权。所以软件开源的话，必须明确地授予用户开源许可证。

![1714201944475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714201944475.png)

> 可以在 NPM 平台上查看包的 License

License 通常分两大类："CopyLeft（**著佐权**）" 和 "Permissive（**宽松式**）"：

- CopyLeft：如果你使用了这个包，那么你的代码也必须开源。这对一些商业化的项目不太友好，比如 GPL 和 LGPL 协议。
- Permissive：对包做了最低限度的使用限制，允许闭源，可以说几乎没有限制，但是各自又有一些区别，具体见下图。

![1714201980848](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714201980848.png)

我们可以用 **license-checker**[1] 工具来一次性检查项目中的 NPM 包是否都是合规的。

## **2. 看贡献频率和下载**量

NPM 平台也可以查看包的每周下载量和趋势图，数字越大意味着使用的人越多。

![1714202039783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202039783.png)

由于社区里有很多功能类似的包，比如 `moment.js` 和 `dayjs` 这类时间日期库。**NPM trends**[2] 可以同时比较多个包的下载量增长趋势，从而更直观地了解它们的受欢迎程度。

![1714202056969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202056969.png)

> 另外，GitHub 仓库的 stars、 forks、 commit frequency 和 contributors 等相关指标也可以侧面反映受欢迎程度。

## **3. 权衡包体积大小**

对于前端来说，优化性能最直接的手段就是**降低代码包大小**。所以选择第三方包时，也要考虑它的包体积。

**bundlephobia**[3] 可以对包体积进行可视化分析。从下图可以看出，`moment.js` 打包后会有将近 300 kB，gzip 压缩后会有 70kB。

![1714202092289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202092289.png)

而 `dayjs` 打包后只有 **6 kB**，远小于 `moment.js`。

![1714202108783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202108783.png)

对于像微信小程序这样对代码包大小有明确限制的项目来说，轻量级的 `dayjs` 是更好的选择。

## **4. 是否有大型团队在进行维护**

尽管大部分的 NPM 包都包含较全的文档，但是往往也都存在一些兼容性、JS 异常或性能相关的 issue。因此如果背后有大型团队在进行维护，我们使用起来会更放心。

可以关注以下几个方面：

### **Used By 和 Contributors 数量**

数量越多意味着你不是早期用户，很多开发者已经帮你趟过水了。

![1714202138978](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202138978.png)

### **Contributors 视图**

可以直观地看到贡献者的贡献频率，如果很多都频繁贡献，意味着仓库受到社区更多的关注和支持。

![1714202170956](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202170956.png)

###  **Comm**unity Standards

将仓库与开源社区的规范标准进行比较，达到的标准越多意味着这个仓库越成熟。

![1714202196932](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202196932.png)

## **5. 评估安全性**

包安全性是选择 NPM 包的另一个关键因素，在仓库的「**Security**」tab 下可以看到它的安全策略。

![1714202216401](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202216401.png)

如果维护者已经采取了措施来确保安全性，会显示**已激活**。

> 对于项目中已有的 NPM 依赖，可以使用命令 `npm audit` 来进行安全性检查。

**npmgraph**[4] 能够对包进行依赖可视化分析来确保没有安全漏洞，然后再安装到项目当中。

![1714202229273](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714202229273.png)



**总结**

回顾一下选择第三方 NPM 包的 5 条最佳实践：

- 检查开源许可证
- 看贡献频率和下载量
- 权衡包体积大小
- 是否有大型开发团队在进行维护
- 评估安全性

在做选择时，我们最好能根据以上维度产出一份调研文档，并在团队内分享和讨论，最终得出一致的结论。