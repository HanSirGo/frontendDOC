# CSS函数

![1711877866440](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877866440.png)

## 基础

### attr()

`attr()` 函数用于获取被选中元素的某个 HTML 属性值，并在样式文件中使用该值。这个函数也可以应用于伪类元素，此时它获取的是伪元素所依附的原始元素的属性值。

下面来看一个简单的例子：

```
div {  
  background-color: attr(data-color);  
}
```

在这个例子中，如果有一个 `<div>` 元素带有 `data-color` 属性（如 `<div data-color="red">`），那么该元素的背景色将被设置为红色。

注意，`attr()` 函数总是返回一个字符串，因此在使用它时需要确保所获取的属性值能够正确地被解析和应用到样式中。此外，不是所有的 CSS 属性都支持 `attr()` 函数，它通常用于那些可以接受字符串值的属性。

### counter()、counters()

`counter()`和`counters()`函数用于获取和操作计数器的值。CSS计数器是一种可以存储和递增/递减的数字值，通常与列表项、标题、页脚等元素的编号和排序相关。

`counter()`函数用于获取指定计数器的当前值。它接受一个参数，即计数器的名称。这个函数的返回值是计数器的当前数值。

```
selector::before {  
  content: counter(counter-name);  
}
```

其中，`selector`是想要添加计数器值的元素的选择器，`counter-name`是之前使用`counter-reset`和`counter-increment`定义的计数器的名称。

`counters()`函数用于获取多个嵌套计数器的值，并将它们连接成一个字符串。它接受两个或三个参数：计数器名称、分隔符字符串（可选），以及计数器样式（可选）。

```
selector::before {  
  content: counters(counter-name, separator);  
  /* 或者 */  
  content: counters(counter-name, separator, style);  
}
```

- `counter-name`：计数器的名称。
- `separator`：一个字符串，用于分隔不同计数器的值。
- `style`：一个可选参数，指定计数器的显示样式，如decimal（十进制）、upper-roman（大写罗马数字）等。如果省略，将使用默认的十进制样式。

假设有一个嵌套列表，想要显示每个列表项的编号，包括所有嵌套的级别。我们可以使用`counters()`函数来实现这一点。

```
/* 重置计数器 */  
ol {  
  counter-reset: item;  
}  
  
/* 递增计数器 */  
li::before {  
  counter-increment: item;  
  content: counters(item, " "); /* 使用空格作为分隔符 */  
}
```

在这个例子中，每当遇到一个`<li>`元素时，`item`计数器就会递增。`counters(item, " ")`函数会将所有嵌套级别的计数器值连接起来，并使用空格分隔它们。因此，对于一个三级嵌套的列表项，它的内容将类似于“1 2 3”。

注意，为了使计数器工作，还需要使用`counter-reset`和`counter-increment`属性来初始化和递增计数器。同时，`counter()`和`counters()`函数通常与`::before`或`::after`伪元素一起使用，以便在元素的内容之前或之后插入计数器的值。

### url()

`url()` 函数用于引用或包含外部资源，如图像、字体或其他媒体文件。这个函数通常与各种CSS属性一起使用，以指定这些属性所需的资源的位置。

`url()`函数的语法如下：

 

```
url( [ <string> | <uri> ] )
```

其中`<string>`或`<uri>`参数表示资源的URL。这个URL可以是绝对路径（指向完整的互联网地址），也可以是相对路径（相对于当前CSS文件或HTML文件的位置）。

以下是一些`url()`函数在CSS中的使用例子：

1. **背景图像**：

```
body {  
  background-image: url('images/background.jpg');  
}
```

2. **字体文件**：

```
@font-face {  
  font-family: 'MyCustomFont';  
  src: url('fonts/MyCustomFont.woff2') format('woff2'),  
       url('fonts/MyCustomFont.woff') format('woff');  
}
```

3. **列表样式图像**：

```
ul li {  
  list-style-image: url('images/list-bullet.png'); 
}
```

4. **光标**：

```
body {  
  cursor: url('images/custom-cursor.cur'), auto;  
}
```

5. **边框图像**：

```
div {  
  border-image: url('images/border.png') 30% round; 
}
```

在使用`url()`函数时，需要注意：

- URL必须被引号包围，可以是单引号或双引号。
- 如果资源位于同一服务器上，可以使用相对路径。如果资源位于不同的服务器或互联网上，需要使用完整的URL。
- 浏览器会尝试下载并缓存`url()`函数中指定的资源，以便在需要时快速访问。
- 如果资源无法加载（例如，由于404错误或跨域问题），则相关的CSS属性可能不会按预期工作。

### var()

`var()` 函数用于访问CSS自定义属性（CSS变量）的值。CSS 自定义属性允许定义可在整个文档或特定元素范围内重复使用的值。通过使用 `var()` 函数，可以在样式表中引用这些值，从而实现更灵活和可维护的样式。

CSS 变量的声明以两个连字符（`--`）开头，后面跟着变量名和值。

```
:root {  
  --main-color: blue;  
  --secondary-color: #333;  
}
```

在这个例子中，在 `:root` 选择器中定义了两个变量 `--main-colo`r 和 `--secondary-color`。`:root` 选择器指向文档的根元素，这意味着这些变量在整个文档中都是可用的。

然后，可以使用 `var()` 函数来引用这些变量的值。

```
body {  
  background-color: var(--main-color);  
  color: var(--secondary-color);  
}
```

在这个例子中，`var(--main-color)` 会被替换为 `blue`，`var(--secondary-color)` 会被替换为 `#333`。

`var()` 函数也可以接受一个可选的第二个参数，作为变量未定义时的回退值。

```
body {  
  background-color: var(--unknown-variable, red);  
}
```

如果 `--unknown-variable` 没有被定义，`background-color` 将会设置为 `red`。

CSS 变量非常有用，因为它们允许：

- 在一个地方定义颜色、尺寸、字体等，然后在整个样式表中重复使用。
- 动态地改变样式，通过JavaScript更改变量的值。
- 实现更高级的样式逻辑和主题切换。

注意，CSS变量的作用域是它们被定义的位置。如果在元素的选择器中定义了变量，那么该变量只会在那个元素及其子元素中可用。如果在 `:root` 或其他更高级别的选择器（如 html）中定义了变量，那么它们将在整个文档中可用。

### element()

`element()` 函数是 CSS3 中引入的一个背景函数，它能够将网站上的某部分元素作为背景图像来使用。换句话说，它可以将指定的 HTML 元素渲染为 CSS 背景图像。

`element()` 函数的基本语法是 `element(id)`，其中 id 是必需参数，表示要作为背景图像使用的元素的 ID。

假设有一个带有 ID `myElement` 的元素，可以这样使用 `element()` 函数：

```css
#someOtherElement {  
  background-image: element(#myElement);  
}
```

这里，`#someOtherElement` 的背景图像会被设置为 `#myElement` 的内容。

### image-set()

`image-set()` 函数允许为不同的设备像素比提供不同的图像资源，从而确保图像在各种设备上都能以适当的分辨率显示。这对于响应式设计和确保图像在不同设备上的清晰度非常有用。

`image-set()`的基本语法如下：

```
image-set(  
  <image-candidate> [<resolution> [, <image-candidate> [<resolution>]]*  
)
```

其中，`<image-candidate>`是图像的 URL，`<resolution>`是该图像适用的设备像素比。可以指定多个图像候选项，每个都带有其适用的分辨率。浏览器将选择最适合当前设备像素比的图像。

```
.img-responsive {  
  background-image: image-set(  
    url('image-320w.jpg') 1x,  
    url('image-640w.jpg') 2x  
  );  
}
```

在这个例子中，如果设备像素比为 1（标准的非高清设备），浏览器将使用`image-320w.jpg`。如果设备像素比为 2（如 Retina 屏幕），浏览器将使用`image-640w.jpg`，这是一个更高分辨率的图像，可以确保在高清设备上显示的清晰度。

虽然`image-set()`主要用于背景图像，但也可以用于`<img>`标签的`srcset`属性，以实现类似的功能，并允许更复杂的图像选择逻辑，包括基于视口宽度和像素密度的选择。

## 颜色

### rgb()、rgba()

`rgb()` 和 `rgba()` 是 CSS 中用于定义颜色的函数。

`rgb()` 函数用于定义一个颜色，它由三个参数组成：红色（R）、绿色（G）和蓝色（B）。每个参数的值范围都是 0 到 255，代表颜色的强度。

```css
color: rgb(255, 0, 0); /* 红色 */  
background-color: rgb(0, 255, 0); /* 绿色 */
```

`rgba() `函数与 `rgb()` 类似，但它多了一个参数：透明度（Alpha）。Alpha 参数的范围是 0 到 1，其中 0 表示完全透明，1 表示完全不透明。

```css
color: rgba(255, 0, 0, 0.5); /* 半透明的红色 */ background-color: rgba(0, 255, 0, 0.3); /* 30% 不透明的绿色 */
```

