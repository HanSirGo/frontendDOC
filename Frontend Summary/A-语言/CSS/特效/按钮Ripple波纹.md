# CSS3实现按钮Ripple波纹

在现代的Web设计中，用户交互效果对于提升用户体验至关重要。按钮波纹效果是一种常见的交互效果，它为用户的点击行为增加了视觉反馈，使界面更加生动和直观。本文将介绍按钮波纹(Ripple)效果的原理和实现方法，并通过一个简单的示例来演示如何使用HTML、CSS和JavaScript来实现这一效果。按钮波纹效果是一种在按钮被点击时产生的视觉反馈效果，它通过在按钮点击位置产生波纹的扩散动画来模拟水波纹的效果，从而提升了用户对于点击行为的感知和反馈。

![1714282512892](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282512892.png)

**实现原理**

1. **创建按钮元素：**使用HTML创建一个按钮元素，作为用户交互的目标对象。
2. **添加波纹效果样式：**使用CSS为按钮元素添加样式，包括按钮的基本样式和波纹效果的样式。
3. **监听点击事件：**使用JavaScript监听按钮的点击事件，获取点击位置的坐标。
4. **创建波纹元素：**在按钮点击位置创建一个波纹元素，用于展示波纹扩散效果。
5. **执行波纹动画：**使用CSS动画或者JavaScript实现波纹的扩散动画效果。
6. **结束动画：**在动画结束后，移除波纹元素，完成一次点击效果的展示。

**思维导图**

要实现按钮波纹效果，依赖于绝对定位，波纹元素在父元素按钮内，其设置相对定位，波纹设置绝对定位，当用户点击按钮区域任意位置时，创建波纹元素span，捕获父元素偏移坐标值 offsetX 和 offsetY , 并赋值给span的left和top值，添加基本类名 ripple-effect，类名下增加波纹span元素放大功能，利用animation的线性函数来执行动画，时间0.6s，可以自行调整，然后添加到按钮元素内部，动画执行完毕需要移除波纹span元素。当然，用户连续点击多次，会创建多个span元素，需要用settimeout 函数来清理。这样一套思维下来，就出现了这个结构图。

![1714282529764](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282529764.png)

图的释义：示例图原点 x 和 y 就是点击按钮元素后捕获的offsetx和offsety坐标，值得注意的是，css中定位都是在元素的左上角，需要把波纹span元素的left和top减半，也就是除以2，甚至更大的数，以此调整波纹大小。别忘了设置 ripple-effect的 transform-origin: center center，同时设置圆角50%; 保障动画执行是以span元素中心进行的。

span中心坐标转换，用span元素的宽高的一半实现中心点:

![1714282540064](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282540064.png)

**完整源代码**

下面是一个简单的示例代码，演示了如何使用HTML、CSS和JavaScript来实现按钮波纹效果，js中包含了两种计算波纹span元素大小和坐标的方案，可以切换查看效果哦。

```html

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" 
content="width=device-width, 
initial-scale=1.0" />
<title>按钮波纹效果</title>
<style>
body {
    display: grid;
    place-items: center;
    min-height: 100vh;
    overflow: hidden;
    background-color: #001C30;
}
.ripple-btn {
    position: relative;
    overflow: hidden;
    border: none;
    /* 按钮背景颜色 */
    background-color: #007bff; 
    color: #fff;
    padding: 30px 40px;
    font-size: 16px;
    cursor: pointer;
    outline: none;
    border-radius: 4px;
    box-shadow: none;
}

.ripple-effect {
    position: absolute;
    border-radius: 50%;
    /* 波纹颜色 */
    background-color: rgba(255, 255, 255, 0.4);
    pointer-events: none;
    animation: ripple-animation 0.6s linear forwards;
    transform-origin: center center;
    box-shadow: none;
}

@keyframes ripple-animation {
    from {
        transform: scale(1);
        opacity: 1;
    }
    to {
        transform: scale(10);
        opacity: 0;
    }
}
</style>
</head>
<body>
    <button class="ripple-btn" id="botton">
        Click Me
    </button>
    <script>
        const bottons = document.querySelector('#botton')
        const effect = document.querySelector('.ripple-effect')
        const { width, height } = 
        bottons.getBoundingClientRect()
        bottons.onclick = function (evt) {
        const { offsetX, offsetY } = evt
        const ripple = document.createElement("span");
        ripple.classList.add("ripple-effect");
        
        // 方案1：计算波纹span大小坐标
        ripple.style.height = '30px'
        ripple.style.width = '30px'
        ripple.style.left = `${offsetX - 15}px`
        ripple.style.top = `${offsetY - 15}px`

        // 方案2：计算波纹span大小坐标
        // const rippleW = width/4
        // ripple.style.height = `${rippleW}px`
        // ripple.style.width = `${rippleW}px`
        // ripple.style.left = `${offsetX - rippleW/2}px`
        // ripple.style.top = `${offsetY - rippleW/2}px`
        bottons.appendChild(ripple);
        setTimeout(() => {
            bottons.removeChild(ripple);
        }, 600);
    }
</script>
</body>
</html>
```

不出意外的话，以上代码跑起来就是文章开头的样子了。gif图片有点卡，真实效果比这图更有意思。如果有更好的实现方法，欢迎大家评论区留言讨论，共同进步，掌握css 技巧。

