## link和@import加载CSS文件的区别

### CSS @import

- `@import` 用于从其他样式表导入样式规则
- `@import` 必须在CSS文档的头部，但可以在`@charset`规则后面
- `@import` 不是一个嵌套语句，不能在条件组的规则中使用
- `@import` 支持媒介查询

### link和@import的区别

- 差别1：老祖宗的差别。link属于XHTML标签，而@import完全是CSS提供的一种方式。

> link标签除了可以加载CSS外，还可以做很多其它的事情，比如定义RSS，定义rel连接属性等，@import就只能加载CSS了。

- 差别2：加载顺序的差别。

> 当在外联css文件A中使用@import引用另一个css文件B时，只有等css文件 A下载完解析时，浏览器才知道还有css文件B需要下载，此时css文件A和css文件B是串行加载的。 如果css文件A和css文件B都是通过link加载，则是并行的。

- 差别3：兼容性的差别。

> 由于@import是CSS2.1提出的所以老的浏览器不支持，@import只有在IE5以上的才能识别，而link标签无此问题。

- 差别4：使用dom控制样式时的差别。

> 我们可以通过js创建一个link标签，然后加载一个css文件。从而改变某一个dom的样式。而js是无法控制@import的。

```js
let link = document.createElement('link')
link.href = './link.css'
link.rel = 'stylesheet'
document.body.append(link)
```

### 不建议使用@import（在webpack、vite项目中忽略）

- 原因一：使用@import引入的CSS会影响浏览器的并行下载

> 使用@import引用的CSS文件只有在引入它的那个CSS文件被下载、解析之后，浏览器才会知道还有另外一个CSS需要下载，这时才会去下载，然后下载后开始解析、构建render tree等一系列操作。这就导致了浏览器无法并行下载所需的样式文件。

#### 案例A：在css文件中@import另一个css文件， 串行加载

link.css

```css
@import url(./@import.css);
.box {
    width: 200px;
    height: 300px;
    background-color: red;
}
```

@import.css

```css
.box {
    width: 100px;
    height: 100px;
    background-color: green;
}
```

![1713684992048](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713684992048.png)

#### 案例B：2个css文件都使用link加载，并行加载

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link href="./@import.css" rel="stylesheet" />
    <link  href="./link.css" rel="stylesheet" />
</head>
<body>
    <div class="box"></div>
</body>
</html>
```

![1713685018143](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713685018143.png)

- 原因2: @import 是 CSS2.1 才出现的概念，所以如果浏览器版本较低，无法正确导入外部样式文件。