使用 `rgba()` 可以实现带有透明度的颜色效果，这在设计需要渐变、阴影或其他视觉效果时非常有用。

### hsl()、hsla()

`hsl()` 和 `hsla()` 是 CSS 中另外两个用于定义颜色的函数，它们基于色相、饱和度、亮度模型，并可选地包含透明度（Alpha）信息。

`hsl() `函数通过三个参数定义颜色：色相（Hue）、饱和度（Saturation）和亮度（Lightness）。

- 色相（Hue）：表示颜色的基本属性，取值范围是 0 到 360 度，代表色轮上的角度。
- 饱和度（Saturation）：表示颜色的纯度，取值范围是 0% 到 100%，其中 0% 表示灰色，100% 表示完全饱和。
- 亮度（Lightness）：表示颜色的明亮程度，取值范围也是 0% 到 100%，其中 0% 表示黑色，100% 表示白色。

```css
color: hsl(0, 100%, 50%); /* 红色 */  background-color: hsl(120, 100%, 25%); /* 深绿色 */
```

`hsla() `函数与 `hsl()` 类似，但它多了一个参数：透明度（Alpha）。Alpha 参数的范围是 0 到 1，其中 0 表示完全透明，1 表示完全不透明。

```css
color: hsla(0, 100%, 50%, 0.5); /* 半透明的红色 */ background-color: hsla(120, 100%, 25%, 0.7); /* 70% 不透明的深绿色 */
```

使用 `hsla()` 可以实现带有透明度的 HSL 颜色效果，这在需要创建复杂的视觉效果或渐变时特别有用。

### hwb()

`HWB()` 是一个颜色函数，它基于 HWB（Hue, Whiteness, Blackness）颜色模型来定义颜色。HWB 模型是 RGB 模型的一个变种，它使用色调（Hue）、白度（Whiteness）和黑度（Blackness）三个参数来定义颜色。

- **色调（Hue）**：与 HSL 和 HSV 模型中的色调相同，表示颜色的基本属性，取值范围是 0 到 360 度。
- **白度（Whiteness）**：表示颜色中白色的成分，取值范围是 0% 到 100%。增加白度会使颜色变得更亮。
- **黑度（Blackness）**：表示颜色中黑色的成分，取值范围也是 0% 到 100%。增加黑度会使颜色变得更暗。

HWB 颜色模型的一个优点是它更符合人类的直观认知，因为可以直接通过调整白度和黑度来控制颜色的亮度和暗度，而不需要像在 RGB 模型中那样同时调整三个分量。

例如，`hwb(0, 0%, 30%)` 表示红色的色调，没有加入白色成分，但加入了 30% 的黑色成分，因此最终的颜色是偏深一点的红色。

`hwb()` 函数在 CSS 中的使用类似于其他颜色函数：

```css
color: hwb(0, 0%, 30%); /* 偏深一点的红色 */ background-color: hwb(120, 50%, 10%); /* 一种绿色调，带有一定的白色和黑色成分 */
```

### lab()

`lab()` 是一个基于 CIELAB 色彩空间的颜色函数。CIELAB 是一种颜色空间，旨在更贴近人眼对颜色的感知。LAB 色彩空间由三个分量组成：L（亮度）、a（从绿色到红色的分量）、b（从蓝色到黄色的分量）。

- **L（亮度）**：表示颜色的亮度，取值范围是 0% 到 100%。0% 代表黑色，100% 代表白色。
- **a**：表示颜色在绿色和红色之间的位置。正值表示偏向红色，负值表示偏向绿色。
- **b**：表示颜色在蓝色和黄色之间的位置。正值表示偏向黄色，负值表示偏向蓝色。

`lab()` 函数在 CSS 中的使用如下：

```css
color: lab(50% -10 20); /* 一个具体的 LAB 颜色值 */ background-color: lab(90% 0 0); /* 高亮度、无 a 和 b 分量的颜色 */
```

与 RGB、HSL 等色彩空间相比，LAB 色彩空间更接近人眼对颜色的视觉感知，因此在某些情况下，使用 LAB 可能会更准确地匹配颜色。然而，`lab()` 函数在 CSS 中的支持度相对较低，可能不是所有浏览器都支持这个函数。

### lch()

`lch()` 是一个基于 CIELCH 色彩空间的颜色函数。CIELCH 色彩空间是 CIELAB 色彩空间的扩展，其中 L 表示亮度，C 表示色度（即色彩的饱和度），H 表示色调（色相）。LCH 色彩空间提供了与 CIELAB 类似的颜色表示，但使用色度（Chroma）代替 a 和 b 分量，这通常更直观且易于理解。

- **L（亮度）**：表示颜色的亮度，取值范围是 0% 到 100%。0% 代表黑色，100% 代表白色。
- **C（色度）**：表示颜色的饱和度，即色彩的鲜艳程度。C 的值越大，颜色越鲜艳；C 的值越小，颜色越接近灰度。
- **H（色调）**：表示颜色的色相，即色彩在色轮上的角度。H 的取值范围是 0 到 360 度，其中 0 度代表红色，120 度代表绿色，240 度代表蓝色等。

`lch()` 函数在 CSS 中的使用如下：

```css
color: lch(50% 60 120deg); /* 一个具体的 LCH 颜色值 */ background-color: lch(90% 0 0deg); /* 高亮度、无色度（即灰色）的颜色 */
```

`lch()` 函数提供了在 CSS 中定义颜色的另一种方式，特别是对于那些需要直接控制亮度和色度而不是红、绿、蓝分量的场景。

### **device-cmyk()**

`device-cmyk()` 用于表示印刷四分色模式（CMYK）的颜色。CMYK 是由青色（Cyan）、品红色（Magenta）、黄色（Yellow）和黑色（Black）四种颜色混合而成的减色混色模型。与 RGB 的加色混色模型不同，CMYK 中颜色混合时，亮度会降低。

在 `device-cmyk()` 函数中，每个颜色分量的取值范围都是 0 到 1，或者可以表示为百分比形式，从 0% 到 100%。这四个参数分别对应 C（青色）、M（品红色）、Y（黄色）和 K（黑色）。

例如，如果想表示一个由 50% 青色、75% 品红色、25% 黄色和 10% 黑色组成的颜色，可以这样写：

```
color: device-cmyk(0.5, 0.75, 0.25, 0.1);
```

或者

```
color: device-cmyk(50%, 75%, 25%, 10%);
```

### color-mix()

`color-mix()` 是 CSS Color Module Level 5 提案中引入的一个实验性功能，用于混合两种颜色。该函数允许指定颜色空间（如 srgb、lch、lab 等）以及每种颜色混合的比例。**`color-mix()` 的语法如下：

```
color-mix(in <color-space>, <color-percentage>, <color>);
```

- `<color-space>`：指定颜色空间，如 srgb、lch、lab 等。
- `<color-percentage>`：指定第一种颜色的混合比例，范围通常是 0% 到 100%。
- `<color>`：指定第二种颜色。

例如，如果想要混合红色（red）和蓝色（blue），并且红色占 70%，蓝色占 30%，可以这样写：

```
div {  
  background-color: color-mix(in srgb, 70%, blue); 
}
```

这将会产生一个偏向于红色的混合色。

### oklab()

`oklab()` 是 CSS Color Module Level 4 中引入的一个实验性颜色函数，用于在 OKLab 色彩空间中定义颜色。OKLab 是一种颜色空间，旨在提供一个更均匀的颜色感知空间，同时保持与 CIELAB 相似的简单性和计算效率。

在 `oklab()` 函数中，颜色由三个分量表示：L（亮度）、O（橙色分量）和 K（黄色分量）。这些分量的取值范围通常是 0 到 1，或者可以转换为百分比形式。

`oklab()` 函数的语法如下：

```
oklab(<number> <number> <number>)
```

其中，每个 `<number>` 分别代表 L、O 和 K 的值。

例如，要定义一个 OKLab 颜色值，可以这样：

```
color: oklab(0.5 0.3 0.2);
```

## 滤镜

### blur()

`blur()` 用于给图像或元素添加模糊效果。它属于 CSS 滤镜功能的一部分，可以在不影响布局的情况下应用各种视觉效果。

`blur()` 函数的语法很简单，它接受一个长度值作为参数，这个值表示模糊的程度。这个长度值可以是像素（px）、百分比（%）或者是 rem 单位。

```
.blurred-element {  
  filter: blur(5px);  
}
```

在这个例子中，`.blurred-element` 类选择器应用于一个 HTML 元素，`blur(5px)` 表示这个元素将应用 5 像素的模糊效果。

注意，blur() 滤镜效果在性能上可能有一定的开销，特别是在移动设备上或当应用于大型图像或元素时。因此，在使用时应谨慎考虑其对页面性能的影响，并尽量避免在不必要的场合使用。

另外，由于 CSS 滤镜在一些较旧的浏览器中可能不受支持，因此在使用 `blur()` 函数时，可能需要添加浏览器前缀（如 `-webkit-`）以确保兼容性，并检查目标浏览器的支持情况。

