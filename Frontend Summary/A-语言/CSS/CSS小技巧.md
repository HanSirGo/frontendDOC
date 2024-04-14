### CSS渐变阶梯进度条

```
几行CSS实现一个渐变阶梯进度条。原理很简单，线性渐变配合mask遮罩，注意动画是 steps阶梯动画
```

![1712986461230](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712986461230.png)

![1712986425802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712986425802.png)

### 打字效果

```html
<div class="wrapper">
    <div class="typing-demo">
      有趣且实用的 CSS 小技巧
    </div>
</div>
```

```css
.wrapper {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.typing-demo {
  width: 22ch;
  animation: typing 2s steps(22), blink .5s step-end infinite alternate;
  white-space: nowrap;
  overflow: hidden;
  border-right: 3px solid;
  font-family: monospace;
  font-size: 2em;
}

@keyframes typing {
  from {
    width: 0
  }
}
    
@keyframes blink {
  50% {
    border-color: transparent
  }
}
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711865706009.png" alt="1711865706009" style="zoom:50%;" />

### 设置阴影

```
当使用透明图像时，可以使用 drop-shadow() 函数在图像上创建阴影，而不是使用 box shadow 属性在元素的整个框后面创建矩形阴影：
```

```html
<div class="wrapper">
  <div class="mr-2">
    <div class="mb-1 text-center">
      box-shadow
    </div>
    
    <img class="box-shadow" src="https://markodenic.com/man_working.png" alt="Image with box-shadow">
  </div>
    
  <div>
    <div class="mb-1 text-center">
      drop-shadow
    </div>
    
    <img class="drop-shadow" src="https://markodenic.com/man_working.png" alt="Image with drop-shadow">
  </div>
