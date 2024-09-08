# CSS 函数 calc() 会引起重排重绘吗？

## **calc() 用法介绍**

CSS 中的 `calc()` 函数可以执行元素属性的计算。

首先简单介绍一下 CSS 中的 `calc()` 函数的用法。

**语法：**

```
property: calc(expression);
```

这里的 `expression` 可以包含加号 (`+`)、减号 (`-`)、乘号 (`*`)、除号 (`/`)，以及可以进行数学运算的数字和长度单位。

**使用示例：**

1. **组合长度单位**：

```
.element {  width: calc(100vh - 20px);}
```

1. **嵌套使用**：

```
.element {  margin: calc(60px + calc(10% * 2));}
```

## **初步分析 calc() 是否引起重排重绘**

首先介绍一下重排重绘：

**重排（Reflow）**

重排是指浏览器重新计算页面元素的尺寸和布局的过程。当页面的**布局属性发生变化**时，浏览器需要**重新计算**并**重新排列**元素，这个过程称为重排。重排可能会引起页面的重新渲染，因为它影响元素的几何属性。

**引起重排的操作** **：**

- **改变**元素的宽度、高度、边距、填充、字体大小等。
- **改变**元素的显示属性（如从 `display: block` 改为 `display: inline`）。
- 内容**变化**，如文本更改或图片加载完成。
- 浏览器窗口大小**改变**（响应式设计中的媒体查询）。

**重绘（Repaint）**

重绘是指浏览器重新绘制页面元素的外观过程，而不影响其布局。当元素的外观属性**改变**，但不会影响其尺寸或位置时，会发生重绘。

引起重绘的操作：

- **改变**元素的颜色、背景色、边框颜色等。
- **改变**元素的可见性（如 `visibility` 属性）。
- **改变**元素的文本装饰（如 `text-decoration`）。

------

以上描述中注意引发重排重绘的关键动词---**改变**。从这个角度来分析，猜想 **calc() 本身并不会引起重排**，而是**布局属性发生变化**会引起重排。

calc() 什么情况会导致重排呢？

- 当表达式中有可变的量（当前状态可能是静态的值），当变量发生改变时引起计算结果发生改变，并且这些改变导致元素尺寸或布局的变化；
- JS 动态改变使用 calc() 计算的属性值，并且这些改变导致元素尺寸或布局的变化。

举一个例子，这个例子会在**可视区宽度发生变化**后（比如手动调整浏览器的视口高度），会引起**元素高度发生改变**，从而引起**重排**。

```
/* property: calc(expression); */
.element {
  width: calc(100vh - 20px);
}
```

## **用 performance 验证 calc() 是否引起重排重绘**

下面用一组**对照示例**验证 calc() 是否引起重排重绘

- 示例1：header 和 footer 高度固定，中间内容区域滚动
- 示例2：在示例1的基础上，通过 setTimeout 让元素高度发生改变

### **示例 1：header 和 footer 高度固定，中间内容区域滚动**

一个简单的例子，header 和 footer 高度固定，中间内容区域滚动

```
<!DOCTYPE html>
<html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
      * {
        margin: 0px;
      }

      .container {
        width: 100vw;
        height: 100vh;
      }

      header,
      footer {
        height: 60px;
        background-color: antiquewhite;
      }

      .content-wrapper {
        height: calc(100vh - 120px);
        overflow: auto;
      }

      .content {
        height: 900px;
        background-color: skyblue;
      }
    </style>
  </head>

  <body>
    <div class="container">
      <header>header</header>
      <div class="content-wrapper">
        <ul id="content" class="content">
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
          <li>test</li>
        </ul>
      </div>
      <footer>footer</footer>
    </div>
  </body>

</html>
```



![1725778011950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725778011950.png)

**重要说明：** **在 Performance 记录中，重排可能不会直接显示为一个单独的事件，而是作为影响性能帧率（FPS）的一个因素。由于重排一定会导致重绘，并且这个示例中我们知道改变了元素的尺寸，所以一定会有重排。所以这里借用观察 Paint 来判断是否重绘，推断出重排。**

用 performance 分析一下

![1725778030783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725778030783.png)

放大看一下

![1725778070292](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725778070292.png)

从火焰图中可以看到，只有一次 Paint，所以没有发生重排。

### **示例 2：通过 setTimeout 让元素高度发生改变**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0px;
        }

        .container {
            width: 100vw;
            height: 100vh;
        }

        header,
        footer {
            height: 60px;
            background-color: antiquewhite;
        }

        .content-wrapper {
            height: calc(100vh - 120px);
            overflow: auto;
        }

        .content {
            height: 900px;
            background-color: skyblue;
        }
    </style>
</head>

<body>
    <div class="container">
        <header>header</header>
        <div class="content-wrapper">
            <ul id="content" class="content">
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
                <li>test</li>
            </ul>
        </div>
        <footer>footer</footer>

        <script>
            setTimeout(() => {
                document.getElementById('content').style.height = '500px';
            }, 10)
        </script>
    </div>
</body>

</html>
```

![1725778105885](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725778105885.png)

分别放大看一下

![1725778127791](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725778127791.png)

可以看到，通过改变元素高度，发生了第二次 Paint，这个例子中发生了重排。

### **对照示例分析**

通过一组对照示例的火焰图，可以分析得到：

- 示例1：header 和 footer 高度固定，中间内容区域滚动

- - 只是通过 calc() 计算元素的属性值，并不会引起重排

- 示例2：在示例1的基础上，通过 setTimeout 让元素高度发生改变

- - 改变元素高度，引发了重排

## **结论**

**calc() 本身并不会引起重排**，当元素**布局属性值发生变化**会引起重排。