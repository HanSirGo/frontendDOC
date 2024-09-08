# CSS 垂直居中

> 了解这个新属性之前，让我们来回顾下 css 垂直居中有哪些方法

#### **历史选择**

1. 表格单元格

```
<div style="display: table;">
  <div style="display: table-cell; vertical-align: middle;">我的内容</div>
</div>
```

1. 绝对定位

```
<div style="position: relative;">
  <div style="position: absolute; top: 50%; transform: translateY(-50%);">Content.</div>
</div>
```

1. inline-block

```
<style>
  .container::before {
    content: "";
    height: 100%;
    display: inline-block;
    vertical-align: middle;
  }
  .content {
    display: inline-block;
    vertical-align: middle;
  }
</style>

<div class="container">
  ::before
  <div class="content">Content.</div>
</div>
```

1. 单行 flex

```
<div style="display: flex; align-items: center;">
  <div>Content.</div>
</div>
<!-- 或者 -->
<div style="display: flex; flex-flow: column; justify-content: center;">
  <div>Content.</div>
</div>
```

1. 多行 flex

```
<div style="display: flex; flex-flow: row wrap; align-content: center;">
  <div>Content.</div>
</div>
```

1. 网格 content

```
<div style="display: grid; align-content: center;">
  <div>Content.</div>
</div>
```

1. 网格 cell

```
<div style="display: grid; align-items: center;">
  <div>Content.</div>
</div>
```

1. 自动边距

```
<div style="display: grid;">
  <div style="margin-block: auto;">Content.</div>
</div>
```

以上是我们常用和不常用的 css 垂直居中方法

#### **现在可以直接使用 align-content**

是不是很熟悉，这不是 flex 的容器的属性吗?然和现在已经是浏览器默认布局了，不需要再使用 flex 或 grid 包装

```
<div style="align-content: center;"><code>align-content</code> is it work!</div>
```

目前在以下浏览器版本生效 Chrome：123 | Firefox：125 | Safari：17.4

在 2024 年的Chrome 123 版本中， CSS 原生可以使用 **1 个 CSS 属性** `align-content: center`进行垂直居中。

**有何魅力？**

这个特性的魅力在哪儿呢？我举例给你看一下

```
<div style="align-content:center; height:200px; background: #614ef5;">
  <sapn>align-content</sapn> ：我能垂直居中！
</div>
```

![1725779247531](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779247531.png)

相信你也发现了，  `align-content: center`属性实现垂直居中， 再也不需要依赖于`Flex`布局或者`Grid`布局了，它针对`普通块级`元素就生效，这就是它的魅力所在🔥！

**我能用吗？**

接下来，我们再看一下`align-content`这个属性的浏览器支持情况：

![1725779304089](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779304089.png)

这是 `can i use` 网站的截图，可以看到主流浏览器， 现在绝大部分版本都是支持的，可以比较放心的使用😺

**我要注意哪些坑？**

看看直接使用 `align-content`有哪些注意点。

**行内元素不生效**

前面我提到了, `align-content`针对普通块级元素生效， 什么意思呢😵？我们看，下面这个代码就不生效

```
<div style="display:inline;align-content:center; height:200px; background: #614ef5;">
  <span>align-content</sapn> ：我能垂直居中！
</div>
```

![1725779356724](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779356724.png)

如果改成`display:inline-block`是有效果的：

![1725779377176](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779377176.png)

其实就是`inline`行内元素不生效！

**flex布局要小心**

接着改为flex布局`display:flex`， 发现`align-content:center`没有生效。这是为什么呢？你前面不是说原来`align-content`数据依赖Flex布局才起作用吗，为什么我改成Flex布局反而不生效了呢

带着这个疑问往下看↓

![1725779435207](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779435207.png)

找到不生效的css样式，浏览器已经给了提示， 意思就是`align-content:center`这个属性在`flex-wrap:no-wrap`中不生效，那我们再增加几个元素来试一下：

```
<style>
.box{
  display:flex;
  align-content:center; 
  height:200px; 
  background: #dcd7ff;
}
.box span{
  width:46px;
  height:46px;
  background-color:#9181ff;
  border-radius:4px;
  margin-left:10px;
}
.box2{
  margin-top:20px;
  flex-wrap:wrap;
}
</style>
<div class="box">
  <span>我</span>
  <span>能</span>
  <span>垂</span>
  <span>直</span>
  <span>居</span>
  <span>中</span>
</div>

<div class="box box2">
  <span>我</span>
  <span>能</span>
  <span>垂</span>
  <span>直</span>
  <span>居</span>
  <span>中</span>
</div>
```

添加`flex-wrap:wrap;`前后对比：

![1725779472994](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725779472994.png)

**留个疑问**

最后给大家留一个思考题🧐

flex中垂直居中，你是否想到了`align-items:center`，大部分情况我们都是用的这个属性来实现垂直居中，和`align-content:center`有什么区别呢？