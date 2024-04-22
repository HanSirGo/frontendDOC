# 应用CSS混合模式，构建特殊的视觉效果

CSS混合模式（blend mode）最早是在CSS3规范中引入的，该规范于1999年首次提出，但混合模式的具体实现和广泛应用则较晚。随着现代浏览器对CSS3规范的全面支持，混合模式才逐渐成为Web设计中常见的一部分。从功能角度看，混合模式允许开发者控制元素与其背后的元素之间的混合效果。

当然，CSS混合模式主要用于通过将两个或多个图层的颜色混合在一起来创建新的颜色效果的技术。它可以应用于背景图像、文字和其他元素，以创建各种视觉效果。这为网页设计师提供了更多的创意和表现空间，使得设计可以更加丰富多彩。

**混合属性**

- `background-blend-mode`：用于混合元素背景图案、渐变和颜色；
- `mix-blend-mode`：用于元素与元素之间的混合；
- `isolation`：用户阻止某些元素在`mix-blend-mode` 使用时被混合。



**兼容性**

background-blend-mode兼容性

![1713693232906](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693232906.png)

 mix-blend-mode兼容性

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693243303.png)

结论：CSS混合模式属性background-blend-mode和mix-blend-mode在PC端兼容性优先于移动端。意味着可以放心在PC端生产环境使用混合模式，相反移动端暂时不推荐在生产模式使用，避免带来不好的用户体验。

**混合模式值可选项**

这里的值可以用于background-blend-mode和mix-blend-mode属性值。

以下列举常见混合模式选项，更多选项前往MDN官方网站查询：【https://developer.mozilla.org/zh-CN/docs/Web/CSS/blend-mode】

\1. 正常模式（normal）：这是默认的混合模式，它将两个图层的颜色直接覆盖在一起；

\2. 叠加模式（overlay）：这种模式将两个图层的颜色叠加在一起，产生一种更加饱和和对比度更高的效果；

\3. 滤色模式（screen）：这种模式将两个图层的颜色取反，并相互乘积，产生一种较亮的效果；

\4. 柔光模式（soft-light）：这种模式通过增加或减少底层图层的亮度来调整混合结果，产生一种柔和的效果；

\5. 差值模式（difference）：这种模式将两个图层的颜色进行差值计算，并产生一种颜色反转的效果；

\6. 颜色加深模式（color-dodge）：这种模式通过增加底层图层的亮度来减少混合结果的亮度，产生一种颜色加深的效果；

7、正片叠底模式（multiply）最终颜色为顶层颜色与底层颜色相乘的结果。如果叠加黑色层，则最终层必为黑色层，叠加白色层不会造成变化。其效果类似于在透明薄膜上重叠印刷的两个图像。

**文字视频背景特效**

最佳效果请把源代码放置浏览器进行预览。下面放置完整css代码和HTML代码，视频文件自行添加，这里不做提供。

![1713693277378](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693277378.png)

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693292661.png)

**mix-blend-mode:screen的效果图**

![1713693307164](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693307164.png)

**mix-blend-mode:screen的效果图**

把mix-blend-mode的值改为difference，本来是gif图，编辑时无法上传，所以贴图了。

![1713693319185](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693319185.png)

**背景混合模式**

background-blend-mode CSS 属性定义该元素的背景图片，以及背景色如何混合，利用这个属性可以做一些漂亮的背景效果。

**官方示例效果**

![1713693332899](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693332899.png)

![1713693340832](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693340832.png)

**栅格背景**

贴图代码是完整的HTML和CSS代码，修改属性查询效果。

HTML。

![1713693351812](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693351812.png)

grid.css样式表。

![1713693362898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693362898.png)

**background-blend-mode：screen效果图**

![1713693371105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693371105.png)

**background-blend-mode：multiply效果图**

![1713693379685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693379685.png)

**网格效果**

调整css代码如下，红色部分，其余不变。

![1713693388596](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693388596.png)

**网格效果截图**

![1713693396359](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693396359.png)

**推荐优质资源**

**bennettfeely**：https://bennettfeely.com/gradients/

点击CSS即可查看完整CSS代码。

![1713693406507](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693406507.png)

**CSS3 Patterns Gallery**：https://projects.verou.me/css3patterns/

背景设计资源库。

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713693416809.png)