# HTML 小技巧，助你提升编码能力

## **创建联系链接**

使用 HTML 创建可点击的电子邮件、电话和短信链接：

```
<!-- 电子邮件链接 -->
<a href="mailto:name@example.com"> 发送电子邮件 </a>

<!-- 电话链接 -->
<a href="tel:+1234567890"> 联系我们 </a>

<!-- 短信链接 -->
<a href="sms:+1234567890"> 发送短信 </a> 
```

## **创建可折叠内容**

当你想在网页上包含可折叠内容时，可以使用 `<details>` 和 `<summary>` 标签。

```
<details> 标签创建了一个隐藏内容的容器，而 <summary> 标签提供了一个可点击的标签来切换该内容的可见性。
```

```
<details>  <summary>点击展开</summary>  <p>此内容可以展开或折叠。</p></details>
```

## **使用语义化元素**

为你的网站选择语义化元素而不是非语义化元素。它们使你的代码更有意义，并改进结构、可访问性和 SEO。

![1725780691268](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725780691268.png)

## **分组表单元素**

使用 `<fieldset>` 标签将表单中的相关元素分组，并使用 `<legend>` 标签与 `<fieldset>` 一起为 `<fieldset>` 标签定义标题。

这对于创建更高效和更易访问的表单非常有用。

```
<form>
   <fieldset>
      <legend>个人信息</legend>
      <label for="firstname">名字:</label>
      <input type="text" id="firstname" name="firstname" />
      <label for="email">电子邮件:</label>
      <input type="email" id="email" name="email" />
      <label for="contact">联系方式:</label>
      <input type="text" id="contact" name="contact" />
      <input type="button" value="提交" />
   </fieldset>
</form> 
```

## **增强下拉菜单**

可以使用 `<optgroup>` 标签在 `<select>` HTML 标签中对相关选项进行分组。

这可以在你处理大型下拉菜单或很长的选项列表时使用。

```
<select>
   <optgroup label="水果">
      <option>苹果</option>
      <option>香蕉</option>
      <option>芒果</option>
   </optgroup>
   <optgroup label="蔬菜">
      <option>番茄</option>
      <option>西兰花</option>
      <option>胡萝卜</option>
   </optgroup>
</select> 
```

## **改进视频展示**

`poster` 属性可以与 `<video>` 元素一起使用，以便在用户播放视频之前显示图片。

```
<video controls poster="image.png" width="500">
  <source src="video.mp4" type="video/mp4 />
</video> 
```

## **支持多选**

可以使用 `<input>` 和 `<select>` 元素的 `multiple` 属性，允许用户一次选择/输入多个值。

```
<input type="file" multiple />
<select multiple>
    <option value="java">Java</option>
    <option value="javascript">JavaScript</option>
    <option value="typescript">TypeScript</option>
    <option value="rust">Rust</option>
</select> 
```

## **将文本显示为下标和上标**

`<sub>` 和 `<sup>` 元素可以分别用来将文本显示为下标和上标。

![1725780736941](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725780736941.png)

## **创建下载链接**

可以使用 `<a>` 元素的 `download` 属性来指定，当用户点击链接时，链接的资源应该被下载而不是被导航。

```
<a href="document.pdf" download="document.pdf"> 下载 PDF </a> 
```

## **为相对链接定义基本 URL**

可以使用 `<base>` 标签为网页中所有相对 URL 定义基本 URL。

当你想为网页上的所有相对 URL 创建一个共享的起点时，这很有用，它可以更容易地导航和加载资源。

```
<head>
   <base href="https://shefali.dev" target="_blank" />
</head>
<body>
   <a href="/blog">博客</a>
   <a href="/get-in-touch">联系我们</a>
</body> 
```

## **控制图片加载**

<img> 元素的 loading 属性可以用来控制浏览器如何加载图片。它有三个值：“eager”、“lazy” 和 “auto”。

```
<img src="picture.jpg" loading="lazy"> 
```

## **管理翻译功能**

可以使用 `translate` 属性来指定元素的内容是否应该被浏览器的翻译功能翻译。

```
<p translate="no">
  这段文本不应该被翻译。
</p> 
```

## **设置最大输入长度**

使用 `maxlength` 属性，可以设置用户在输入字段中输入的最大字符数。

```
<input type="text" maxlength="4"> 
```

## **设置最小输入长度**

使用 `minlength` 属性，可以设置用户在输入字段中输入的最小字符数。

```
<input type="text" minlength="3"> 
```

## **启用内容编辑**

使用 `contenteditable` 属性来指定元素的内容是否可编辑。

它允许用户修改元素中的内容。

```
<div contenteditable="true">
   你可以编辑此内容。
</div> 
```

## **控制拼写检查**

可以使用 `<input>` 元素、可编辑内容元素和 `<textarea>` 元素的 `spellcheck` 属性来启用或禁用浏览器对它们的拼写检查。

```
<input type="text" spellcheck="true"/> 
```

## **确保可访问性**

`alt` 属性指定了当图片无法显示时图片的备用文本。

始终为图片包含描述性的 alt 属性，以改善可访问性和 SEO。

```
<img src="picture.jpg" alt="图片描述"> 
```

## **定义链接的目标行为**

可以使用 `target` 属性来指定当点击链接时链接的资源将在哪里显示。

```
<!-- 在同一个框架中打开 -->
<a href="https://shefali.dev" target="_self">打开</a>

<!-- 在一个新的窗口或标签页中打开 -->
<a href="https://shefali.dev" target="_blank">打开</a>

<!-- 在父框架中打开 -->
<a href="https://shefali.dev" target="_parent">打开</a>

<!-- 在窗口的完整主体中打开 -->
<a href="https://shefali.dev" target="_top">打开</a>

<!-- 在命名的框架中打开 -->
<a href="https://shefali.dev" target="framename">打开</a> 
```

## **提供附加信息**

`title` 属性可以用来在用户将鼠标悬停在元素上时提供有关该元素的附加信息。

```
<p title="世界卫生组织">WHO</p> 
```

## **接受特定文件类型**

可以使用 `accept` 属性来指定服务器接受的文件类型（仅用于文件类型）。这与 `<input>` 元素一起使用。

```
<input type="file" accept="image/png, image/jpeg" /> 
```

## **优化视频加载**

可以使用 `<video>` 元素的 `preload` 属性来使视频文件加载更快，以便实现更流畅的播放。

```
<video src="video.mp4" preload="auto">
   你的浏览器不支持 video 标签。
</video> 
```