</div>
```

```css
.wrapper {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.mr-2 {
  margin-right: 2em;
}

.mb-1 {
  margin-bottom: 1em;
}

.text-center {
  text-align: center;
}

.box-shadow {
  box-shadow: 2px 4px 8px #585858;
}

.drop-shadow {
  filter: drop-shadow(2px 4px 8px #585858);
}
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711865814369.png" alt="1711865814369" style="zoom:50%;" />

### 平滑滚动

```
无需 JavaScript 即可实现平滑滚动，只需一行 CSS：scroll-behavior: smooth；
```

### 自定义光标

```css
可以使用自定义图像，甚至表情符号来作为光标。
cursor: url(https://picsum.photos/20/20), auto;

cursor: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg'  width='40' height='48' viewport='0 0 100 100' style='fill:black;font-size:24px;'><text y='50%'>🚀</text></svg>"), auto;
```

![1711867123159](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711867123159.png)

### 自定义选中样式

```
CSS 伪元素::selection，可以用来自定义用户选中文档的高亮样式。

.custom-highlighting::selection {
  background-color: #8e44ad;
  color: #fff;
}
```

### 空元素样式

```
可以使用 :empty 选择器来设置完全没有子元素或文本的元素的样式
```

### 动态工具提示

```
可以使用 CSS 函数 attr() 来创建动态的纯 CSS 工具提示 。

<p>
  Hover <span class="tooltip" data-tooltip="Tooltip Content">Here</span> to see the tooltip.
</p>

.tooltip {
  position: relative;
  border-bottom: 1px dotted black;
}

.tooltip:before {
  content: attr(data-tooltip); 
  position: absolute;
  width: 100px;
  background-color: #062B45;
  color: #fff;
  text-align: center;
  padding: 10px;
  line-height: 1.2;
  border-radius: 6px;
  z-index: 1;
  opacity: 0;
  transition: opacity .6s;
  bottom: 125%;
  left: 50%;
  margin-left: -60px;
  font-size: 0.75em;
  visibility: hidden;
}
```

### 圆形渐变边框

```
<div class="box gradient-border">
  炫酷渐变边框
</div>

.gradient-border {
  border: solid 5px transparent;
  border-radius: 10px;
  background-image: linear-gradient(white, white), 
    linear-gradient(315deg,#833ab4,#fd1d1d 50%,#fcb045);
  background-origin: border-box;
  background-clip: content-box, border-box;
}

.box {
  width: 350px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 100px auto;
}
```

### 毛玻璃特效

```
可以使用 CSS 中的 backdrop-filter 属性来实现毛玻璃特效：

.login {
  backdrop-filter: blur(5px);
}

backdrop-filter 属性可以为一个元素后面区域添加图形效果（如模糊或颜色偏移）。因为它适用于元素_背后_的所有元素，为了看到效果，必须使元素或其背景至少部分透明。
```

![1711866771351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711866771351.png)

###  将文本设为大写或小写

```
大写或小写字母可以不必在 HTML中设置。可以在 CSS 中使用text-transform属性来强制任何文本为大写或小写。

/* 大写 */
.upper {
  text-transform: uppercase;
}

/* 小写 */
.lower {
  text-transform: lowercase;
}

text-transform 属性专门用于控制文本的大小写，当值为uppercase时会将文本转为大写，当值为capitalize时会将文本转化为小写，当值为capitalize时会将每个单词以大写字母开头。
```

![1711866843723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711866843723.png)

### 实现首字下沉

```
我们可以使用::first-letter来实现文本首字母的下沉：

p.texts:first-letter {
  font-size: 200%;
  color: #8A2BE2;
}

:first-letter选择器用来指定元素第一个字母的样式，它仅适用于在块级元素中
```

![1711866877937](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711866877937.png)

### 实现正方形

```
我们可以通过CSS中的纵横比来实现一个正方形，这样只需要设置一个宽度即可：

.square {
  background: #8A2BE2;
  width: 25rem;
  aspect-ratio: 1/1;
}

aspect-ratio 媒体属性可以用来测试视口的宽高比。当然上述例子比较简单，来看看MDN中给出的纵横比的示例：

/* 最小宽高比 */
@media (min-aspect-ratio: 8/5) {
  div {
    background: #9af; /* blue */
  }
}

/* 最大宽高比 */
@media (max-aspect-ratio: 3/2) {
  div {
    background: #9ff;  /* cyan */
  }
}

/* 明确的宽高比, 放在最下部防止同时满足条件时的覆盖*/
@media (aspect-ratio: 1/1) {
  div {
    background: #f9a; /* red */
  }
}
```

### 图片文字环绕

```
shape-outside 是一个允许设置形状的 CSS 属性。它还有助于定义文本流动的区域：

.any-shape {
  width: 300px;
  float: left;
  shape-outside: circle(50%);
}

shape-outside 属性定义了一个可以是非矩形的形状，相邻的内联内容应围绕该形状进行包装。默认情况下，内联内容包围其边距框; shape-outside提供了一种自定义此包装的方法，可以将文本包装在复杂对象周围而不是简单的框中。
```

### :where() 简化代码

```
当对多个元素应用相同的样式时，CSS 可能如下：

.parent div,
.parent .title,
.parent #article {
  color: red;
}
这样代码看起来可读性不是很好，:where() 伪类这时就派上用场了。**:where()** 伪类函数接受选择器列表作为它的参数，将会选择所有能被该选择器列表中任何一条规则选中的元素。

上面的代码使用:where()就可以这么写：

.parent :where(div, .title, #article) {
  color: red;
}
```

### user-select

`user-select` 属性可以用来控制用户是否能够选择文本。

```
 <div>
   <p>You can't select this text.</p>
</div>
<p>You can select this text.</p>
```

CSS：

```
div {
  width: max-content;
  height: 40px;
  border: 3px solid purple;
  user-select: none;
}
```

> **解析**：`user-select` 属性用于控制用户是否能够选择文本。通过设置 `user-select` 属性，可以限制用户对文本的选择行为或禁止选择。该属性可以应用于任何 HTML 元素，并接受以下值：
>
> - `auto`：默认值，表示用户可以选择文本。
> - `none`：禁止用户选择文本。
> - `text`：允许用户选择文本，但不能选择元素的其他部分，如背景、边框等。
> - `all`：允许用户选择元素内的所有内容，包括文本、背景和边框。
>
> `user-select` 属性的应用场景通常涉及到用户交互和用户体验的控制，可以在以下情况下使用该属性：
>
> 1. **防止文本被选中**：在某些情况下，你可能希望防止用户选择特定区域或元素内的文本，例如，防止用户选择输入框中的内容或防止复制敏感信息。通过将 `user-select` 设置为 `none`，可以禁止用户选择这些文本，从而保护数据的安全性。
> 2. **控制文本选择范围**：有时你可能只希望用户能够选择特定的文本内容，而不包括其他元素中的样式信息。通过将 `user-select` 设置为 `text`，可以限制用户只能选择文本内容，而不能选择其他元素的样式信息，从而提供更精确的文本选择控制。
> 3. **自定义选择效果**：使用 `user-select` 属性，你还可以自定义文本选择的外观效果。通过设置适当的 CSS 样式，如更改选中文本的背景色、前景色等，可以为用户提供独特的文本选择体验，增强页面的可视化效果。
> 4. **取消文本选择**：在某些特定情况下，你可能希望用户无法选择任何文本，以防止复制、截屏等操作。通过将 `user-select` 设置为 `none`，可以完全禁止用户选择文本，从而实现取消文本选择的效果。

### pointer-events

可以使用 `pointer-events` 属性来控制元素对指针事件的反应。

```
<div>
   <p class="first">
      Please <a href="https://shefali.dev/blog">Click here</a>
   </p>
   <p class="second">
      Please <a href="https://shefali.dev/blog">Click here</a>
   </p>
</div>
```

CSS：

```
.first {
  pointer-events: none; 
}

.second {
  pointer-events: auto;
}
```

> **解析**：`pointer-events` 属性用于控制元素对指针事件的反应。该属性允许指定一个值来控制元素是否响应鼠标事件、触摸事件或笔事件。它有以下值：
>
> - `auto`：元素按照默认方式响应指针事件。
> - `none`：元素不响应指针事件，事件将向下传递到下一层元素。
> - `visiblePainted`：元素响应指针事件，但只有在元素的背景颜色或图片已经被绘制时才会响应。
> - `visibleFill`：元素响应指针事件，但只有在元素的填充区域内部时才会响应，对于描边无效。
> - `visibleStroke`：元素响应指针事件，但只有在元素的描边区域内部时才会响应，对于填充无效。
> - `visible`：元素响应指针事件，只要它可见且鼠标事件发生在元素的边框框线上或内部。
>
> `pointer-events` 属性的应用场景如下：.
>
> 1. **禁用用户交互**：通过将元素的 `pointer-events` 设置为 `none`，可以阻止用户与该元素进行任何交互操作，如点击、滚动等。这在需要禁用某个元素的交互能力时非常有用。
> 2. **创建自定义的点击区域**：有时候，可能希望一个元素在视觉上占据更大的空间，但只对特定区域响应点击事件。通过将 `pointer-events` 设置为 `none`，然后在需要响应点击的子元素上重新设置为 `auto`，可以创建自定义的点击区域。
> 3. **优化元素叠加情况下的交互**：当多个元素重叠在一起时，可能会出现交互冲突。通过设置不同元素的 `pointer-events` 属性，可以控制哪个元素应该优先响应指针事件，以解决叠加元素之间的交互问题。
> 4. **实现鼠标样式变化**：通过设置 `pointer-events` 属性，可以根据特定的交互状态来改变鼠标样式。例如，在元素被禁用或不可点击时，将 `pointer-events` 设置为 `none`，并将鼠标样式修改为指示不可点击的样式。

### accent-color

当涉及到复选框和单选按钮等输入时，浏览器通常会引入默认颜色，该颜色可能与 UI 配色方案不太协调。

为了保持 UI 的一致性，可以使用accent-color 属性更改输入的默认颜色。

```
<form>
   <input type="radio" id="html" />
   <label for="html">HTML</label>
   <input type="radio" id="css" />
   <label for="css">CSS</label>
   <input type="radio" id="js" />
   <label for="js">JavaScript</label>
</form>
```

CSS:

```
input {
  accent-color: green;
}
```

效果如下：

![1711877337327](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877337327.png)

> **解析**：`accent-color` 属性用于指定元素的强调色。它可以应用于很多元素，例如按钮、链接、输入框、选择框等等，以突出显示它们在页面中的作用。使用该属性可以使你的网页在不同的主题和模式下保持一致的强调色，从而提高网页的可访问性和用户体验。

### backdrop-filter

有时候你可能想要对一个元素后面的区域应用滤镜效果（模糊效果），可以使用 `backdrop-filter` 属性。

```
<div class="container">
  <div class="box">
    <p>This is an example of backdrop-filter property.</p>
  </div>
</div>
```

CSS：

```
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 350px;
  width: 350px;
  background: url(img.webp) no-repeat center;
}
.box {
  padding: 10px;
  font-weight: bold;
  color: white;
  background-color: transparent;
  backdrop-filter: blur(10px);
}
```

效果如下：

![1711877388855](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877388855.png)

> **解析**：`backdrop-filter` 属性用于在元素背景的后面应用滤镜效果。它可以让你创建出模糊、色彩变化或其他视觉效果来改变元素背景区域的外观。通过使用该属性，可以为元素的背景添加一层视觉效果，使背景与页面的其余内容产生视觉上的分离和层次感。这个属性通常与背景图像或颜色一起使用，以提供一种更加丰富和吸引人的设计效果。

### caret-color

当使用 `input` 或 `textarea` 元素时，可以使用 `caret-color` 属性来更改这些元素的文本光标颜色，以匹配网页的配色方案。

```
<input type="text" placeholder="Your Name" />
```

CSS：

```
input {
  caret-color: red;
}
```

> **解析**：`caret-color` 属性用于指定文本光标的颜色。文本光标是指在输入框或文本区域中表示当前输入位置的闪烁符号，该属性可以接受各种颜色值，例如十六进制颜色、RGB 值、颜色名称等。它可以应用于任何支持文本输入和编辑的元素，如 `input`、`textarea` 等。

### image-rendering

可以使用 `image-rendering` 属性来控制缩放图像的渲染方式并优化质量。不过，该属性不会影响未经缩放的图像。

```
img {  image-rendering: pixelated;}
```

> **解析**：`image-rendering` 属性用于控制图像在浏览器中的渲染方式。它可以影响图像在缩放、旋转或变形等操作时的呈现质量。它提供了不同的值，可以让你选择最适合你需求的图像渲染方式。以下是一些常见的属性值：
>
> - `auto`：浏览器默认的图像渲染方式。
> - `crisp-edges`：通过强调图像边缘来实现清晰的渲染效果，适用于像素风格的图像。
> - `pixelated`：通过像素化的方式来渲染图像，适用于放大图像时保持像素风格的效果。
>
> 
>
> 以下是 `image-rendering` 属性的一些常见应用场景：
>
> 1. 改善小图片的清晰度：对于小图片，如果直接展示可能会出现锯齿或者模糊的问题。通过将 `image-rendering` 设置为 `pixelated`，可以让浏览器在缩放图片时使用像素化的方式，从而获得更加清晰的效果。这在需要展示小图标、小徽标等场景下非常有用。
> 2. 提高大图片的加载速度：对于大图片，如果直接展示会导致页面加载速度变慢，影响用户体验。通过将 `image-rendering` 设置为 `crisp-edges`，可以让浏览器在缩放图片时只显示原始边缘，从而加快图片加载速度。
> 3. 在高分辨率设备上显示高清图像：对于高分辨率设备，如果直接展示低分辨率的图片，可能会出现模糊或者失真的问题。通过将 `image-rendering` 设置为 `auto` 或者 `high-quality`，可以让浏览器在高分辨率设备上显示高清的图像，提高用户体验。
> 4. 优化动画效果的展示：当图像用于动画场景时，通过将 `image-rendering` 设置为 `optimizeQuality` 或者 `optimizeSpeed`，可以根据需要平衡图像质量和展示速度，从而优化动画效果的展示。

### mix-blend-mode

如果想要设置一个元素内容与其背景的混合效果，可以使用 `mix-blend-mode` 属性。

```
<div>
  <img src="cat.jpg" alt="cat" />
</div>
```

```
div {
  width: 600px;
  height: 400px;
  background-color: rgb(255, 187, 0);
}
img {
  width: 300px;
  height: 300px;
  mix-blend-mode: luminosity;
}
```

效果如下：

![1711877541946](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877541946.png)

> **解析**：`mix-blend-mode` 属性用于控制元素内容与其背景之间的混合模式。通过设置 `mix-blend-mode` 可以改变元素在视觉上与其周围元素的交互方式。这个属性可以应用于任何具有背景的元素，包括文本、图像和其他 HTML 元素。使用 该属性可以创造出各种独特的视觉效果，如颜色叠加、透明度混合、文字特效等。
>
> 
>
> 常用的 `mix-blend-mode` 值包括：
>
> - `normal`：默认值，没有混合效果。
> - `multiply`：将元素的颜色与背景进行相乘。
> - `screen`：将元素的颜色与背景进行屏幕模式混合。
> - `overlay`：根据元素和背景的亮度进行混合。
> - `darken`：选择较暗的颜色作为最终混合结果。
> - `lighten`：选择较亮的颜色作为最终混合结果。
> - `color-dodge`：通过减少对比度来混合颜色。
> - `color-burn`：通过增加对比度来混合颜色。
> - `difference`：计算颜色之间的差异。
> - `exclusion`：排除两种颜色的共同部分。
> - `hue`：保留元素的色调，应用背景的饱和度和亮度。
> - `saturation`：保留元素的饱和度，应用背景的色调和亮度。
> - `color`：保留元素的色调和饱和度，应用背景的亮度。
> - `luminosity`：保留元素的亮度，应用背景的色调和饱和度。

### object-fit

可以使用 `object-fit` 属性来设置图像或视频的大小调整行为，使其适应其容器。

```
<div>
  <img src="cat.jpg" alt="cat" />
</div>
```

CSS：

```
div {
  width: 500px;
  height: 400px;
  border: 3px solid purple;
}

img {
  width: 500px;
  height: 300px;
  object-fit: cover; 
}
```

效果如下：

![1711877601073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877601073.png)

> **解析**：`object-fit` 属性用于控制替换元素（如 `<img>`、`<video>` 或 `<iframe>`）在其容器中的尺寸调整行为。通过设置 `object-fit` 属性，可以更好地控制替换元素在容器中的尺寸调整行为，确保它们能够正确地适应容器并保持比例。
>
> 
>
> 当替换元素的尺寸与其容器不匹配时，浏览器默认会根据一定规则调整元素的大小和比例。然而，这可能导致图像或视频失真，或者在容器中无法正确显示。`object-fit` 属性可以解决这个问题，它有以下几个取值：
>
> - `fill`：默认值，元素会被拉伸以填充容器，可能导致元素的宽高比发生变化，从而导致元素变形。
> - `contain`：元素会等比例缩放，保持其原始宽高比，并使其适应容器，不会超出容器边界，并且会在容器内居中显示。
> - `cover`：元素会等比例缩放，保持其原始宽高比，并将其放大到填充容器，可能会超出容器边界，但不会变形，并且会在容器内居中显示。
> - `none`：元素会保持其原始大小，不会进行任何尺寸调整。
> - `scale-down`：元素会根据容器的大小来确定是按原始大小显示还是进行缩小

### object-position

`object-position` 属性与 `object-fit` 属性一起使用，用于指定图像或视频在其内容框内的 x/y 坐标上的位置。

```
<div>
  <img src="cat.jpg" alt="cat" />
</div>
```

CSS：

```
div {
  width: 500px;
  height: 400px;
  border: 3px solid purple;
}

img {
  width: 500px;
  height: 300px;
  object-fit: cover;
  object-position: bottom right;
}
```

效果如下：

![1711877648257](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877648257.png)

这里设置了 `object-position: bottom right;` 这意味着在调整图像大小时，它将显示图像的右下角部分。

> **解析**：`object-position` 属性用于指定替换元素（如 `<img>`、`<video>` 或 `<iframe>`）在其容器中的位置。它可以与 `object-fit` 属性一起使用，以控制替换元素的大小和位置。
>
> 
>
> 当使用 `object-fit` 属性调整替换元素的大小时，可能会在容器中留下空白区域。`object-position` 属性允许我们根据需要将替换元素在容器内进行精确定位，以填充空白区域。
>
> 
>
> `object-position` 属性接受两个值：
>
> - 水平定位：使用关键字 `left`、`center` 或 `right`，或者使用百分比或长度值来指定水平方向上的位置。
> - 垂直定位：使用关键字 `top`、`center` 或 `bottom`，或者使用百分比或长度值来指定垂直方向上的位置。

### outline-offset

可以使用 outline-offset 属性来指定描边与元素边框之间的间距。

```
<div></div>
```

```
div {
  width: 300px;
  height: 300px;
  border: 3px solid purple;
  outline: 3px solid rgb(81, 131, 148);
  outline-offset: 10px;
}
```

效果如下：

![1711877692492](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877692492.png)

> **解析**：`outline-offset` 属性用于指定描边（outline）与元素边框之间的间距。它可以用来调整描边的位置和外观。默认情况下，描边与元素的边框紧密相邻，且没有间距。但是，通过使用 `outline-offset` 属性，可以在描边和边框之间创建一个间距，该属性接受长度值或百分比值。正值会将描边向外扩展，负值会将描边向内收缩。

### scroll-behavior

可以使用 scroll-behavior 属性来实现平滑滚动，而无需使用任何 JavaScript，只需要一行 CSS 代码。 

```
html {
  scroll-behavior: smooth;
}
```

> **解析**：`scroll-behavior` 属性用于控制页面滚动的行为。通过设置该属性的值，可以让页面在滚动时呈现出不同的效果，该属性支持两个值：
>
> - `auto`：默认值，表示使用浏览器默认的滚动行为，即瞬间跳转到目标位置。
> - `smooth`：启用平滑滚动效果，当用户通过链接或编程方式触发页面内的锚点跳转或滚动行为时，页面将会平滑滚动到目标位置。
>
> 启用平滑滚动效果可以提升用户体验，使页面滚动更加流畅自然。此外，平滑滚动还有助于减少眼睛视觉的跳跃感和头晕感，并且能够更好地吸引用户的注意力。

### text-justify

当将 text-align 属性的值设置为 justify 时，可以使用 text-justify 属性来设置文本的对齐方式。

```
p {
  text-align: justify;
  text-justify: inter-character;
}
```

> **解析**：`text-justify` 属性用于设置文本的对齐方式，当 `text-align` 属性的值设置为 `justify` 时生效。通过设置 `text-justify` 属性，可以控制文本在行内的分布方式，以填充行内的空白空间，从而实现文本的自动调整和对齐。该属性支持以下几个值：
>
> - `auto`：默认值，由浏览器根据当前文本内容和容器宽度自动选择适合的对齐方式。
> - `none`：取消文本的对齐方式，文本将保持左对齐或右对齐，取决于 `text-align` 属性的值。
> - `inter-word`：使文本在单词之间进行自动调整，填充行内的空白空间。
> - `inter-character`：使文本在字符之间进行自动调整，填充行内的空白空间。

### accent-color

`accent-color` 是从 Chrome 93 开始被得到支持的一个不算太新属性。之前一直没有好好介绍一下这个属性。直到最近在给一些系统整体切换主题色的时候，更深入的了解了一下这个属性。

简单而言，CSS `accent-color` 支持使用几行简单的 CSS 为**表单元素**着色，是的，只需几行代码就可以将主题颜色应用到页面的表单输入。

表单元素一直被吐槽**很难自定义**[1]。而 `accent-color` 就是规范非常大的一个改变，我们开始能更多的自定义原生的表单的样式了！

```html
<div class="wrapper">
 <form action="">
  <fieldset>
   <legend>Accent-color Demo</legend>

   <label>
    Strawberries
    <input type="checkbox" id="berries_1" value="strawberries">
   </label>

   <label>
    Radio Buttons
    <div>
     <input type="radio" name="accented-demo" checked>
     <input type="radio" name="accented-demo">
     <input type="radio" name="accented-demo">
    </div>
   </label>

   <label>
    Range Slider
    <input type="range">
   </label>

   <label>
    Progress element
    <progress max="100" value="50">50%</progress>
   </label>
  </fieldset>
 </form>
</div>
```

只需要最简单的布局 CSS，与 `accent-color` 关系不大，我就不列出来了，这样，我们的 DEMO 大致如下：

![1712999832790](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999832790.png)

可以看到，表单控件的主题颜色是**蓝色**，在之前，我们是没办法修改这个颜色的。

而现在，我们可以简单的借助 `accent-color`，修改表单的主题色：

```css
:root {
 accent-color: rgba(250, 15, 117);
}
```

其中，`rgba(250, 15, 117)` 表示粉色，此时，整体的效果就变成了：

![1712999862826](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999862826.png)

当然，这个 `accent-color` 也支持传入 CSS 变量，配合更多的其他颜色一起进行修改。

我们可以对上述的 DEMO 再简单改造：

```css
:root {
 --brand: rgba(250, 15, 117);
 accent-color: var(--brand);
}
fieldset {
 border: 1px solid var(--brand);
}
legend {
 color: var(--brand);
}
```

我们设置了一个 CSS 变量 `--brand: rgba(250, 15, 117)`，除了把这个颜色赋值给表单的 `accent-color`，还能赋值给其它更多元素。譬如这里，我们赋值给了 `<fieldset>` 的边框色和 `<legend>` 的文字颜色。

这样，当我们修改 CSS 变量值时，整个主题色会一起发生变化：

![1712999905685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712999905685.png)

通常而言，更多的时候，我们会将 `accent-color` 应用于：

- **checkbox**[3]
- **radio**[4]
- **range**[5]
- **progress**[6]

### color-scheme

还有一个容易忽略的细节点。`accent-color` 还支持和 **color-scheme**[8] 一起使用。

OK，什么是 color-scheme 呢？color-scheme 是 CSS 的一个属性，用于指定网页的颜色方案或主题。它定义了网页元素应该使用哪种颜色方案来呈现内容。

color-scheme 属性有以下几个可能的取值：

- auto：表示使用用户代理（浏览器）的默认颜色方案。这通常是浏览器自动根据操作系统或用户设置选择的方案。
- light：表示使用浅色颜色方案。这通常包括浅色背景和深色文本。
- dark：表示使用深色颜色方案。这通常包括深色背景和浅色文本。

通过指定适当的 color-scheme 值，开发者可以为网页提供不同的颜色方案，以适应用户的偏好或操作系统的设置。这有助于提供更好的可访问性和用户体验。

譬如，我们可以将页面的 `color-schema` 设置为 `light dark`：

```css
body {
  color-scheme: light dark;
}
```

上述代码表示页面将同时支持浅色和深色颜色方案。它告诉浏览器，网页希望适应用户代理（浏览器）的默认颜色方案，并同时支持浅色和深色模式。

当使用 `color-scheme: light dark` 时，浏览器会根据用户代理的默认颜色方案来选择适当的颜色方案。如果用户代理处于浅色模式，网页将使用浅色颜色方案来呈现内容；如果用户代理处于深色模式，网页将使用深色颜色方案来呈现内容。

此时，我们的代码可以这样改造：

```css
:root {
 --brand: rgba(250, 15, 117, 1);
 accent-color: var(--brand);
}
@media (prefers-color-scheme: dark) {
 :root {
  --brand: rgba(3, 169, 244, 1);
 }
 
 body {
  background: #000;
  color: #fff;
 }
}
body {
 color-scheme: light dark;
}
```

下面是我在手机上调整日间模式和夜间模式的效果图：

![1713000014877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713000014877.png)

通过 `@media (prefers-color-scheme: dark) {}` 媒体查询，在黑夜模式下，展示不同的 `accent-color`。

