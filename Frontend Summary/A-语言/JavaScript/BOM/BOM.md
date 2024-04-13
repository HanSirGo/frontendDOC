## BOM

```
BOM 指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。BOM 的核心是 window，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global（全局） 对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。window 对象含有 locati on 对象、navigator 对象、screen 对象等子对象，并且 DOM 的最根本的对象 document 对象也是 BOM 的 window 对 象的子对象。
```

```js
window.navigator.onLine // 判断是否有网
```

> 《DOM, DOCUMENT, BOM, WINDOW 有什么区别?》
>
> 《Window 对象》
>
> 《DOM 与 BOM 分别是什么，有何关联？》
>
> 《JavaScript 学习总结（三）BOM 和 DOM 详解》

##### 获取 loaction 中参数值

```js
# 在 JavaScript 中，你可以使用 window.location.search 属性来获取 URL 中的查询参数。

// 获取 URL 中的查询参数
var queryString = window.location.search;

// 去除问号，并将查询参数转换为对象形式
var params = new URLSearchParams(queryString);

// 获取特定参数的值
var paramValue = params.get('paramName');

console.log(paramValue);
```

> **请注意**，这只适用于现代浏览器环境。在某些旧版本的浏览器中，可能需要使用其他方法来解析查询参数。

##### 启用虚拟键盘 API

默认情况下，虚拟键盘 API 是不可用的，需要使用 Javascript 来启用它。

```js
if ("virtualKeyboard" in navigator) {
  navigator.virtualKeyboard.overlaysContent = true
}
```

这有点奇怪，还需使用 Javascript 来启用。当然，我们也可以使用这样的 `meta` 标签来启用：

```html
<meta
  name="viewport"
  content="width=device-width, initial-scale=1.0, virtual-keyboard=overlays-content"
/>
```

或者使用 CSS 属性：

```css
html {
  virtual-keyboard: overlays-content;
}
```

