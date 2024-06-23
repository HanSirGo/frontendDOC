## CSS 选择器

### 通用选择器

通用选择器使用 `*` 来选择全部元素。

![1711876299442](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876299442.png)

### 元素选择器

同于选择指定 HTML 元素。

![1711876314575](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876314575.png)

### 类选择器

选择具有指定类名的所有元素。

![1711876330367](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876330367.png)

### ID选择器

选择具有指定ID的元素。

![1711876348675](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876348675.png)

### 组合选择器

连接两个或多个类名或 ID，以选择具有所有指定类名/ID的元素。

![1711876367011](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876367011.png)

### 多重选择器

使用逗号将多个选择器声明分开，这样可以很容易地将相同的样式应用于多个选择器声明。

![1711876381604](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876381604.png)

### 后代选择器

后代选择器表示选择某个元素的所有后代元素，即位于该元素内部的所有子孙元素。在使用后代选择器时，在两个选择器之间加上一个空格，表示前一个选择器所选中的元素中包含的后代元素。

![1711876398005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876398005.png)

### 相邻选择器

相邻选择器用于选择紧接在另一个元素后面的直接相邻兄弟元素的选择器，使用加号（+）作为组合符号，将两个选择器连接起来。它选择的是位于第一个选择器后紧邻的同级元素。

![1711876413428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876413428.png)

### 子选择器

子选择器用于选择某个元素的直接子元素，使用大于号（>）作为组合符号，将两个选择器连接起来。它选择的是父级元素下的直接子元素，即元素树结构中的一级关系。

![1711876430291](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876430291.png)

### 通用兄弟选择器

通用兄弟选择器使用波浪号（即通用兄弟组合符）来选择在第一个选择器之前的所有元素，而不要求它们是第一个选择器的直接兄弟元素。

![1711876444511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876444511.png)

### 猫头鹰选择器

猫头鹰选择器用于选择所有具有前置兄弟元素的元素，用来在一个容器内的元素之间添加间距，但不包括第一个元素，因为第一个元素没有前置兄弟元素。

![1711876461802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876461802.png)

### 属性选择器

属性选择器用于选择具有特定属性的元素。

- 使用`[attribute]`来选择所有具有 `attribute` 属性的元素：

![1711876483000](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876483000.png)

- 使用 `[attribute=value]` 来选择具有指定属性及属性值的元素：

![1711876532605](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876532605.png)

- 使用 `[attribute~=value]` 来选择具有指定属性，并且该属性的多个值中**包含给定值**的元素：

![1711876551792](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876551792.png)

- 使用 `[attribute*=value]` 来选择具有指定属性，并且该属性的值中**包含特定部分值**的元素。

![1711876567539](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876567539.png)

- 使用 `[attribute^=value`] 来选择具有指定属性，并且该属性的值**以给定值开头**的元素。

![1711876584406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876584406.png)

- 使用 `[attribute$=value]` 来选择具有指定属性，并且该属性的值**以给定值结尾**的元素。

![1711876600463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876600463.png)

## 伪类

### 链接伪类选择器

这 4 个伪类用于选择链接处于不同状态的元素。它们最常与链接一起使用，但是 `:active` 也适用于按钮，而 `:hover` 可以用于各种元素：

- `:link`：选取未被访问过的链接，用于为用户尚未点击的超链接设置样式。
- `:visited`：选取已经被用户访问过的链接，用于为用户之前点击过的超链接设置样式。
- `:hover`：当用户指针（例如鼠标光标）悬停在某些元素（通常是链接）上时，选取这些元素。
- `:active`：在元素（通常是链接或按钮）被激活时，例如用户点击它们的瞬间，选取这些元素。

这 4 个伪类可用于提高用户交互性，例如增加链接的可见度，使其更加显著突出，从而改善用户体验。

![1711876657226](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876657226.png)

### :focus 伪类

`:focus` 伪类用于选择当前获得焦点的元素。当用户与网页上的表单元素进行交互时，可以通过点击或键盘导航，使某个特定的输入框处于焦点状态。这意味着用户的输入将直接应用到该输入框上。