```
.blurred-element {  
  -webkit-filter: blur(5px); /* Chrome, Safari, Opera */  
  filter: blur(5px);  
}
```

最后，blur() 函数可以与其他 CSS 滤镜函数结合使用，以创建更复杂的效果，如亮度（`brightness()`）、对比度（`contrast()`）和灰度（`grayscale()`）等。

```
.complex-effect {  
  filter: blur(5px) brightness(50%) contrast(200%) grayscale(50%);  
}
```

在这个例子中，`complex-effect` 类将同时应用模糊、亮度、对比度和灰度滤镜效果。

### brightness()

`brightness()` 是一个滤镜函数，用于调整图像或元素的亮度。这个函数允许增加或减少图像或元素的整体亮度，使其看起来更亮或更暗。

`brightness()` 函数接受一个介于 0 和 1 之间的数字作为参数。这个数字表示相对于原始亮度的乘数因子。例如，如果参数是 0.5，那么亮度将降低到原始亮度的一半；如果参数是 1，那么亮度将保持不变；如果参数是 2，那么亮度将增加到原始亮度的两倍。

```
img {  
  filter: brightness(0.7);  
}
```

在这个例子中，所有`<img>`元素的亮度被降低到原始亮度的 70%。

注意，`brightness()` 函数的效果是线性的，这意味着它不会考虑图像中颜色的感知亮度。因此，在某些情况下，增加或减少亮度可能会导致颜色看起来不自然。

### contrast()

`contrast()` 是一个滤镜函数，用于调整图像或元素的对比度。对比度是指图像中最亮和最暗部分之间的差异程度。通过增加或减少对比度，可以改变图像的色彩鲜明度和清晰度。

`contrast()` 函数接受一个数值作为参数，这个数值表示对比度的调整量。这个数值通常大于 0，其中 1 表示原始对比度（即不进行任何调整）。数值小于 1 会减少对比度，使图像看起来更加柔和或褪色；数值大于 1 会增加对比度，使图像看起来更加鲜明和清晰。

```
img {  
  filter: contrast(1.5);  
}
```

在这个例子中，所有`<img>`元素的对比度被增加到原始对比度的 1.5 倍，这通常会使图像看起来更加鲜明。

注意，`contrast()`函数的效果取决于图像本身的色彩和亮度分布。在某些情况下，增加对比度可能会导致图像细节丢失或色彩失真。因此，在使用`contrast()`函数时，应该谨慎调整参数值，以达到最佳效果。

### saturate()

`saturate()` 是一个滤镜函数，用于调整图像或颜色的饱和度。饱和度是指颜色的强度和纯度，即颜色中包含的灰色成分的多少。`saturate()` 函数允许增加或减少颜色的饱和度，从而使颜色更加鲜艳或更加暗淡。

`saturate() `函数接受一个数值作为参数，这个数值表示饱和度的调整量。这个数值通常大于 0，其中 1 表示不进行任何调整，保持原始饱和度。数值小于 1 会减少饱和度，使颜色看起来更加不饱和或灰色；数值大于 1 会增加饱和度，使颜色看起来更加鲜艳。

```
div {  
  filter: saturate(2);  
}
```

在这个例子中，所有 `<div>` 元素的颜色饱和度被增加到原始饱和度的 2 倍，这通常会使颜色看起来更加鲜艳。

注意，`saturate()` 函数的效果取决于原始颜色的饱和度。对于已经接近完全饱和的颜色，增加饱和度可能不会有显著的效果。同时，过度增加饱和度可能会导致颜色看起来过于鲜艳或不自然。

### sepia()

`sepia()`是一个滤镜函数，用于将图像或元素的颜色转换为棕褐色（或称为“褐色”）效果，模仿老照片或复古风格。这个函数接受一个参数，通常是一个百分比值，用于指定转换的强度。

`sepia()`函数的参数范围是 0% 到 100%。当参数为 0% 时，图像或元素保持原样，不进行任何棕褐色转换。随着参数值的增加，图像或元素的颜色逐渐变为棕褐色，直到参数值达到 100% 时，完全转换为棕褐色。

```
img {  
  filter: sepia(75%);  
}
```

在这个例子中，所有`<img>`元素的颜色被转换为 75% 的棕褐色效果。

`sepia()`函数的效果是在不改变图像亮度或对比度的情况下，调整颜色的色调和饱和度，使其呈现出棕褐色的外观。这种效果在网页设计中常用于营造复古或怀旧的感觉。

### invert()

`invert()` 是一个滤镜函数，用于反转图像或元素的颜色。具体来说，它会将图像或元素中的每个颜色值取反，从而得到相应的反色。在数字图像处理中，这个操作也被称为“取反”或“负片”效果。

使用 `invert()` 函数时，不需要指定任何参数，因为它会自动计算颜色值的反色。当应用 `invert()` 滤镜到一个元素上时，该元素中的所有颜色都会被反转。

```
img {  
  filter: invert(1);  
}
```

在这个例子中，所有 `<img>` 元素的颜色被反转。注意，`invert()` 函数的效果取决于图像或元素的原始颜色。对于彩色图像，`invert()` 会将每种颜色分量（红色、绿色和蓝色）取反，从而得到反色图像。

### grayscale()

`grayscale()` 是一个滤镜函数，用于将图像或元素转换为灰度图像。这个函数接受一个参数，通常是一个百分比值，用于指定转换的强度。

`grayscale()`函数的参数范围是 0% 到 100%。当参数为 0% 时，图像或元素保持原样，不进行任何灰度转换。随着参数值的增加，图像或元素的颜色逐渐变为灰度，直到参数值达到 100% 时，完全转换为黑白灰度图像。

```
img {  
  filter: grayscale(50%);  
}
```

在这个例子中，所有`<img>`元素的颜色被转换为 50% 的灰度效果，即半灰度半彩色。

`grayscale()`函数的效果是通过减少图像中每个颜色分量的饱和度来实现的，从而得到灰度图像。这种效果在网页设计中常用于创建简约、低调或复古风格的视觉效果。

### drop-shadow()

`drop-shadow()` 是一个滤镜函数，用于给图像或元素添加阴影效果。它可以接受多个参数来定义阴影的偏移量、模糊半径、扩展半径和颜色。

`drop-shadow()` 函数的语法如下：

```
drop-shadow(offset-x offset-y blur-radius spread-radius color)
```

- `offset-x`: 阴影的水平偏移量。正值会向右偏移，负值会向左偏移。
- `offset-y:` 阴影的垂直偏移量。正值会向下偏移，负值会向上偏移。
- `blur-radius`: 阴影的模糊半径。这个参数是可选的，如果不指定，阴影将不会有模糊效果。
- `spread-radius`: 阴影的扩展半径。这个参数也是可选的，它定义了阴影的大小。正值会增加阴影的大小，负值会缩小阴影。
- `color`: 阴影的颜色。

```
div {  
  filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.5));  
}
```

在这个例子中，所有 `<div>` 元素将会有一个向右下方偏移 2px，模糊半径为 4px，颜色为半透明黑色的阴影。

注意，`drop-shadow()` 函数与 `box-shadow` 属性有些相似，但它们在表现和行为上有一些差异。`box-shadow` 是在元素的边框外添加阴影，而 `drop-shadow()` 则是通过滤镜在图像或元素上直接创建阴影效果。

### hue-rotate()

`hue-rotate()` 是一个滤镜函数，用于调整图像或元素的色调。它接受一个角度值作为参数，该角度值表示色调旋转的角度。正值会增加色调，而负值会减少色调。

`hue-rotate()`函数的语法如下：

```
hue-rotate(angle)
```

其中`angle`是一个表示旋转角度的值，可以是正数或负数。角度值是以度（`deg`）为单位的，但不需要指定单位，因为 CSS 会自动解析它。

```
img {  
  filter: hue-rotate(90deg);  
}
```

在这个例子中，所有`<img>`元素的色调将被旋转 90 度。这意味着图像中的颜色将按照色相环的方向旋转 90 度，从而实现色调的调整。

## 渐变

### linear-gradient()、**repeating-linear-gradient()**

`linear-gradient()`和 `repeating-linear-gradient()` 用于创建**线性渐变**效果。它们都可以用来在两个或多个颜色之间创建平滑的过渡。

`linear-gradient()`函数用于生成线性渐变背景。它接受至少两种颜色作为参数，并且可以根据需要接受更多的参数来定义渐变的方向、颜色停止点等。其语法如下：

```
linear-gradient(angle | to direction, color-stop1, color-stop2, ...);
```

- `angle`：定义渐变线的角度。
- `to direction`：使用关键词定义渐变的方向（如 to top, to right 等）。
- `color-stop`：定义渐变中的颜色停止点。

```
background: linear-gradient(to right, red, yellow);
```

这将创建一个从左到右的红色到黄色的线性渐变背景。

