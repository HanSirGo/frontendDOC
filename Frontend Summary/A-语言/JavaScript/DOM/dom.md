## DOM-文档对象模型

```
DOM 指的是文档对象模型，它指的是把文档当做一个对象来对待，这个对象主要定义了处理网页内容的方法和接口。
```

> 《DOM, DOCUMENT, BOM, WINDOW 有什么区别?》
>
> 《Window 对象》
>
> 《DOM 与 BOM 分别是什么，有何关联？》
>
> 《JavaScript 学习总结（三）BOM 和 DOM 详解》

### dom渲染流程

```js
1、浏览器解析html源码，然后创建一个DOM树。

在DOM树中，每一个HTML标签都有一个对应的节点(元素节点),并且每一个文本也都有一个对应的节点(文本节点)。DOM树的根节点就是documentElement，对应的是html标签。 

2、浏览器解析CSS代码，计算出最终的样式数据。

对CSS代码中非法的语法它会直接忽略掉。解析CSS的时候会按照如下顺序来定义优先级：浏览器默认设置，用户设置，外联样式，内联样式，html中的style(嵌在标签中的行间样式)。

3、创建完DOM树并得到最终的样式数据之后，构建一个渲染树。

渲染树和DOM树有点像，但是有区别。DOM树完全和html标签一一对应，而渲染树会忽略不需要渲染的元素(head、display:none的元素)。渲染树中每一个节点都存储着对应的CSS属性。

4、当渲染树创建完成之后，浏览器就可以根据渲染树直接把页面绘制到屏幕上
```

### 获取DOM元素有哪些方法？

| 方法                                   | 描述                      |
| :------------------------------------- | :------------------------ |
| document.getElementById(id)            | 通过id获取dom             |
| document.getElementsByTagName(tagName) | 通过标签名获取dom         |
| document.getElementsByClassName(class) | 通过class获取dom          |
| document.getElementsByName(name)       | 通过标签的属性name获取dom |
| document.querySelector(选择器)         | 通过选择器获取dom         |
| document.querySelectorAll(选择器)      | 通过选择器获取dom         |

### 操作DOM元素有哪些方法

| 方法                   | 描述                                                         |
| :--------------------- | :----------------------------------------------------------- |
| createElement          | 创建一个标签节点                                             |
| createTextNode         | 创建一个文本节点                                             |
| cloneNode(deep)        | 复制一个节点，连同属性与值都复制，deep为true时，连同后代节点一起复制，不传或者传false，则只复制当前节点 |
| createDocumentFragment | 创建一个文档碎片节点                                         |
| appendChild            | 追加子元素                                                   |
| insertBefore           | 将元素插入前面                                               |
| removeChild            | 删除子元素                                                   |
| replaceChild           | 替换子元素                                                   |
| getAttribute           | 获取节点的属性                                               |
| createAttribute        | 创建属性                                                     |
| setAttribute           | 设置节点属性                                                 |
| romoveAttribute        | 删除节点属性                                                 |
| element.attributes     | 将属性生成类数组对象                                         |

### DOM的类型有哪几种？

```js
元素节点              Node.ELEMENT_NODE(1)
属性节点              Node.ATTRIBUTE_NODE(2)
文本节点              Node.TEXT_NODE(3)
CDATA节点             Node.CDATA_SECTION_NODE(4)
实体引用名称节点       Node.ENTRY_REFERENCE_NODE(5)
实体名称节点          Node.ENTITY_NODE(6)
处理指令节点          Node.PROCESSING_INSTRUCTION_NODE(7)
注释节点              Node.COMMENT_NODE(8)
文档节点              Node.DOCUMENT_NODE(9)
文档类型节点          Node.DOCUMENT_TYPE_NODE(10)
文档片段节点          Node.DOCUMENT_FRAGMENT_NODE(11)
DTD声明节点            Node.NOTATION_NODE(12)
```

### 全屏