![1711876674389](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876674389.png)

### :checked 伪类

`:checked` 伪类用于选择当前被选中或选择的单选按钮、复选框或 select 元素的选项。

![1711876691391](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876691391.png)

### :disabled 伪类

`:disabled` 伪类用于匹配被禁用的表单元素，例如按钮或文本输入框。

![1711876708868](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876708868.png)

### :enabled 伪类

`:enabled` 伪类用于匹配可以交互和接收输入的表单元素。

![1711876727410](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876727410.png)

### :valid 伪类

`:valid` 伪类用于选择具有与其属性（如 `pattern`、`type` 等）所指定要求相匹配的内容的输入元素。

当 `input` 元素的内容符合其属性所指定的要求时，可以使用 `:valid` 伪类选择它们。

![1711876742515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876742515.png)

### :invalid 伪类

`:invalid` 伪类用于选择具有内容不符合要求的输入元素。

当`input`元素的内容不符合其要求时，可以使用 `:invalid` 伪类来选择它们。

![1711876755681](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876755681.png)

### :required 伪类

`:required` 伪类用于选择具有 `required` 属性的输入元素，该属性表示在提交表单之前必须填写它们。

当 `input` 元素具有 `required` 属性时，可以使用 `:required` 伪类选择它们。

![1711876770222](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876770222.png)

### :optional 伪类

`:optional` 伪类用于选择没有 `required` 属性的输入元素，这意味着它们不是必填项。

当`input` 元素没有 required 属性时，可以使用 `:optional` 伪类选择它们。

![1711876788366](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876788366.png)

### :first-child 伪类

`:first-child` 伪类用于选择父元素中的第一个子元素。

![1711876802499](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876802499.png)

### :last-child 伪类

`:last-child` 伪类用于选择父元素中的最后一个子元素。

![1711876815574](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876815574.png)

### :nth-child 伪类

`:nth-child` 伪类根据元素在父元素中的位置进行选择，允许进行各种选择。`:nth-child` 还可以自定义模式选择元素：

- `:nth-child(odd)` 或 `:nth-child(2n+1)` 选择每个奇数位置的子元素
- `:nth-child(even)` 或 `:nth-child(2n)` 选择每个偶数位置的子元素

公式中的 `n` 就像一个计数器，可以按重复周期选择元素。例如，`:nth-child(3n)` 将选择每三个元素。

![1711876833694](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876833694.png)

### :nth-last-child 伪类

`:nth-last-child` 伪类与 `:nth-child` 类似，但是从最后一个子元素向后计数。

![1711876848939](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876848939.png)

### :only-child 伪类

当需要选择在其父级元素中唯一的一个子元素时，可以使用 `:only-child` 伪类。

![1711876865778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876865778.png)

### :first-of-type 伪类

`:first-of-type` 伪类选择在其父元素中的特定类型的元素中的第一个元素。

![1711876882455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876882455.png)

### :last-of-type 伪类

`:last-of-type` 伪类选择在其父元素中的特定类型的元素中的最后一个元素。

![1711876898850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876898850.png)

### :nth-of-type 伪类

当需要根据元素在兄弟元素中的类型和位置选择元素时，可以使用 `:nth-of-type` 伪类。

![1711876916402](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876916402.png)

### :nth-last-of-type 伪类

`:`当需要根据元素在兄弟元素中的类型和位置选择元素，并且从末尾开始计数时，可以使用 `:nth-last-of-type` 伪类。

![1711876933227](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876933227.png)

### :only-of-type 伪类

当需要选择在其兄弟元素中为特定类型的元素仅有一个的元素时，可以使用 `:only-of-type` 伪类。

![1711876949494](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876949494.png)

### :target 伪类

`:target` 用于选择具有与 URL 片段匹配的 ID 属性的元素。URL 片段是指 URL 中的 # 符号后面的部分。当从页面中的链接点击跳转到带有特定片段的 URL 时，`:target` 伪类将会被应用于与片段匹配的对应元素上。这样可以利用 `:target` 来为被直接链接到的页面部分设置不同的样式，从而提供视觉上的反馈和突出显示。