`repeating-linear-gradient()`函数与 `linear-gradient()` 类似，但是它创建的渐变会无限地重复。这意味着渐变模式会在容器内重复填充，直到填满整个容器。参数与 `linear-gradient()` 相同，但效果是重复的。

```
background: repeating-linear-gradient(to right, red, yellow 10px);
```

这将创建一个从左到右的重复线性渐变，其中红色和黄色之间的过渡每10像素重复一次。

### radial-gradient()、**repeating-radial-gradient()**

`radial-gradient()` 和 `repeating-radial-gradient()` 用于创建**径向渐变**（也称为圆形渐变或镜像渐变）效果。这些渐变从一个中心点开始，向外扩散或重复。

`radial-gradient()` 函数用于生成径向渐变背景。它至少需要两种颜色作为参数，并可以指定渐变的中心、形状（圆形或椭圆形）以及大小。其语法如下：

```
radial-gradient(shape size at position, color-stop1, color-stop2, ...);
```

- `shape`：可以是 circle（圆形）或 ellipse（椭圆形）。
- `size`：定义渐变的大小，如 `closest-side,` `farthest-side`, `closest-corner`, `farthest-corner`，或者使用具体的长度值。
- `at position`：定义渐变中心的位置。
- `color-stop`：定义渐变中的颜色停止点。

```
background: radial-gradient(circle, red, yellow);
```

这将创建一个从中心开始的红色到黄色的圆形径向渐变背景。

`repeating-radial-gradient()` 函数与 `radial-gradient()` 类似，但它创建的渐变会无限地重复。这意味着渐变模式会在容器内重复填充，直到填满整个容器。其语法与 `radial-gradient()` 相同，但效果是重复的。

```
background: repeating-radial-gradient(circle, red, yellow 10%);
```

这将创建一个从中心开始的重复径向渐变，其中红色和黄色之间的过渡每`10%`的径向距离重复一次。

### conic-gradient()、repeating-conical-gradient()

`conic-gradient()` 和 `repeating-conic-gradient()` 用于创建锥形渐变和重复的锥形渐变效果。这些渐变从一个中心点开始，沿着辐射状的线条向外扩散。

`conic-gradient()` 函数用于生成锥形渐变背景。它接受多个颜色停止点作为参数，每个颜色停止点由一个颜色值和一个可选的角度（或百分比）组成，表示渐变中颜色变化的位置。其语法如下：

```
conic-gradient(color-stop1 angle1, color-stop2 angle2, ...);
```

- `color-stop`：定义渐变中的颜色停止点。
- `angle`：定义颜色停止点的位置，可以是度数（0到360之间）或百分比（0%到100%之间）。

```
background: conic-gradient(red 0%, yellow 100%);
```

这将创建一个从红色渐变到黄色的锥形渐变背景，其中红色位于渐变的起点（0度或0%），黄色位于渐变的终点（360度或100%）。

`repeating-conic-gradient()` 函数与 `conic-gradient()` 类似，但是它创建的渐变会无限地重复。这意味着锥形渐变模式会在容器内重复填充，直到填满整个容器。语法与 conic-gradient() 类似，但渐变会重复。

```
background: repeating-conic-gradient(red 0% 25%, yellow 25% 50%);
```

这将创建一个重复的锥形渐变，其中红色和黄色之间的过渡从0%到25%，然后从25%到50%重复这个过渡。

## 数学

###  calc()

`calc()` 用于在 CSS 表达式中进行计算的功能，允许在 CSS 的属性值中使用基本的数学运算。

`calc()` 函数的主要用途是动态计算长度值。这意味着可以使用加、减、乘、除等运算符来组合不同的单位，并得出一个结果作为 CSS 属性的值。这在需要根据其他属性值或视口大小（如 vw、vh、vmin、vmax 等）动态调整元素尺寸时特别有用。

例如，如果想设置一个元素的宽度为其父元素宽度的一半减去 50px，可以这样写：

```
.element {  
  width: calc(50% - 50px);  
}
```

`calc()` 函数也可以和其他 CSS 函数一起使用，例如 `min()`, `max()` 和 `clamp()`。这些函数允许设置一个值的范围或限制，`calc()` 可以在这个范围内进行计算。

例如，使用 `min()`和 `max()` 来确保元素的高度始终在 100px 和 200px 之间：

```
.element {  
  height: calc(min(max(100px, 20vw), 200px));  
}
```

在这个例子中，`20vw` 会根据视口的宽度变化，但使用 `min()` 和 `max()` 函数可以确保计算出来的高度值始终在 `100px` 和 `200px` 之间。

注意，`calc()` 函数中的运算符两边必须有空格，否则它将不会被正确解析。例如，`calc(50% - 50px)` 是正确的，而 `calc(50%-50px)` 则是错误的。

**基本使用**

calc() 函数有助于在 CSS 属性值中执行计算。它可以用在如下属性中：length、frequency、angle、time、percentage、number、integer。

```css
width: calc(100% - 80px);
```

calc() 函数用一个表达式作为它的参数，用这个表达式的结果作为值。这个表达式可以是任何如下操作符的组合（+、-、*、/），采用标准操作符处理法则的简单表达式。表达式中的运算对象可以使用任意长度的值。如果你愿意，甚至可以在一个表达式中混用这类值的不同单位。在需要时，还可以使用小括号来建立计算顺序。

该函数具有广泛的用途，例如，如果希望某些内容占据大部分视口高度。这时就可以使用相对的单位vh和绝对单位px进行混合计算：

```css
.content {
  height: calc(100vh - 60px);
}
```

当视口大小发生变化或者用户使用较大或较小的设备时，100vh的值将动态更新，因此计算也将动态更新。使用calc() 的好处在于，它既可以避免添加一些 magic number，也可以避免使用 JavaScript 来计算样式的属性值。

**生成调色板**

我们可以通过传入CSS自定义属性来扩展 `calc()` 的功能。一个非常有用的例子就是使用 `hsl()` （表示色调、饱和度和亮度）来创建一致的调色板。给定饱和度、亮度和起始色调的值，就可以使用 calc() 函数计算互补值来构建完整的调色板。

```css
.colors {
  --base-hue: 140;
  --saturation: 95%;
  --lightness: 80%;
  --rotation: 60;

  color: #222;
  text-align: center;
}

.color {
  padding: 0.25rem;
  background-color: hsl(var(--hue), var(--saturation), var(--lightness));
}

.color1 {
  --hue: calc(var(--base-hue));
}

.color2 {
  --hue: calc(var(--base-hue) + var(--rotation));
}

.color3 {
  --hue: calc(var(--base-hue) + var(--rotation) * 2);
}
```

![1711868430848](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711868430848.png)

**注意事项**

> 在使用 `calc()` 函数时需要注意：
>
> - **+ 和 - 运算符的两边必须要有空白字符。** 比如，`calc(50% -8px)` 会被解析成为一个无效的表达式，解析结果是：一个百分比 后跟一个负数长度值。而加有空白字符的、有效的表达式 `calc(8px + -50%)`会被解析成为：一个长度 后跟一个加号 再跟一个负百分比。
>
> - 
>
> - - 和 / 这两个运算符前后不需要空白字符，但如果考虑到统一性，仍然推荐加上空白符。
>
> - 用 0 作除数会使 HTML 解析器抛出异常。
>
> - 涉及自动布局和固定布局的表格中的表列、表列组、表行、表行组和表单元格的宽度和高度百分比的数学表达式，auto 可视为已指定。
>
> - `calc()` 函数支持嵌套，但支持的方式是：把被嵌套的 calc() 函数全当成普通的括号。所以，函数内直接用括号就好了。

### clamp()

`clamp()` 用于将一个值限制在一个特定的范围内。这个函数接受三个参数：最小值（MIN）、首选值（VAL）和最大值（MAX）。

- 如果首选值（VAL）在最小值（MIN）和最大值（MAX）之间，那么 `clamp()` 函数将返回首选值。
- 如果首选值（VAL）小于最小值（MIN），那么函数将返回最小值。
- 如果首选值（VAL）大于最大值（MAX），那么函数将返回最大值。

这个函数的语法是 `clamp(MIN, VAL, MAX)`。它特别有用在需要根据不同条件动态调整 CSS 属性的值，同时确保这个值始终在一个可接受的范围内。

例如，假设有一个元素的字体大小需要根据视口宽度进行调整，但希望字体大小始终在 12px 和 24px 之间。可以这样使用 `clamp()` 函数：

```
h1 {  
  font-size: clamp(12px, 2vw, 24px);  
}
```

在这个例子中，2vw 是首选值，它根据视口的宽度变化。如果视口宽度很小，导致 2vw 计算出来的值小于 12px，`clamp()` 函数将返回 12px。如果视口宽度很大，导致 2vw 计算出来的值大于 24px，`clamp()` 函数将返回 24px。如果 2vw 计算出来的值在 12px 和 24px 之间，那么 `clamp()` 函数将直接返回这个计算值。

这个函数为响应式设计和动态布局提供了很大的灵活性，因为它允许开发者定义一个值的范围，而不是固定一个具体的值。