```js
// 全屏事件

handleFullScreen() {
    let element = document.documentElement
    // 判断是否已经是全屏
    // 如果是全屏,退出
    if(this.fullscreen){
        if(document.exitFullscreen){
            document.exitFullscreen()
        } else if(document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen()
        } else if(document.mozCancelFullScreen) {
            document.mozCancelFullScreen()
        } else if(document.msExitFullscreen){
            document.msExitFullscreen()
        }
        console.log('已还原')
    } else { // 进入全屏
        if(element.requestFullscreen){
            element.requestFullscreen()
        }else if(element.webkitRequestFullScreen){
            element.webkitRequestFullScreen()
        }else if(element.mozRequestFullScreen){
            element.mozRequestFullScreen()
        }else if(element.msRequestFullscreen){
            // IE11
            element.msRequestFullscreen()
        }
    }
}
```

# Dom中的各种距离

#### 第一个发现 window.devicePixelRatio 的存在

本打算用截图软件丈量尺寸，结果发现截图软件显示的屏幕宽度与浏览器开发者工具获取的宽度不一致，这是为什么呢?

- 截图软件显示的屏幕宽度是1920

![1713599650021](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599650021.png)

- window.screen.width显示的屏幕宽度是1536

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599668450.png)

这是怎么回事？原来在PC端，也存在一个设备像素比的概念。它告诉浏览器一个css像素应该使用多少个物理像素来绘制。要说设备像素比，得先说一下像素和分辨率这两个概念。

- **像素** 屏幕中最小的色块，每个色块称之为一个像素（Pixel）

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599686434.png)

- **分辨率** 分辨率=屏幕水平方向的像素点数 * 屏幕垂直方向的像素点数; 另外说一下,关于分辨率有多种定义,可以细分为**显示分辨率****[1]**、**图像分辨率****[2]**、**打印分辨率****[3]**和**扫描分辨率****[4]**等，此处是指显示分辨率。

![1713599701430](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599701430.png)

- **设备像素比**

设备像素比的定义是：

`window.devicePixelRatio` =显示设备物理像素分辨率显示设备CSS像素分辨率\frac{显示设备物理像素分辨率}{显示设备CSS像素分辨率}显示设备CSS像素分辨率显示设备物理像素分辨率

根据设备像素比的定义, 如果知道显示设备横向的css像素值，根据上面的公式，就能计算出显示设备横向的物理像素值。

```js
# 显示设备宽度物理像素值= window.screen.width * window.devicePixelRatio;
```

设备像素比在我的笔记本电脑上显示的数值是1.25, 代表一个css逻辑像素对应着1.25个物理像素。

![1713599734389](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599734389.png)

我前面的公式计算了一下，与截图软件显示的像素数值一致。这也反过来说明，截图软件显示的是物理像素值。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599749115.png)

- window.devicePixelRatio 是由什么决定的 ?

发现是由笔记本电脑屏幕的缩放设置决定的，如果设置成100%, 此时window.screen.width与笔记本电脑的显示器分辨率X轴方向的数值一致,都是1920(如右侧图所示), 此时屏幕上的字会变得比较小,比较伤视力。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599764402.png)

![1713599785323](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599785323.png)

- 逻辑像素是为了解决什么问题?

逻辑像素是为了解决屏幕相同，分辨率不同的两台显示设备， 显示同一张图片大小明显不一致的问题。比如说两台笔记本都是15英寸的，一个分辨率是`1920*1080`,一个分辨率是`960*540`, 在`1920*1080`分辨率的设备上，每个格子比较小，在`960*540`分辨率的设备上，每个格子比较大。一张`200*200`的图片，在高分率的设备上看起来会比较小，在低分辨率的设备上，看起来会比较大。观感不好。为了使同样尺寸的图片，在两台屏幕尺寸一样大的设备上，显示尺寸看起来差不多一样大，发明了逻辑像素这个概念。规定所有电子设备呈现的图片等资源尺寸统一用逻辑像素表示。然后在高分辨率设备上，提高devicePixelRatio, 比如说设置`1920*1080`设备的devicePixelRatio(dpr)等于2, 一个逻辑像素占用两个格子,在低分辨率设备上，比如说在`960*540`设备上设置dpr=1, 一个css逻辑像素占一个格子, 这样两张图片在同样的设备上尺寸大小就差不多了。通常设备上的逻辑像素是等于物理像素的，在高分辨率设备上，物理像素是大于逻辑像素数量的。由此也可以看出，物理像素一出厂就是固定的，而设备的逻辑像素会随着设备像素比设置的值不同而改变。但图片的逻辑像素值是不变的。

#### document.body、document.documentElement和window.screen的宽高区别

