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