**基本使用**

clamp() 的实际用途：在可接受的值范围内设置边界。`clamp()` 被用在 `length`、`frequency`、`angle`、`time`、`percentage`、`number`、`integer` 属性中。其语法如下：

```
clamp(MIN, VAL, MAX) 
```

> clamp(MIN, VAL, MAX) 其实就是表示 max(MIN, min(VAL, MAX))。clamp() 函数接收三个用逗号分隔的表达式作为参数，按最小值、首选值、最大值的顺序排列，这三个值得顺序很重要。
>
> - 当首选值比最小值要小时，则使用最小值。
> - 当首选值介于最小值和最大值之间时，用首选值。
> - 当首选值比最大值要大时，则使用最大值。
>
> 这个表达式可以是数学函数、字面量或其它计算为有效的参数类型表达式，如 attr()，或嵌套的 min 和 max 。作为数学表达式，可以使用加减乘除运算而无需使用 calc() 函数。也可以用括号来确定计算顺序。表达式中的每一个值都可以用不同的单位。

```
.element {
  width: clamp(200px, 50%, 1000px);
}
```

有一个元素，其最小宽度为200px，首选值为50%，最大值为1000px。

![1711874713264](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874713264.png)

这个元素的宽度永远不会低于200px，首选值是50%并且仅在视口宽度大于400px和小于2000px 时才有效，宽度不会超过1000px。

那 `clamp()` 是如何计算的呢？当` clamp()` 用作值时，它等同于使用`max()`和`min()`函数：

```
.element {
  width: clamp(200px, 50%, 1000px);
  width: max(200px, min(50%, 1000px));
}
```

这里的50%取决于浏览器视口宽度，假设视口宽度为1150px，解析的过程如下：

```
1. width: max(200px, min(50%, 1000px));
2. width: max(200px, min(575px, 1000px));
3. width: max(200px, 575px);
4. width: 575px;
```

**字体大小**

![1711874802288](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874802288.png)

假设需要要一个最小为16px，最大为50px的标题。`clamp()`函数将给出一个介于两者之间的值，而不会低于最小值或超过最大值。

```
.title {
  font-size: clamp(16px, 5vw, 50px);
}
```

在这里使用`clamp()` 就非常完美，因为它可以确保使用的字体大小易于访问和阅读。如果要使用 `min()` 设置最大字体，就无法在小视口中控制字体。

```
.title {
  font-size: min(3vw, 24px); 
}
```

![1711874838967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874838967.png)

在移动视口中，字体很小。因此，不要只对字体大小使 `min()` 函数。当然，也可以通过媒体查询来取消或添加它。正如上面所说的，可以在 `max()` 函数中嵌套一个 `min()`，这将模拟`clamp()`函数：

```
.title {
  font-size: max(16px, min(10vw, 50px));
}
```

**边框和阴影**

有时需要边框宽度和半径应该在移动设备上更小，通过使用 `clamp()` 就可以根据视口宽度来使它们动态变化。

```
.element {
  box-shadow: 0 3px 10px 0 red;
  border: min(1vw, 10px) solid #468eef;
  border-radius: clamp(7px, 2vw, 20px);
  box-shadow: 0 3px clamp(5px, 4vw, 50px) 0 rgba(0, 0, 0, 0.2);
}
```

![1711874889760](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874889760.png)

![1711874902179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874902179.png)

**Grid 间隙**

当使用 CSS Grid 布局时设置网格间隙，并且希望为移动视口缩小间隙。就可以使用 `clamp()`：

```
.wrapper {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  grid-gap: clamp(1rem, 2vw, 24px);
}
```

![1711874934178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874934178.png)

**注意事项**

- 允许设置表达式的值为 `max()` 和 `min()` 。这些表达式是数学表达式，所以你可以进行加减乘除运算。
- 表达式可以是使用标准运算符优先规则组合  + 、- 、* 、/ 等运算符的值，确保在 + 、- 两侧各有一个空格。表达式中的操作数可以是长度值。可以给每一个值设置不同的单位。还可以用括号来确定计算顺序。

### min()

### max()

`min()` 和 `max()` 函数用于限制一个值的最小和最大值。它们通常与 `calc()` 函数结合使用，以确保计算出的结果落在指定的范围内。

`min()` 函数接受两个或更多参数，并返回其中的最小值。如果参数是数值，它会返回最小的数值；如果参数是长度值，它会返回最短的长度。

```
.element {  
  width: calc(min(50%, 300px));  
}
```

在这个例子中，元素的宽度将被设置为 50% 和 300px 中的较小值。如果容器的宽度小于 600px，元素的宽度将是容器宽度的一半；如果容器的宽度大于或等于 600px，元素的宽度将是 300px。

`max()` 函数与 `min()` 函数类似，但它返回的是参数中的最大值。

```
.element {  
  height: calc(max(100px, 10vh));  
}
```

在这个例子中，元素的高度将被设置为 100px 和 10% 视口高度中的较大值。如果视口的高度小于 1000px，元素的高度将是 100px；如果视口的高度大于或等于 1000px，元素的高度将是视口高度的 10%。

注意，`min()` 和 `max()` 函数接受的参数类型必须一致（即都是数值或都是长度值），否则它们将无法正确工作。此外，`min()` 和 `max()` 函数也可以用在其他接受数值或长度值作为参数的 CSS 属性中，不仅仅是 `width`、`height`、`font-size` 等属性。

### min()

**基本使用**

`min()` 函数方法接受一个或多个用逗号分隔的表达式作为他的参数，数值最小的表达式的值将会作为指定的属性的值。

`min()` 的实际用途：以包含元素响应上下文的方式，在最大允许值上设置边界。尽管是 `min()` 函数，但结果是提供的值将作为属性允许的最大值。

下面来看一个简单的例子，希望元素的最大宽度为500px：

```
.element {
  width: min(50%, 500px);
}
```

![1711874973620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874973620.png)

浏览器必须选择最小的值（50%，500px）。选择该选项取决于视口宽度。如果50%的计算值大于500px，那么它将被忽略，而使用500px。否则，如果50%的计算值小于500px，则50%将用作宽度值。视口宽度为1000px：

![1711874990681](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711874990681.png)

**容器宽度**

![1711875010595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711875010595.png)

有一个容器的宽度是其父级宽度的80%，并且宽度不应超过780px，那么该如何写：

```
.container {
  max-width: 780px;
  width: 80%;
}
```

如果使用 `min()` 函数，就可以这样设置最大值，代码简洁了很多：

```
.container {
  max-width: min(80%, 780px);
}
```

**平滑渐变**

在 CSS 中使用渐变时，可能需要通过使颜色之间的过渡更平滑一些来针对移动设备进行一些调整。来看一个例子：

```
.element {
  background: linear-gradient(135deg, #2c3e50, #2c3e50 60%, #3498db);
}
```

![1711875078629](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711875078629.png)

在移动设备中，这个颜色渐变就会有一条分割线，不是很好看。可以通过媒体查询来解决这个问题：

```
@media (max-width: 700px) {
  .element {
    background: linear-gradient(135deg, #2c3e50, #2c3e50 25%, #3498db);
  }
}
```

虽然这样是可行的，但我们可以使用CSS 中的 min() 函数使这个颜色渐变更加动态：

```
.element {
  background: linear-gradient(135deg, #2c3e50, #2c3e50 min(20vw, 60%), #3498db);
}
```

这样颜色渐变就看起来顺滑了很多：

![1711875139270](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711875139270.png)

**注意事项**

- 表达式中的操作数可以是任何长度值。表达式中的每个值可以使用不同的单位。必要时，还可以使用括号来确定计算顺序。
- 通常需要组合 `min()`和 `max()`的值，或者在 `clamp()` 或 `calc()` 函数中使用 `min()`。
- 如果需要应用多个长度约束，则可以提供两个以上的参数。

### max()

**基本使用**

`max()` 函数方法接受一个或多个用逗号分隔的表达式作为他的参数，数值最大的表达式的值将会作为指定的属性的值。

`max()` 的实际用途：以包含元素响应上下文的方式，在最小允许值上设置边界。

下面来看一个简单的例子，通常，页面的侧边栏有一个固定的宽度，而主宽度是灵活大小的。如果视口足够大，可以使侧边栏更具动态性以占用更多空间。为此，需要使用 max() 函数为其设置**最小宽度。**

```
.wrapper {
  display: flex;
}

aside {
  flex-basis: max(30vw, 150px);
}

main {
  flex-grow: 1;
}
```

侧边栏的最小宽度为150px，如果视口宽度大于500px（500*30%=150），则最小宽度为30vw。

## 图形

### circle()

`circle()` 函数用于定义一个**圆形区域**，并可以用于多种 CSS 属性中，如`shape-outside`或`clip-path`。这个函数允许指定一个圆形的半径和位置，以创建特定的布局或剪裁效果。