![1711876964838](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876964838.png)

### :not() 伪类

使用 `:not()` 伪类可以选择不符合指定选择器或条件的元素。这在需要排除某些特定元素时非常有用。

![1711876981670](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876981670.png)

### has() 伪类

使用 `:has()` 伪类可以选择包含某个特定元素或选择器的父级元素，并为其应用样式。

![1711876997046](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711876997046.png)

**基本使用**

举一个场景例子，我们看以下代码，一个容器中，图片是可以显隐的，我想要实现：

- 图片显示时，字体大小为 12px
- 图片隐藏时，字体大小为 20px

```html
<div class="container">
    哈哈哈哈哈
    <img class="test-img" v-if="showImg"></img>
</div>
```

如果按照以前的做法，就是使用 动态class 的方式去玩完成这个功能，但是现在有 `:has()`可以通过 css 的方式去完成这件事~

```css
.container {
    font-size: 20px;
}
.container:has(img) {
    font-size: 12px;
}

或者
.container:has(.test-img) {
    font-size: 12px;
}
```

**组合使用**

现在又有两个场景

- 判断容器有没有**子img**，有的话字体设置为 12px（上面的例子是后代选择器，不是子选择器）
- 判断容器有没有一个小相邻的img，有的话设置字体颜色为 red

我们可以这么去实现：

```css
.container:has(>img) {
    font-size: 12px;
}

.container:has(+img) {
    color: red;
}
```

再来一个场景，当我 hover 到 子img 上时，我想要让 container 的字体变粗，可以这么去使用~

```css
.container:has(>img:hover) {
    color: red;
}
```

**兼容性**

还是有一些浏览器不支持

![1719136599347](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719136599347.png)

### :where

**基本使用**

**:where()**  CSS 伪类函数接受选择器列表作为它的参数，将会选择所有能被该选择器列表中任何一条规则选中的元素。

以下代码，文本都会变成 yellow 颜色

```css
:where(div p) span {
    color: yellow;
}

<div class="test-div">
    <span>哈哈</span>
</div>
<p class="test-p">
    <span>哈哈</span>
</p>
```

**使用场景**

其实 `:where()` 的功能本来就有，只不过有了它之后，实现起这些功能来就更加方便快捷~接下来就来讲讲它的组合/叠加功能

我们来看下面的这段 css 代码

```css
div a:hover,
li a:hover,
.cla a:hover,
.aa .bb a:hover,
[class^='bold'] a:hover{
  color: yellow;
}
```

我们可以使用 `:where()`来简化这个写法，使用它找出 div li .cla 这三种选择器，选择器可以是标签，也可以是类名，也可以是选择器表达式

```css
:where(div, li, .cla, .a .b, [class^='bold']) a:hover {
    color: yellow;
}
```

再来看看使用 `:where()` 的组合，完成一些功能，我们看以下的代码

```css
.dark-theme button,
.dark-theme a,
.light-theme button,
.light-theme a{
 color: pink;
}
```

我们完全可以使用 `:where()` 简化这个写法

```css
:where(.dark-theme, light-theme) :where(button, a) {
    color: pink;
}
```

**优先级**

`:where()`的优先级是 0，我们可以看下面代码

```css
.test {
    color: yellow;
}
:where(.test) {
    color: pink
}
```

最后字体颜色是 yellow

**兼容性**

全绿~

![1719136424486](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719136424486.png)

### :is

`:is()`跟`:where()`可以说一模一样，区别就是 `:is()`的优先级不是0，而是由传入的选择器来决定的，拿刚刚的代码来举个例子

```css
div {
    color: yellow;
}
:where(.test) {
    color: pink
}

<div class="test">哈哈</div>
```

这要是 `:where()`，那么字体颜色会是 yellow，因为它的优先级是 0

但是如果是 `:is()`的话，字体颜色会是 pink，因为 类选择器 优先级比 标签选择器 优先级高~