差别是很容易辨别的，如下图所示：

- document.body -- body标签的宽高
- document.documentElement -- 网页可视区域的宽高(**不包括滚动条**)
- window.screen -- 屏幕的宽高

![1713599806783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599806783.png)

- 网页可视区域不包括滚动条

如下图所示，截图时在未把网页可视区域的滚动条高度计算在内的条件下, 截图工具显示的网页可视区域高度是168, 浏览器显示的网页可视区域的高度是167.5, 误差0.5，由于截图工具是手动截图，肯定有误差，结果表明，网页可视区域的高度 **不包括滚动条高度**。宽度同理。

![1713599839931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599839931.png)

- 屏幕和网页可视区域的宽高区别如下：

屏幕宽高是个固定值，网页可视区域宽高会受到缩放窗口影响。

![1713599853670](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599853670.png)

- 屏幕高度和屏幕可用高度区别如下：

屏幕可用高度=屏幕高度-屏幕下方任务栏的高度，也就是：

```js
# window.screen.availHeight = window.screen.height - 系统任务栏高度
```

![1713599889427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599889427.png)

#### scrollWidth, scrollLeft, clientWidth关系

```js
# scrollWidth(滚动宽度,包含滚动条的宽度)=scrollLeft(左边卷去的距离)+clientWidth(可见部分宽度);
// 同理
# scrollHeight(滚动高度,包含滚动条的高度)=scrollTop(上边卷去的距离)+clientHeight(可见部分高度);
```

需要注意的是，上面这三个属性，都取的是溢出元素的**父级元素属性**。而不是溢出元素本身。本例中溢出元素是body(document.body),其父级元素是html(document.documentElement)。另外，

```js
# 溢出元素的宽度(document.body.scrollWidth）=父级元素的宽度(document.documentElement.scrollWidth) - 滚动条的宽度(在谷歌浏览器上滚动条的宽度是19px)
```

![1713599931873](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599931873.png)image.png

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
    <title>JS Dom各种距离</title>
    <style>
        html,
        body {
            margin: 0;
        }

        body {
            width: 110%;
            border: 10px solid blue;
        }

        .rect {
            height: 50px;
            background-color: green;
        }
    </style>
</head>

<body>
    <div id="rect" class="rect"></div>
</body>

</html>
```

#### 元素自身和父级元素的scrollWidth和scrollLeft关系?

从下图可以看出:

- 元素自身没有X轴偏移量，元素自身的滚动宽度不包含滚动条
- 父级元素有X轴便宜量, 父级元素滚动宽度包含滚动条![1713599955088](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599955088.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
    <title>JS Dom各种距离</title>
    <style>
        div {
            border: 1px solid #000;
            width: 200px;
            height: 600px;
            padding: 10px;
            background-color: green;
            margin: 10px;
        }
    </style>
</head>

<body>
    <div class="rect">    111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111
    </div>
</body>
<script>
</script>
</html>
```

#### offsetWidth和clientWidth的关系?

offsetWidth和clientWidth的共同点是都包括 自身宽度+padding , 不同点是**offsetWidth包含border**。

如下图所示:

- rect元素的clientWidth=200px(自身宽度) + 20px(左右padding) = 220px
- rect元素的offsetWidth=200px(自身宽度) + 20px(左右padding) + 2px(左右boder) = 222px

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713599980915.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
    <title>JS Dom各种距离</title>
    <style>
        div {
            border: 1px solid #000;
            width: 200px;
            height: 100px;
            padding: 10px;
            background-color: green;
            margin: 10px;
        }
    </style>
</head>

<body>
    <div class="rect">111111111111111111111111111111111111111111111111</div>
</body>
<script>


</script>

</html>
```

#### event.clientX，event.clientY, event.offsetX 和 event.offsetY 关系

代码如下，给rect元素添加一个mousedown事件，打印出事件源的各种位置值。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1.0"> -->
    <title>JS Dom各种距离</title>
    <style>
        html,
        body {
            margin: 0;
        }

        body {
            width: 200px;
            padding: 10px;
            border: 10px solid blue;
        }

        .rect {
            height: 50px;
            background-color: green;
        }
    </style>
</head>

<body>

    <div id="rect" class="rect"></div>


</body>
<script>
    const rectDom = document.querySelector('#rect');

    rectDom.addEventListener('mousedown', ({ offsetX, offsetY, clientX, clientY, pageX, pageY, screenX, screenY }) => {
        console.log({ offsetX, offsetY, clientX, clientY, pageX, pageY, screenX, screenY });
    })
</script>

</html>
```

