> ## **IFC - inline format context （内联格式化上下文）**
>
> **IFC特性：**
>
> 1. 水平方向根据`direction`依次布局。
> 2. 不会在元素前后换行。
> 3. 受`white-space`属性的影响。
> 4. `margin/padding` 在竖直方向无效，水平方向有效的。
> 5. `white/height` 对非替换行内元素无效，宽度由元素内容决定。（替换元素在下面解释）
> 6. 非替换行内元素的行框高由`line-height`决定而替换行内元素的行框高则是由`height`，`padding`，`border`，`margin`决定
> 7. 浮动或者绝对定位会转化为block
> 8. `vertical-align`属性生效
>
> **触发条件：**
>
> 1. `display`为`inline`或者行内元素