```css
:is(.test) {
    color: pink
}
div {
    color: yellow;
}

<div class="test">哈哈</div>
```

**兼容性**

全绿~

![1719136503112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719136503112.png)

### 其他伪类

除了上面提到的伪类，CSS中还包含以下伪类：

- `:root`：选择文档中最高级别的父元素，通常是HTML文档中的 `<html>` 元素。可用于定义对所有页面元素可用的CSS变量。
- `:is()`：匹配可以是多个选择器之一的元素，使得长的选择器列表更加简短和易读。例如，`:is(h1, h2, h3)` 会匹配这三个标题元素之一。
- `:where()`：与 `:is()` 类似，但允许根据条件选择元素，而不影响选择器的特异性。
- `:default`：匹配设置为默认选择状态的用户界面元素（如单选按钮或复选框）。
- `:empty`：选择没有子元素（包括文本节点）的元素。
- `:fullscreen`：选择当前以全屏模式显示的元素。
- `:in-range`：匹配值在指定范围内的表单元素（使用 `min` 和 `max` 属性指定范围）。
- `:out-of-range`：匹配值在指定范围之外的表单元素。
- `:indeterminate`：选择状态不确定的表单元素，例如既未选中也未取消选中的复选框（在树状结构中经常见到）。
- `:read-only`：匹配由于 readonly 属性而不可编辑的表单元素。
- `:read-write`：选择可由用户编辑的表单元素，意味着它们不是只读的。
- `:lang()`：根据元素的语言属性进行匹配。例如，`:lang(en)` 选择使用英语定义的元素。

## 伪元素

### ::before 伪元素

`::before` 伪元素用于在元素内容之前插入内容。它可以用于添加装饰性内容、图标或其他不需要出现在实际 DOM 中的元素。

![1711877027220](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877027220.png)

### ::after 伪元素

`::after` 伪元素与 `::before` 伪元素类似，用于在元素的内容之后插入内容。

![1711877043380](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877043380.png)

### ::first-letter 伪元素

`::first-letter` 伪元素用于选择并修改块级元素的第一个字母，从而应用特定的样式。这个伪元素只能选择每个块级元素的第一个字母，并且仅在有文本内容的时候生效。

![1711877058804](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877058804.png)

### ::first-line 伪元素

`::first-line` 伪元素用于选择并修改块级元素的第一行，从而应用特定的样式。这个伪元素只能选择每个块级元素的第一行，并且仅在有文本内容的时候生效。

![1711877071806](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877071806.png)

### ::placeholder 伪元素

`::placeholder` 伪元素用于选择并修改表单字段的占位符文本，从而应用特定的样式。占位符文本是在用户未输入任何内容时显示的默认文本。

![1711877087023](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877087023.png)

### ::selection 伪元素

`::selection` 伪元素用于选择并修改用户所选文本的样式。可以应用于包含文本内容的任何元素，如段落、标题、按钮等。

![1711877099833](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877099833.png)

### ::marker 伪元素

`::marker` 伪元素用于为列表项中的标记符设置样式，这些标记符通常包含无序列表的项目符号或有序列表的数字/字母。

![1711877111976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711877111976.png)

### 其他伪元素

除了上述伪元素之外，CSS 还提供了以下伪元素：

- `::file-selector-button`：用于选择文件的按钮元素的伪元素。它可以用来样式化文件上传控件中的选择按钮。
- `::cue`：用于样式化媒体元素（如音频或视频）中的字幕或注释的伪元素。可以用来设置字幕的样式，比如字体、颜色等。
- `::part()`：用于自定义 Web 组件的内部部件的伪元素。它可以针对组件的具体部分应用样式，并通过组件的 shadow DOM 提供的 Custom Elements API 进行访问。
- `::slotted()`：用于样式化插槽内容的伪元素。插槽是一种在 Web 组件中用于插入内容的机制，`::slotted()` 可以应用样式到被插入的内容，以实现更精确的样式控制。

###    

