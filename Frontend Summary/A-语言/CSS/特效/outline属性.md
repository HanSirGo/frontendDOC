# CSS3的outline属性，制作不一样的交互效果

CSS 的 outline 属性是在一条声明中设置多个轮廓属性的简写属性 ，例如 outline-style, outline-width 和 outline-color。另外一个配套属性outline-offset  专门用于配置边框到元素中心的偏移距离的，属性值为负数则边框在元素内部，甚至是一个最小圆，属性值为正数则边框在元素外部，单位是像素。在下文案例中可以看到具体用法。

**border和outline区别**

1. outline 不占据空间，绘制于元素内容周围，不属于盒子的一部分，因此不会影响元素或相邻元素的位置；
2. 根据规范，outline 通常是矩形，但也可以是非矩形的。

更多关于outline属性基础知识：https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline

在有了对outline的基本认识后，就可以进行应用和实战了，根据自己的需求来设置不同的交互效果。以下案例据来自小编在项目中用过的交互效果，仅供参考，希望能给好奇css的小伙伴带来一些启发和灵感。

![1713692454130](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692454130.png)

**基础样式**

```css

body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    grid-auto-flow: column;
    gap: 50px;
    place-content: center;
    place-items: center;
    background: #001C30;
}
.outline {
    width: 200px;
    aspect-ratio: 1 / 1;
    background: #4ECDC4;
    border-radius: 50%;
    transition: all 0.5s ease;
}
```

```html
<div class="outline"></div>
```

效果图

![1713692499058](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692499058.png)

**虚线外边框**

下列样式追加到 .outline 类名下，设置outline-offset=20px时，表示outline边框将会距元素外边框 20px，样式如下：

```css
outline: 10px dashed tomato;
outline-offset: 20px;
```

刷新页面后效果是这样的：

![1713692525094](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692525094.png)

添加鼠标过渡动画效果，当鼠标移入时，边框偏移距离outline-offset=0，此时边框应该紧贴这元素周围。

```css

.outline:hover {
    cursor: pointer;
    outline-offset: 0;
}
```

实际效果图：

![1713692554373](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692554373.png)

如果想让边框偏移到元素内部，意味outline-offset值为负数，假如是 -100px，你可以这样设置样式：

```css
.outline:hover {
    cursor: pointer;
    outline-offset: -100px;
}
```

此时的效果是这样的，哈哈，css果然魅力无穷，太深奥了！！！别急下一个案例更有意思。

![1713692578161](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692578161.png)

**图片蒙层效果**

首先得有一张图片，保留body基本样式。重写写一个class类名来定位img标签。当鼠标移入时，图标出现蒙层效果，边框偏移到图片元素距离是50px，为了让边框多余部分被裁剪掉，可以使用clip-path属性来进行裁剪为圆形，同时有一种缩放过渡状态，不让效果显得那么笨拙。这个效果样式应该是这样的。

```css
.outline-img {
    width:  300px;
    aspect-ratio: 1 / 1;
    object-fit: cover;
    transition: all 1s ease;
    outline: 300px solid rgba(0, 0, 0, 0.3);
    outline-offset: -150px;
    border-radius: 50%;
    clip-path: circle(calc(50% - 40px));
}
.outline-img:hover {
    clip-path: circle(50%);
    cursor: pointer;
    outline: 20px solid tomato;
    outline-offset: 50px;
}

 <img 
   class="outline-img" 
   src="https://picsum.photos/id/64/200/200">
```

刷新页面，此时的交互效果是这样的：

![1713692611307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692611307.png)

如果不想看到图片随着边框放大而放大的效果，可以配置图片的clip-path属性如下，同时清理掉hover模块配置的clip-path属性，保存后刷新浏览器页面，就会看到下方效果图了，恭喜你，outline属性入门咯。精彩继续，下一个案例具有很强的个性化色彩。

```css
clip-path: circle(50%);
```

![1713692654314](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692654314.png)

**多彩球头像**

此时还是找一张自己喜欢的图片，保留body基础样式，清理掉其他案例样式。这里多彩球头像利用了背景属性来配置多个小球，还用到了滤镜效果。代码也很简单，不再做过多赘述，你也可以粘贴到实际项目中去直接使用。

```css

/* 自定义图片背景多彩小球特效 */
.pic-box {
    padding: 40px;
    margin: 0 50px;
    border-radius: 50%;
    cursor: pointer;
    clip-path: circle(calc(50% - 40px));
    --w--: 150px; /* 外边框宽度 */
    --p--: 100px; /* 小球的偏移量*/
    --r1--: radial-gradient(50% 50%,#233E8B 90%,#0000);
    --r2--: radial-gradient(50% 50%,#1EAE98 90%,#0000);
    --r3--: radial-gradient(50% 50%,#A9F1DF 90%,#0000);
    --r4--: radial-gradient(50% 50%,#eb2288 90%,#0000);
    --r5--: radial-gradient(50% 50%,#F7FD04 90%,#0000);
    --r6--: radial-gradient(50% 50%,#81B214 90%,#0000);
    --r7--: radial-gradient(50% 50%,#FC5404 90%,#0000);
    --r8--: radial-gradient(50% 50%,#158467 90%,#0000);
    --r9--: radial-gradient(50% 50%,#01C5C4 90%,#0000);
    --r10--: radial-gradient(50% 50%,#B8DE6F 90%,#0000);
    background: 
    var(--r1--) calc(30% - var(--p--)) calc(10% - var(--p--)),
    var(--r2--) calc(80% + var(--p--)) calc(10% - var(--p--)),
    var(--r3--) calc(80% + var(--p--)) calc(80% + var(--p--)),
    var(--r4--) 55% calc(5% - var(--p--)),
    var(--r5--) calc(18% - var(--p--)) calc(88% + var(--p--)),
    var(--r6--) calc(10% - var(--p--)) calc(70% + var(--p--)),
    var(--r7--) calc(93% + var(--p--)) calc(60% + var(--p--)),
    var(--r8--) calc(88% + var(--p--)) calc(30% + var(--p--)),
    var(--r9--) calc(50% + var(--p--)) calc(99% + var(--p--)),
    var(--r10--) calc(5% + var(--p--)) calc(30% + var(--p--));
    background-size: 18px 18px, 25px 25px, 38px 39px;
    background-repeat: no-repeat;
    filter: grayscale(82%);
    transition: all 0.5s ease;
    outline: var(--w--) solid #0005;
    outline-offset: calc(var(--w--) * -1);
}
.pic-box:hover {
    filter: grayscale(0%);
    clip-path: circle(50%);
    --p--: 0px;
    outline: 3px solid #EF746F;
    outline-offset: -2px;
}
```

```html

<body>
    <img 
      class="pic-box" 
      src="https://picsum.photos/id/823/200/200">
</body>
```

刷新浏览器后，最终效果如下图，也是今天文章outline属性的最后一个案例。看到下图惊艳效果，有没有被css魅力所震撼到，哈哈，反正小编认为css魔法很强大，css内容虽然很多，只要精心积累打磨，滴水石穿，久久为功，还是能写出很多漂亮的交互效果的。

![1713692713561](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713692713561.png)

关于CSS的outline属性就介绍到这里，案例中有不明白的伙伴可以评论区留言讨论。