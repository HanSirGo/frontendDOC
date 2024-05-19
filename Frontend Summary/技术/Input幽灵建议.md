## Vue3实现一个超级有趣的功能：Input幽灵建议

## 实现

有两部分组成：

- 输入的文本
- 提示的文本

并且这两种文本需要叠在一起，且提示文本需要设置透明度，这样才能区分两种文本，大致是以下的效果

![1716028659972](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716028659972.png)

代码如下，这里需要注意，外层标签需要使用 `label`，这样建议文本叠在输入文本上的时候，才不会影响`input`框的正常输入

![1716028677714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716028677714.png)

接着我们准备一些假数据，以及绑定 Input 的 Tab 键盘事件，记得阻止默认行为，因为按 Tab 键会导致 Input 失焦~

![1716028688755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1716028688755.png)