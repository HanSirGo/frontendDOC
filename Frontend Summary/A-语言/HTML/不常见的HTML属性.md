## **inputmode**

inputmode 属性可以定义 input 或者 textarea 元素弹出键盘的类型。这对于移动端开发还是很实用的,比如

```js
//数字键盘numeric
<input type="text" inputmode="numeric" />
//手机号键盘tel
<input type="text" inputmode="tel" />
//邮箱 email
<input type="text" inputmode="email" />
//链接 url
<input type="text" inputmode="url" />
//邮箱 email
<input type="text" inputmode="email" />
//搜索 search (键盘会出现Go/确认/Return)
<input type="text" inputmode="search" />
//等等
```

## **poster**

poster 属性一般用于 video 标签,用于设置视频的预览图,如果不设置则取视频第一帧展示

```html
<video src="xxx.mp4" poster="preview.png"></video>
```

## **multiple**

multiple 通常用于 input 标签文件选择时候的多选功能,如

```html
<input type="file" id="files" name="files" multiple />
```

除此之外,还可以用于 select 标签多选,如

```html
<label for="cars">请选择一个汽车品牌：</label>

<select name="cars" id="cars" multiple>
  <option value="audi">奥迪</option>
  <option value="byd">比亚迪</option>
  <option value="geely">吉利</option>
  <option value="volvo">沃尔沃</option>
</select>
```

## **accesskey**

accesskey 用于规定快捷键，用于激活/聚焦元素,比如下面一个超链接

```html
<a href="https://juejin.cn/" accesskey="j">JueJin</a>
```

如果你的浏览器是谷歌的话,直接按下 alt+j 就会跳转到掘金链接。当然,激活 accesskey 的操作取决于浏览器及其平台,如下图所示

![1712480382998](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480382998.png)

## **tabindex**

tabindex 可以指示其元素是否可以聚焦，以及它是否/在何处参与顺序键盘导航（通常使用 Tab 键，因此得名）。如下面代码

```html
<input tabindex="2" type="text" />
<div tabindex="0">可以聚焦的div</div>
<input tabindex="1" type="text" />
```

当按 tab 键的时候它们会按照 tabindex 顺序聚焦

## **download**

download 一般用于 a 标签中,可以让浏览器直接进行下载,而不是展示链接内容,比如:

```html
<a href="./a.jpg" download>下载</a>
```

点击就会下载`a.jpg`,如果你想改变下载文件名,可以这样写

```html
<a href="./a.jpg" download="b.jpg">下载</a>
```

这样下载的文件名就改变了

注意: download 如果下载跨域资源的话会失效

## **dir**

dir 是一个指定元素中文本排版方向属性,它的取值如下

- ltr,指从左到右,用于那种从左向右书写的语言（比如英语）
- rtl,指从右到左,用于那种从右向左书写的语言（比如阿拉伯语）
- auto,根据文字自动判断书写方向

如下用法

```html
<p dir="ltr">从左往右的文本方向</p>
<p dir="rtl">从左往右的文本方向</p>
<p dir="auto"> اتجاه النص من اليسار إلى اليمين </p>
<p dir="auto"> 从左往右的文本方向</p>
```

![1712480462522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480462522.png)

## **translate**

translate 属性用于指定元素是否可被浏览器翻译,比如你想阻止浏览器翻译你的品牌名字可以设置为 no

```html
<p translate="no">Chicken Is Beautiful</p>
```

## **contenteditable**

contenteditable 可以指定元素可以进行编辑,如

```html
<div contenteditable="true">此元素可编辑</div>
```

此时这个元素就能编辑了

![1712480495043](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480495043.png)

## **spellcheck**

用于检测元素内容是否有拼写的语法错误,注意这个元素需要设置可编辑`contenteditable=true`或者是 input,textarea 等可编辑元素

```html
<div contenteditable="true" spellcheck="true">
  what ar you doing
</div>
```

## **loading**

对于图像加载我们通常会有这样的要求,即当页面滚动到图片与视口底部交汇时才加载图片,没错这就是我们常说的懒加载。而 loading 属性则是为了处理懒加载而生的。我们将其设置为 lazy 便实现了懒加载

```html
<img src="./a.png" loading="lazy" alt="" />
```