- 在`shape-outside`属性中，`circle()`函数定义了一个围绕元素内容的圆形路径，使得文本或其他内容可以环绕在这个圆形的外部流动。这对于创建独特的文本布局和视觉效果非常有用。
- 在`clip-path`属性中，`circle()`函数则用于剪裁元素的内容，只显示圆形区域内的部分。这可以用于创建圆形头像、圆形图标等效果。

`circle()`函数的语法如下：

```
circle( [ <length> | <percentage> ]? [ at <position> ]? )
```

- `<length>`或`<percentage>`参数定义了圆形的半径。如果省略，浏览器会提供一个默认值。
- `at <position>`参数是可选的，用于指定圆形的位置。如果省略，圆形将居中于元素。

例如，可以使用以下代码来创建一个半径为`100px`，位置在`(50px, 50px)`的圆形：

```
shape-outside: circle(100px at 50px 50px);
```

或者，如果想要创建一个半径为50%的圆形，使其始终与元素的大小相匹配，可以这样写：

```
clip-path: circle(50%);
```

### ellipse()

`ellipse()` 函数用于定义一个椭圆形区域。与`circle()`函数类似，`ellipse()`函数也可以用于多种CSS属性中，如`shape-outside`或`clip-path`，以创建特定的布局或剪裁效果。

- 在`shape-outside`属性中，`ellipse()`函数定义了一个围绕元素内容的椭圆形路径，使得文本或其他内容可以环绕在这个椭圆形的外部流动。这对于创建独特的文本布局和视觉效果非常有用，特别是在需要文本环绕非圆形区域时。
- 在`clip-path`属性中，`ellipse()`函数则用于剪裁元素的内容，只显示椭圆形区域内的部分。这可以用于创建椭圆形头像、椭圆形图像裁剪等效果。

`ellipse()`函数的语法通常如下：

```
ellipse( [ <length> | <percentage> ]{2} [ at <position> ]? )
```

- `<length>`或`<percentage>`参数定义了椭圆的水平和垂直半径。可以提供两个值来分别指定椭圆的宽度和高度。
- `at <position>`参数是可选的，用于指定椭圆形的位置。如果省略，椭圆形将居中于元素。

例如，以下代码创建了一个水平半径为100px，垂直半径为50px，位置在(50px, 50px)的椭圆形：

```
shape-outside: ellipse(100px 50px at 50px 50px);
```

或者，如果想要创建一个与元素大小成比例的椭圆形，可以使用百分比值：

```
clip-path: ellipse(50% 75%);
```

这将创建一个宽度为元素宽度50%，高度为元素高度75%的椭圆形剪裁。

### inset()

`inset()` 函数用于定义图形剪裁区域的函数，主要在`clip-path`属性中使用。它允许创建一个矩形或带有圆角的矩形剪裁区域，该区域定义了元素内容应该显示的部分。

`inset()`函数的语法如下：

```
inset( [ <length> | <percentage> ]{1,4} [ round <border-radius> ]? )
```

- `[ <length> | <percentage> ]{1,4}`：这个参数指定了矩形边框的偏移量。可以提供1到4个值，分别代表上、右、下、左四个方向的偏移。如果只给出一个值，则所有四个方向都将使用这个值。如果给出两个值，第一个值将用于上下偏移，第二个值将用于左右偏移。如果给出三个值，它们将分别用于上、左右、下的偏移。如果给出四个值，它们将分别用于上、右、下、左的偏移。这些值可以是长度单位（如px、em等）或百分比。
- `round <border-radius>`：这是一个可选参数，用于指定矩形的圆角半径。可以提供一个或多个值来定义不同角的圆角大小。这些值遵循与CSS的`border-radius`属性相同的语法和规则。

```css
/* 创建一个从边框向内偏移10px的矩形剪裁区域 */  
clip-path: inset(10px);  
  
/* 创建一个从顶部边框向内偏移20px，从右侧和左侧边框向内偏移30px，从底部边框向内偏移40px的矩形剪裁区域 */  
clip-path: inset(20px 30px 40px);  
  
/* 创建一个从顶部边框向内偏移10%，从右侧和左侧边框向内偏移20%，从底部边框向内偏移30%，并带有50px圆角的矩形剪裁区域 */  
clip-path: inset(10% 20% 30% round 50px);  
  
/* 创建一个带有四个不同圆角的矩形剪裁区域 */  
clip-path: inset(20px round 10px 20px 30px 40px);
```

注意，`inset()`函数在clip-path属性中使用时，将创建一个内部剪裁效果，只显示矩形区域内的内容，而矩形外部的内容将被剪裁掉。

### polygon()

`polygon()` 函数用于定义一个由直线段组成的多边形剪裁区域。这个函数通常在`clip-path`属性中使用，以剪裁图像或文本，只显示多边形区域内的部分。

`polygon()`函数的语法如下：

```
polygon( [ <fill-rule> || <length-percentage>{2,n} ] )
```

- `<fill-rule>`：可选参数，用于定义当路径自相交时如何填充多边形。它可以是`nonzero`（默认值）或`evenodd`。
- `<length-percentage>{2,n}`：这个参数是必需的，它定义了多边形的每个顶点的坐标。每个顶点由两个值表示，分别是X和Y坐标。可以提供至少两个顶点来定义一个简单的多边形，每个顶点对由空格分隔。

以下是一些使用`polygon()`函数的例子：

```css
/* 定义一个三角形剪裁区域 */  
clip-path: polygon(50% 0%, 0% 100%, 100% 100%);  
  
/* 定义一个四边形（矩形）剪裁区域 */  
clip-path: polygon(0% 0%, 100% 0%, 100% 100%, 0% 100%);  
  
/* 定义一个五边形剪裁区域 */  
clip-path: polygon(0% 0%, 25% 100%, 50% 0%, 75% 100%, 100% 0%);  
  
/* 使用 fill-rule */  
clip-path: polygon(nonzero, 50% 0%, 0% 100%, 100% 100%);
```

这里，每个多边形的顶点坐标都是相对于元素自身的宽度和高度的百分比。可以使用绝对长度单位（如px, em等）代替百分比，但这通常不常见。

注意，`polygon()`函数定义的顶点顺序很重要，它决定了多边形的形状和方向。

### path()

`path()`函数与`clip-path`属性结合使用，用于定义图形剪裁区域。该函数允许指定一个SVG路径来定义剪裁的形状。通过`path()`函数，可以使用SVG路径语法来创建复杂的剪裁区域，从而实现更精确的图像或文本剪裁效果。

`path()`函数的语法如下：

```
clip-path: path(<svg-path-data>);
```

其中`<svg-path-data>`是一个SVG路径数据字符串，它描述了剪裁区域的形状。这个字符串可以使用SVG路径命令和参数来定义，例如`M（moveto）`、`L（lineto）`、`C（curveto）`等。

以下是一个使用`path()`函数剪裁图像的例子：

```
.clipped-image {  
  clip-path: path('M0,0 L1,0 L1,1 L0,1 Z');  
}
```

这里，`path('M0,0 L1,0 L1,1 L0,1 Z')`定义了一个矩形剪裁区域。这个路径从点`(0,0)`开始，沿直线到`(1,0)`，然后到`(1,1)`，再到`(0,1)`，最后关闭路径（Z表示回到起始点）。

## 变换

### scale()

### scaleX()

### scaleY()

### scaleZ()

### scale3d()

`scale()`、`scaleX()`、`scaleY()` 和 `scale3d()` 是 CSS3 中的**缩放函数**，用于调整元素的大小。这些函数可以单独或组合使用，以实现元素在二维或三维空间中的缩放效果。

`scale()` 函数用于同时调整元素在水平和垂直方向上的大小。它接受两个参数，第一个参数表示水平缩放比例，第二个参数表示垂直缩放比例。如果只提供一个参数，那么第二个参数默认与第一个参数相同，实现等比例缩放。

```
transform: scale(2); /* 元素在水平和垂直方向上放大两倍 */  
transform: scale(1.5, 0.5); /* 元素在水平方向上放大1.5倍，垂直方向上缩小到一半 */
```

`scaleX()` 函数仅用于调整元素在 X 轴上的大小。它接受一个参数，表示水平缩放比例。

```
transform: scaleX(2); /* 元素在水平方向上放大两倍 */
```

`scaleY()` 函数仅用于调整元素在 Y 轴上的大小。它接受一个参数，表示垂直缩放比例。

```
transform: scaleY(0.5); /* 元素在垂直方向上缩小到一半 */
```

`scaleZ()` 函数仅用于调整元素在 Z 轴上的大小。它接受一个参数，表示在 Z 轴缩放比例。

```
transform: scaleZ(0.5); /* 元素在Z轴上缩小到一半 */
```

`scale3d()` 函数用于在三维空间中调整元素的大小。它接受三个参数，分别表示在X轴、Y轴和Z轴上的缩放比例。

```
transform: scale3d(1, 2, 0.5); /* 元素在X轴上不变，Y轴上放大两倍，Z轴上缩小到一半 */
```

注意：当使用多个变换函数时，它们应该通过空格分隔，并按特定的顺序应用（例如，先应用 `scale()`，然后是 `rotate()` 等）。

