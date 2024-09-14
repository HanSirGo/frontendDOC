# Fabric.js 库

Fabric.js是一个用于创建交互式的Canvas元素的JavaScript库。它提供了一种简单而强大的方式来处理Canvas元素上的图形对象，使得在Canvas上绘制、编辑和操作图形变得更加容易。

![1726298982628](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1726298982628.png)

基本使用Fabric.js的步骤如下：

1、 **引入Fabric.js库**：首先，在HTML文件中引入Fabric.js库文件，可以通过CDN或本地文件引入。

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/fabric.js/4.5.0/fabric.min.js"></script>
```

2、 **创建Canvas元素**：在HTML文件中创建一个Canvas元素，用于显示Fabric.js中的图形对象。

```
<canvas id="canvas" width="800" height="600"></canvas>
```

3、 **初始化Fabric.js**：在JavaScript文件中初始化Fabric.js，创建Canvas实例。

```
// 获取Canvas元素
var canvas = new fabric.Canvas('canvas');
```

4、 **创建图形对象**：使用Fabric.js提供的方法创建各种图形对象，如矩形、圆形、文本等。

```
// 创建一个矩形对象var rect = new fabric.Rect({  left: 100,  top: 100,  width: 200,  height: 150,  fill: 'red'});
// 将矩形对象添加到Canvas中canvas.add(rect);
```

5、 **操作图形对象**：可以对添加到Canvas中的图形对象进行编辑、移动、缩放等操作。

```
// 移动矩形对象到新的位置
rect.set({ left: 200, top: 200 });
canvas.renderAll(); // 重新渲染Canvas
```

6、 **事件处理**：Fabric.js还提供了丰富的事件处理功能，可以监听Canvas和图形对象的各种事件。

```
// 监听鼠标点击事件
canvas.on('mouse:down', function(options) { 
	console.log('鼠标点击位置：', options.e.clientX, options.e.clientY);
});
```

通过以上步骤，你可以开始使用Fabric.js创建交互式的Canvas应用程序，绘制和操作各种图形对象。Fabric.js提供了丰富的功能和API，可以满足各种Canvas绘图需求。

Fabric.js提供了许多内置的图形对象类型，可以用来创建各种图形和元素。以下是Fabric.js中常用的一些图形对象类型：

> 1.**Rect（矩形）**：矩形对象，可以设置宽度、高度、填充颜色等属性。
>
> 2.**Circle（圆形）**：圆形对象，可以设置半径、填充颜色等属性。
>
> 3.**Triangle（三角形）**：三角形对象，可以设置三个顶点的坐标、填充颜色等属性。
>
> 4.**Ellipse（椭圆）**：椭圆对象，可以设置水平和垂直半径、填充颜色等属性。
>
> 5.**Line（直线）**：直线对象，可以设置起点、终点、线条颜色等属性。
>
> 6.**Polyline（折线）**：折线对象，可以设置多个点的坐标、线条颜色等属性。
>
> 7.**Polygon（多边形）**：多边形对象，可以设置多个顶点的坐标、填充颜色等属性。
>
> 8.**Text（文本）**：文本对象，可以设置文本内容、字体、大小、颜色等属性。
>
> 9.**Image（图片）**：图片对象，可以加载并显示图片，设置位置、大小等属性。

这些是Fabric.js中常用的一些图形对象类型，你可以使用它们来创建各种图形元素，并通过Fabric.js提供的方法和属性来操作和编辑这些图形对象。Fabric.js还支持自定义图形对象类型，使得用户可以根据需要扩展和定制图形对象。