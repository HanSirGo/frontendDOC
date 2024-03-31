## Loading效果

> 文档地址：https://css-loaders.com/

```css
// 这些与效果的 HTML 结构都很简单，只需一行：

 
<div class="loader"></div>

// 想要哪个效果，直接点击就可以复制其 CSS 代码，例如：

 
/* HTML: <div class="loader"></div> */
.loader {
  width: 40px;
  aspect-ratio: 1;
  background: #25b09b;
  clip-path: polygon(0 0,100% 0,100% 100%);
  animation: l2 2s infinite cubic-bezier(0.3,1,0,1);
}
@keyframes l2 {
  25%  {clip-path: polygon(0    0,100% 0   ,0 100%)}
  50%  {clip-path: polygon(0    0,100% 100%,0 100%)}
  75%  {clip-path: polygon(100% 0,100% 100%,0 100%)}
  100% {clip-path: polygon(100% 0,100% 100%,0 0   )}
}
```

> 这里使用 `clip-path` 属性定义一个多边形裁剪路径，形状是一个从左上角到右下角的三角形。然后，使用动画关键帧`@keyframes`定义了一个名为"l2"的动画，在动画的关键帧定义中，根据时间的百分比，通过不断改变clip-path属性的值来实现裁剪路径的变化：
>
> - 25%时，裁剪路径为从左上角到右下角的直线
> - 50%时，裁剪路径为从左上角到右下角的对角线
> - 75%时，裁剪路径为从右上角到右下角的直线
> - 100%时，裁剪路径为从右上角到右下角的对角线
>
> 这样就实现了一个简单的 Loading 效果。