### rotate()

### rotateX()

### rotateY()

### rotateZ()

### rotate3d()

`rotate()`、`rotateX()`、`rotateY()`、`rotateZ()`、`rotate3d()` 是 CSS3 中的**旋转函数**，用于调整元素在二维或三维空间中的旋转角度。这些函数可以作为 `transform` 属性的值来使用，以实现元素的旋转效果。

`rotate()` 函数用于在二维空间中旋转元素。它接受一个参数，表示旋转的角度（以度或弧度为单位）。旋转是相对于元素的中心点进行的，并且按照顺时针方向进行。

```
transform: rotate(45deg); /* 元素顺时针旋转 45 度 */
```

`rotateX()` 函数用于在三维空间中绕 X 轴旋转元素。它接受一个参数，表示旋转的角度。正值表示元素按照顺时针方向旋转，负值表示逆时针方向旋转。

```
transform: rotateX(90deg); /* 元素绕 X 轴顺时针旋转 90 度 */
```

`rotateY()` 函数用于在三维空间中绕 Y 轴旋转元素。它的参数和用法与 rotateX() 相同，只是旋转轴是 Y 轴。

```
transform: rotateY(-45deg); /* 元素绕 Y 轴逆时针旋转 45 度 */
```

`rotateZ()` 函数用于在三维空间中绕 Z 轴旋转元素。它的参数和用法与 `rotateX()` 和 `rotateY()` 相同，只是旋转轴是 Z 轴。在二维空间中，`rotateZ()`的效果与 `rotate()` 相同。

```
transform: rotateZ(180deg); /* 元素绕 Z 轴旋转 180 度 */
```

`rotate3d()` 函数用于在三维空间中沿任意向量旋转元素。它接受四个参数，分别表示旋转向量的 X、Y、Z 分量以及旋转的角度。通过指定不同的向量分量，可以实现沿任意方向的旋转。

```
transform: rotate3d(1, 0, 0, 60deg); /* 元素沿 X 轴方向旋转 60 度 */
```

### translate()

### translatex()

### translatey()

### translateZ()

### translate3d()

`translate()`、`translatex()`、`translatey()`、`translateZ()`、`translate3d()` 是 CSS3 中的**平移函数**，用于在二维或三维空间中移动元素的位置。这些函数可以作为 `transform` 属性的值来使用，以实现元素的平移效果。

`translate()` 函数用于在二维空间中移动元素。它接受两个参数，第一个参数表示在 X 轴上的移动距离，第二个参数表示在 Y 轴上的移动距离。如果只提供一个参数，那么第二个参数默认值为 0，表示只在 X 轴上移动。

```
transform: translate(50px, 100px); /* 元素在 X 轴上移动 50px，在 Y 轴上移动 100px */
```

`translateX()` 函数仅用于在水平方向（X轴）上移动元素。它接受一个参数，表示在 X 轴上的移动距离。

```
transform: translateX(30px); /* 元素在 X 轴上移动 30px */
```

`translateY()` 函数仅用于在垂直方向（Y轴）上移动元素。它接受一个参数，表示在 Y 轴上的移动距离。

```
transform: translateY(-50px); /* 元素在 Y 轴上向上移动 50px */
```

`translateZ()` 函数用于在三维空间的 Z 轴上移动元素。它接受一个参数，表示在 Z 轴上的移动距离。在二维渲染中，`translateZ() `通常不会产生可见的效果，但它可以影响 3D 变换和其他 3D CSS 属性。

```
transform: translateZ(20px); /* 元素在 Z 轴上移动 20px（在 2D 渲染中通常不可见） */
```

`translate3d()` 函数用于在三维空间中同时移动元素在 X、Y 和 Z 轴上的位置。它接受三个参数，分别表示在 X、Y、Z 轴上的移动距离。

```
transform: translate3d(10px, 20px, 30px); /* 元素在 X 轴上移动 10px，Y 轴上移动 20px，Z 轴上移动 30px */
```

注意：在进行 3D 变换时，`translateZ()` 和 `translate3d()` 会影响元素的层叠上下文，可能会改变元素的层叠顺序。

### skew()

### skewX()

### skewY()

`skew()`、`skewX()` 和 `skewY()` 是 CSS3 中的**倾斜函数**，用于在二维空间中对元素进行倾斜变换。这些函数可以作为 trans``form 属性的值来使用，以实现元素的倾斜效果。

`skew()` 函数允许在一个声明中同时指定 X 轴和 Y 轴的倾斜角度。它接受两个参数，第一个参数对应 X 轴的倾斜角度，第二个参数对应 Y 轴的倾斜角度。如果第二个参数未提供，则默认值为 0。

```
transform: skew(30deg, 20deg); /* 元素在 X 轴上倾斜 30 度，在 Y 轴上倾斜 20 度 */
```

`skewX()` 函数仅用于在水平方向（X 轴）上倾斜元素。它接受一个参数，表示元素在 X 轴上的倾斜角度。

```
transform: skewX(45deg); /* 元素在 X 轴上倾斜 45 度 */
```

`skewY()` 函数仅用于在垂直方向（Y 轴）上倾斜元素。它接受一个参数，表示元素在 Y 轴上的倾斜角度。

```
transform: skewY(-30deg); /* 元素在 Y 轴上倾斜 -30 度（即向上倾斜） */
```

### perspective()

`perspective()` 用于设置 3D 变换元素的透视效果。`perspective()` 函数必须与 `transform` 属性一起使用，以在元素上创建一种视觉上的深度感。

`perspective()` 函数接受一个长度值作为参数，这个值表示观察者与 `z=0` 平面之间的距离。这个距离定义了 3D 变换的透视效果。

```
.element {  
  transform: perspective(500px) rotateX(45deg);  
}
```

在这个例子中，`.element` 将被应用一个透视效果，观察者似乎位于距离 z=0 平面 500px 的地方，并且元素还绕 X 轴旋转了 45 度。

透视效果让元素在 z 轴上的变换看起来更加自然。离观察者较近的元素会显得更大，而离观察者较远的元素则会显得更小。这模仿了我们在现实世界中观察物体时的透视效果。

注意：在处理多个 3D 变换元素时，需要谨慎使用 `perspective()` 函数，因为每个元素上的透视效果是独立的。如果需要在一个容器内对多个元素应用透视效果，通常最佳做法是在容器元素上设置透视效果，而不是在每个子元素上分别设置。

### matrix()

### matrix3d()

`matrix()` 和 `matrix3d()` 是 CSS3 中 `transform` 属性的两个函数值，它们允许通过矩阵来定义元素的 2D 和 3D 变换。

`matrix()` 函数用于定义 2D 变换矩阵。它接受六个参数，这些参数表示一个 3x3 的变换矩阵（但实际上只用到了六个值，因为最后一行总是 [0, 0, 1]）。这个矩阵用于计算元素的最终位置。

**matrix(n1, n2, n3, n4, n5, n6)** 中的参数意义如下：

- n1 和 n4 控制缩放和倾斜（`scaleX` 和 `scaleY`）。
- n2 和 n3 控制倾斜（`skewX` 和 `skewY`）。
- n5 和 n6 控制平移（`translateX` 和 `translateY`）。

实际上，这个矩阵可以看作是以下形式的简写：

```
| n1 n2 0 |  
| n3 n4 0 |  
| n5 n6 1 |
```

`matrix3d()` 函数用于定义 3D 变换矩阵。它接受 16 个参数，这些参数表示一个 4x4 的变换矩阵。这个矩阵用于计算元素在三维空间中的最终位置和方向。

**matrix3d(n1, n2, n3, n4, n5, n6, n7, n8, n9, n10, n11, n12, n13, n14, n15, n16)** 中的参数意义较为复杂，但大致可以划分为以下几类：

- 前四个参数（n1-n4）通常与 X 轴相关。
- 接下来的四个参数（n5-n8）通常与 Y 轴相关。
- 再接下来的四个参数（n9-n12）通常与 Z 轴相关（包括透视效果）。
- 最后四个参数（n13-n16）控制平移（`translateX`, `translateY`, `translateZ`）和透视因子。

实际上，这个矩阵可以看作是以下形式的简写 

```
| n1  n2  n3  n4 |  
| n5  n6  n7  n8 |  
| n9 n10 n11 n12 |  
|n13 n14 n15 n16 |
```

其中，最后一列（n13, n14, n15, n16）主要用于平移变换和透视效果。

注意：

- `matrix()` 和 `matrix3d()` 允许更精细的控制元素变换，但它们也比较复杂，不容易直观地理解。
- 这两个函数通常用于高级的动画效果和复杂的布局情况。
- 当使用 `matrix()` 或 `matrix3d()` 时，需要确保你提供的参数能正确地形成一个有效的变换矩阵。
- 与其他 `transform` 函数值一样，`matrix()` 和 `matrix3d()` 的效果不会触发页面的重新布局，也不会影响其他元素（除了被变换的元素本身及其子元素）。