我们通过y轴方向的高度值,了解一下这几个属性的含义。绿色块的高度是50px, 我们找个特殊的位置（绿色块的右小角）点击一下，如下图所示:

- offsetY=49, 反推出这个值是相对于元素自身的顶部的距离
- clientY=69, body标签的border-top是10，paiding是10, 反推出这个值是相对网页可视区域顶部的距离
- screenY=140，目测肯定是基于浏览器窗口，

所以它们各自的含义,就很清楚了。

![1713600015689](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600015689.png)

| 事件源属性                       | 表示的距离                                                   |
| :------------------------------- | :----------------------------------------------------------- |
| **event.offsetX、event.offsetY** | 鼠标相对于事件源元素（srcElement）的X,Y坐标，                |
| **event.clientX、event.clientY** | 鼠标相对于浏览器窗口可视区域的X，Y坐标（窗口坐标），可视区域不包括工具栏和滚动偏移量。 |
| **event.pageX、event.pageY**     | 鼠标相对于文档坐标的x,y坐标，文档坐标系坐标 ＝ 视口坐标系坐标 ＋ 滚动的偏移量 |
| **event.screenX、event.screenY** | 鼠标相对于用户显示器屏幕左上角的X,Y坐标                      |

- pageX和clientX的关系

我们点击下图绿色块的右下角，把pageX和clientX值打印出来。如下图所示:

- 可视区域的宽度是360，点击点的clientX=359(由于是手动点击,有误差也正常)
- 水平方向的偏移量是56
- pageX是415，360+56=416,考虑到点击误差，可以推算出 `ele.pageX = ele.clientX + ele.scrollLeft`

![1713600033612](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600033612.png)

#### getBoundingClientRect获取的top,bottom,left,right的含义

从下图可以看出，上下左右这四个属性，都是相对于浏览器可视区域左上角而言的。

![1713600057377](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600057377.png)

从下图可以看出，当有滚动条出现的时候，right的值是359.6，而不是360+156(x轴的偏移量), 说明通过getBoundingClientRect获取的属性值是**不计算滚动偏移量的**，是相对浏览器可视区域而言的。

![1713600068028](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600068028.png)

#### movementX和movementY的含义?

MouseEvent.movementX/movementX是一个相对偏移量。返回当前位置与上一个mousemove事件之间的水平/垂直距离。以当前位置为基准, 鼠标向左移动, movementX就是负值，向右移动，movementX就是正值。鼠标向上移动,movementY就是负值，向下移动，movementY就是正值。数值上，它们等于下面的计算公式。这两个值在设置拖拽距离的时候高频使用，用起来很方便。

```js
curEvent.movementX = curEvent.screenX - prevEvent.screenX;
curEvent.movementY = curEvent.screenY - prevEvent.screenY;
```

### 想移动元素，mouse和drag事件怎么选?

mouse事件相对简单，只有mousedown(开始),mousemove(移动中),mouseup(结束)三种。与之对应的移动端事件是touch事件，也是三种touchstart(手指触摸屏幕), touchmove(手指在屏幕上移动), touchend(手指离开屏幕)。

相对而言, drag事件就要丰富一些。

- 被拖拽元素事件

| 事件名    | 触发时机           | 触发次数 |
| :-------- | :----------------- | :------- |
| dragstart | 拖拽开始时触发一次 | 1        |
| drag      | 拖拽开始后反复触发 | 多次     |
| dragend   | 拖拽结束后触发一次 | 1        |

- 目标容器事件

| 事件名    | 触发时机                                               | 触发次数 |
| :-------- | :----------------------------------------------------- | :------- |
| dragenter | 被拖拽元素进入目标时触发一次                           | 1        |
| dragover  | 被拖拽元素在目标容器范围内时反复触发                   | 多次     |
| drop      | 被拖拽元素在目标容器内释放时(前提是设置了dropover事件) | 1        |

想要移动一个元素，该如何选择这两种事件类型呢？选择依据是:

| 类型      | 选择依据                                                     |
| :-------- | :----------------------------------------------------------- |
| mouse事件 | 1. 要求丝滑的拖拽体验 2. 无固定的拖拽区域 3. 无需传数据      |
| drag事件  | 1. 拖拽区域有范围限制 2. 对拖拽流畅性要求不高 3. 拖拽时需要传数据 |

### 现在让我们写个拖拽效果

光说不练假把式, 扫清了学习障碍后，让我们自信满满地写一个兼容PC端和移动端的拖动效果。不积跬步无以至千里，幻想一口吃个胖子，是不现实的。这一点在股市上体现的淋漓尽致。都是有耐心的人赚急躁的人的钱。所以，要我们沉下心来，打牢基础，硬骨头啃一点就会少一点，步步为营，稳扎稳打，硬骨头也会被啃成渣。

```html
<!DOCTYPE html>
<html lang="en">

<head>
        
    <meta charset="UTF-8" />    
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />   
    <title>移动小鸟</title>
    <style>
        body {
            margin: 0;
            font-size: 0;
            position: relative;
            height: 100vh;
        }

        .bird {
            position: absolute;
            width: 100px;
            height: 100px;
            cursor: grab;
            z-index: 10;
        }
    </style>
</head>

<body>
    <img class="bird" src="./bird.png" alt="" />  
</body>

<script>
    let evtName = getEventName();
    // 鼠标指针相对于浏览器可视区域的偏移
    let offsetX = 0, offsetY = 0;
    // 限制图片可以X和Y轴可以移动的最大范围，防止溢出
    let limitX = 0, limitY = 0;

    // 确保图片加载完
    window.onload = () => {
        const bird = document.querySelector(".bird");
        const { width, height } = bird;

        limitX = document.documentElement.clientWidth - width;
        limitY = document.documentElement.clientHeight - height;

        bird.addEventListener(evtName.start, (event) => {
            // 监听鼠标指针相对于可视窗口移动的距离
            // 注意移动事件要绑定在document元素上，防止移动过快,位置丢失
            document.addEventListener(evtName.move, moveAt);
        });

        // 鼠标指针停止移动时,释放document上绑定的移动事件
        // 不然白白产生性能开销
        document.addEventListener(evtName.end, () => {
            document.removeEventListener(evtName.move, moveAt);
        })

        // 移动元素
        function moveAt({ movementX, movementY }) {
            const { offsetX, offsetY } = getSafeOffset({ movementX, movementY });

            window.requestAnimationFrame(() => {
                bird.style.cssText = `left:${offsetX}px;top:${offsetY}px;`;
            });
        };
    };

    // 获取安全的偏移距离
    const getSafeOffset = ({ movementX, movementY }) => {
        // //距上次鼠标位置的X,Y方向的偏移量
        offsetX += movementX;
        offsetY += movementY;

        // 防止拖拽元素被甩出可视区域
        if (offsetX > limitX) {
            offsetX = limitX;
        }

        if (offsetX < 0) {
            offsetX = 0;
        }

        if (offsetY > limitY) {
            offsetY = limitY;
        }

        if (offsetY < 0) {
            offsetY = 0;
        }

        // console.log({ movementX, movementY, offsetX, offsetY });
        return { offsetX, offsetY };
    }

    // 区分是移动端还是PC端移动事件
    function getEventName() {
        if ("ontouchstart" in window) {
            return {
                start: "touchstart",
                move: "touchmove",
                end: "touchend",
            };
        } else {
            return {
                start: "pointerdown",
                move: "pointermove",
                end: "pointerup",
            };
        }
    }
</script>

</html>
```

### 彩蛋

在chrome浏览器上发现一个奇怪的现象,设置的border值是整数，计算出来的值却带有小数

![1713600117164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600117164.png)

而当border值是4的整数倍的时候,计算值是正确的

![1713600132827](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713600132827.png)

看了**这篇文章****[5]**解释说，浏览器可能只能渲染具有整数物理像素的border值，不是整数物理像素的值时，计算出的是近似border值。这个解释似乎讲得通，在设备像素比是window.devicePixelRatio=1.25的情况下, 1px对应的是1.25物理像素, `1.25*4的倍数`才是整数，所以设置的逻辑像素是4的整数倍数，显示的渲染计算值与设置值一致,唯一让人不理解的地方，为什么padding,margin，width/height却不遵循同样的规则。

