# 关闭全局 Input 的自动拼写校验

## 自动校验

> 注：以下**输入框**包含`input、textarea`

事情是这样的，上个星期，接到了一个需求，要求去除掉项目中的输入框的**自动拼写检查**功能，也就是下图出现的红线，这个检查是浏览器自带的

![1718450414212](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450414212.png)

## 解决方法

其实是有解决方法的，而且也不难，很简单，只需要在**输入框**标签上加上一个属性`spellcheck=false`即可，也就是：

![1718450425816](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450425816.png)

![1718450433204](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450433204.png)

## 解决思路

但是问题来了，我需要给全局的**输入框**标签都加上才行，由于本项目是使用了antd-design这个组件库，那我们来看看整个项目都有哪些组件包含了**输入框**标签呢？

- 1、Input：包含input
- 2、Select：包含input
- 3、InputNumber：包含input
- 4、Textarea：包含textarea
- 5、body：直接在 body 上加 spellcheck="true"

## 多种解决方法

### 1、ConfigProvider

![1718450450157](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450450157.png)

ant-design官方提供了一个组件，可以用来为全局的组件进行统一配置，这个组件可以传入一个`input`的参数，即可配置全局的**Input**标签

![1718450461921](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450461921.png)

也就是：

![1718450471844](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450471844.png)

由于这个配置只针对全局的**Input**，所以结果都有哪些组件成功去掉了拼写校验功能呢：

- 1、Input：✅
- 2、Select：❌
- 3、InputNumber：❌
- 4、Textarea：❌

### 2、修改defaultProps

大概的思路就是，修改ant-design的源码中的**input**这一部分，给`Input、Textarea`这些组件加上一个defaultProps，这个defaultProps长这样：

![1718450494439](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450494439.png)

所以具体实现为以下代码

![1718450506725](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450506725.png)

结果就是，全局的**Input、Textarea**都去除了拼接检查了，但是**Select、InputNumber**却没有去除，因为他们是依赖了**RCSelect、RCInputNumber**这两个另外的组件，所以想改这两个，就得去改他们两依赖的组件，所以结果就是：

- 1、Input：✅
- 2、Select：❌
- 3、InputNumber：❌
- 4、Textarea：✅

### 3、拦截React.createElement

我们都知道在React中，只要涉及到**JSX**语法，最终在解析时都会通过**React.createElement**方法来创建标签

![1718450520068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450520068.png)

所以思路就是在最终调用**React.createElement**时，判断如果是`input、textarea`这两个类型的话，就给标签加上`spellCheck: false`这个属性，具体代码为

![1718450537171](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450537171.png)

这样做的结果是：

- 1、Input：✅
- 2、Select：✅
- 3、InputNumber：✅
- 4、Textarea：✅

但是总是觉得直接涉及到React自带方法的修改，有点不太好。。

### 4、全局监听input事件

思路就是全局监听`input`这个事件，并在这个事件里去给触发事件的DOM节点增加**spellCheck: false**，具体代码为：

![1718450551426](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450551426.png)

这样做的结果是：

- 1、Input：✅
- 2、Select：✅
- 3、InputNumber：✅
- 4、Textarea：✅

### 5、MutationObserver

兼容性比较差，所以不考虑了吧~~~

![1718450563545](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450563545.png)

### 6、body 直接加 spellcheck="true"

哎。。。都怪没有好好看文档。。。其实前面做的都是无用功，最方便的做法是直接在 `body` 上加 `spellcheck="false"` 就行了，底下的后代元素会自动继承这个属性值。。

![1718450577891](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450577891.png)

![1718450586944](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1718450586944.png)