如果只是想进行简单的 2D 或 3D 变换，使用更高级的 `translate(),` `rotate()`, `scale()`, `skew()` 等函数可能更容易理解和实现。但是，对于复杂的变换或组合变换，`matrix()` 和 `matrix3d()` 提供了更强大的功能。

## 布局

### fit-content()

`fit-content()` 用于根据内容自动调整元素的尺寸。它接受一个参数，该参数可以是一个长度值（如 px、%、em 等）或百分比值，用于定义元素的最小或最大尺寸。当元素的内容超过指定尺寸时，`fit-content()` 函数可以确保元素的大小适应内容，但不会超过其最大限制。

在 CSS Grid 布局中，`fit-content()` 函数特别有用，因为它允许网格项根据内容动态调整大小。例如，可以使用 `fit-content()` 函数来设置网格项的宽度，以便它们根据内容自动伸缩，同时保持一定的最小或最大宽度。

```
.grid-container {  
  display: grid;  
  grid-template-columns: repeat(auto-fit, minmax(200px, fit-content(100%)));  
  grid-gap: 20px;  
}  
  
.grid-item {  
  background-color: #f2f2f2;  
  padding: 20px;  
}
```

在这个例子中，创建了一个名为 .`grid-container` 的网格容器，并使用 `grid-template-columns` 属性定义了网格列的宽度。通过使用 `repeat(auto-fit, minmax(200px, fit-content(100%)))`，告诉浏览器根据可用空间自动适应网格项的数量，并确保每个网格项的最小宽度为 200px，最大宽度为其内容的宽度（但不超过父容器的宽度）。这样，当内容较长时，网格项会自动扩展宽度以适应内容；而当内容较短时，网格项会收缩至最小宽度。

### minmax()

`minmax()`是 CSS Grid 布局中的一个函数，它用于定义一个长度范围，表示网格容器中的网格项可以使用的最小和最大尺寸。`minmax()` 函数接受两个参数：最小值和最大值。

在 `grid-template-columns` 或 `grid-template-rows` 属性中，可以使用 `minmax()` 来为网格项设置列或行的最小和最大尺寸。这允许网格项在需要时扩展或收缩，但不会超过指定的最大或最小尺寸。

`minmax()` 函数的语法如下：

```
minmax(min-length, max-length)
```

其中：

- `min-length` 是网格项可以使用的最小尺寸。
- `max-length` 是网格项可以使用的最大尺寸。

这两个参数都可以是任何有效的 CSS 长度单位，如 px、em、% 等。如果省略 `max-length`，则默认值为 `auto`，表示网格项可以扩展到占据所有可用空间。

```
.grid-container {  
  display: grid;  
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));  
  grid-gap: 10px;  
}  
  
.grid-item {  
  background-color: #f2f2f2;  
  padding: 20px;  
}
```

在这个例子中，`.grid-container` 是一个网格容器，使用 `grid-template-columns` 属性来定义网格的列。`repeat(auto-fill, minmax(100px, 1fr))` 表示浏览器应该尝试填充尽可能多的列，每列的最小宽度为 100px，最大宽度为 1fr（即网格容器可用空间的一个等份）。如果容器宽度允许，所有列都将尝试扩展到相同的宽度（即 1fr），但如果容器宽度有限，列宽将不会小于 100px。

使用 `minmax()`函数可以创建更加灵活和响应式的网格布局，其中网格项可以根据可用空间自动调整大小，同时保持一定的尺寸限制。

### repeat()

`repeat()` 是 CSS Grid 布局和 Flexbox 布局中的一个函数，用于重复一个或多个值指定的次数。在 Grid 布局中，它通常与 `grid-template-columns` 和 `grid-template-rows` 属性一起使用，以定义重复的网格轨道。在 Flexbox 布局中，它可以与 flex 属性一起使用，以定义重复的 flex 项。

在 Grid 布局中，`repeat()` 函数的语法如下：

```
repeat(count, value)
```

- `count` 是一个正整数，表示要重复的值的次数。
- `value` 是要重复的值，可以是一个长度、百分比、fr 单位等。

`repeat()` 函数会根据指定的次数重复给定的值，从而简化代码并创建更加灵活的网格布局。

```
.grid-container {  
  display: grid;  
  grid-template-columns: repeat(3, 100px);  
  grid-gap: 10px;  
}
```

在这个例子中，`.grid-container` 是一个网格容器，使用 `grid-template-column`s 属性定义了网格的列。`repeat(3, 100px)` 表示我们要创建三列，每列的宽度都是 100px。因此，总宽度将是 3 * 100px = 300px。

此外，`repeat()` 函数还可以与 `auto-fill` 和 `auto-fit` 关键字一起使用，以根据可用空间自动调整列或行的数量。例如：

```
.grid-container {  
  display: grid;  
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));  
  grid-gap: 10px;  
}
```

在这个例子中，`repeat(auto-fill, minmax(100px, 1fr))` 表示浏览器应该尝试填充尽可能多的列，每列的最小宽度为 100px，最大宽度为 1fr。`auto-fill` 关键字告诉浏览器根据容器宽度自动计算需要多少列。

通过使用 `repeat()` 函数，可以更加轻松地创建复杂且灵活的网格布局，而无需手动指定每个网格轨道的尺寸。

## 动画

### cubic-bezier()

`cubic-bezier()` 用于定义自定义的缓动曲线，让动画或过渡效果可以具有非线性的速度变化。这个函数基于贝塞尔曲线来工作，允许通过指定四个点（两个端点和两个控制点）来定义动画的速度曲线。

`cubic-bezier()` 函数的语法如下：

 

```
cubic-bezier(n,n,n,n)
```

这里的 n 是 0 到 1 之间的数值，它们分别代表两个控制点的 x 和 y 坐标。端点始终是 (0,0) 和 (1,1)，所以不需要指定。

使用 `cubic-bezier() `函数时，需要提供这四个数值作为参数，它们定义了贝塞尔曲线的形状。这些数值会影响动画的速度和加速度，从而创造出各种各样的动画效果。

下面是一些常用的预定义`cubic-bezier()` 值，以及它们所代表的效果：

- `cubic-bezier(0.25, 0.1, 0.25, 1.0)`：慢到快然后慢的缓动（与 `ease-in-out` 类似）。
- `cubic-bezier(0.42, 0, 1, 1)`：先慢后快的缓动（与 `ease-out` 类似）。
- `cubic-bezier(0, 0, 0.58, 1)`：先快后慢的缓动（与 `ease-in` 类似）。
- `cubic-bezier(0, 0, 0, 1)`：线性缓动（与 linear 类似）。

要在 CSS 中使用 `cubic-bezier()` 函数，可以将它应用于 `transition-timing-function` 或 `animation-timing-function` 属性。例如：

```
/* 使用自定义的 cubic-bezier 缓动函数 */  
div {  
  transition: width 2s cubic-bezier(0.25, 0.1, 0.25, 1.0);  
}  
  
/* 或者在关键帧动画中使用 */  
@keyframes example {  
  0% { background-color: red; }  
  100% { background-color: blue; }  
}  
  
div {  
  animation: example 3s cubic-bezier(0.42, 0, 1, 1);  
}
```

在这个例子中，`div` 元素的宽度过渡和背景颜色动画都将使用自定义的 `cubic-bezier()` 缓动函数。可以通过调整四个数值来创建出适合动画需求的缓动曲线。

### steps()

`steps()` 用于在动画或过渡中创建一种阶跃式（步进式）的变化效果，而不是平滑的过渡。当使用 `steps()` 函数时，属性会在指定的段数内突然改变，而不是在两个状态之间平滑地过渡。

`steps()` 函数的语法如下：

```
steps(number, [start | end])
```

- `number` 是一个正整数，表示动画或过渡应该被分割成多少个阶跃。
- `start` 或 `end` 是一个可选参数，用于指定阶跃变化发生的时刻。如果省略或设置为 end，则每个阶跃会在每个间隔的结束时刻发生。如果设置为 `start`，则每个阶跃会在每个间隔的开始时刻发生。

例如，如果希望一个元素在 2 秒内从其原始位置跳到 100px 的位置，并且希望这个跳跃发生在 1 秒和 2 秒的时刻，可以使用 steps(2, end)。

```
div {  
  width: 100px;  
  height: 100px;  
  background-color: red;  
  animation: jump 2s steps(2, end) infinite;  
}  
  
@keyframes jump {  
  0% { transform: translateX(0); }  
  100% { transform: translateX(100px); }  
}
```

在这个例子中，`div` 元素会无限次地在 2 秒内从其原始位置跳跃到 100px 的位置。由于使用了 `steps(2, end)`，跳跃会在动画的第 1 秒和第 2 秒结束时发生。

`steps()` 函数通常用于创建一种机械或数字化的动画效果，因为它允许属性在特定的时间点突然改变，而不是平滑地过渡。这种效果在模拟数字时钟、步进式进度条或某些类型的用户界面动画时特别有用。