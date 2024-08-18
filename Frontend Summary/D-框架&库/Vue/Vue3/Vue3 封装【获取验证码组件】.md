# Vue3 如何封装一个合格的【获取验证码组件】

## 获取验证码组件

最近封装了一个 **获取验证码** 的组件，虽然算是一个比较小的组件，但是还是感觉比较有意思的，大致效果如下

![图片](https://mmbiz.qpic.cn/mmbiz_gif/TZL4BdZpLdgj61WJzFoFeV5oLJ3C49zxb1ibPwWwnzqLKRJjeonUXIYsb1Qjysns1xCRabuqRLibExUepOrqZLjw/640?wx_fmt=gif&from=appmsg&wxfrom=13&tp=wxpic)



## 最基础的组件

我们可以从简单的开始，先实现一个比较简单基础的组件，后面再去完善它，最基础的代码如下

![1723968837938](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968837938.png)

这个时候可以看到这个组件的雏形已经出来了！

![1723968893062](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968893062.png)

### 实现倒计时

接下来我们需要写一个 `useCountDown` 的 Hooks ，来编写倒计时的逻辑~大家在风中组件的时候，要有这种习惯，就是把一些逻辑比较紧密的代码抽取到一个 Hooks 中去

> 注意：`setState` 是异步更新，所以想获取最新值需要使用`useRef`

![1723968937969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968937969.png)

封装完后可以到组件中去使用

![1723968957514](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968957514.png)



这样就能达到想要的倒计时效果，但是还是有很多不足，比如按钮没有禁用，而且尺寸不能自定义，也没有响应式把验证码内容暴露出去

![1723968972317](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968972317.png)

## 完善组件封装

上面的组件封装的太简单了，一个好的组件需要具有比较好的自定义性、拓展性，所以我们需要完善一下组件的封装

可以把这个组件拆成两个部分：

- CountdownInput输入框
- CountButton按钮

### CountdownInput输入框

![1723968993417](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723968993417.png)

### CountButton

![1723969013222](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723969013222.png)



## 最终使用

![1723969030425](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723969030425.png)



效果如下

![1723969045622](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723969045622.png)