# BOM-浏览器对象模型

```
BOM 指的是浏览器对象模型，它指的是把浏览器当做一个对象来对待，这个对象主要定义了与浏览器进行交互的法和接口。BOM 的核心是 window，而 window 对象具有双重角色，它既是通过 js 访问浏览器窗口的一个接口，又是一个 Global（全局） 对象。这意味着在网页中定义的任何对象，变量和函数，都作为全局对象的一个属性或者方法存在。window 对象含有 locati on 对象、navigator 对象、screen 对象等子对象，并且 DOM 的最根本的对象 document 对象也是 BOM 的 window 对 象的子对象。
```

## Window

#### window.open

```js
window.open(url, [name], [configuration])

# url: 新打开页面的url
# name: 新打开窗口的名字，可以通过此名字获取该窗口对象
# configuration: 新打开窗口的一些配置项，比如是否有菜单栏、滚动条、长高等信息

// 例如：新打开一个没有菜单栏、标题栏、工具栏但是有滚动条、状态栏、地址栏且可伸缩窗口的方法调用如下：
window.open('index.html','newWindow','menubar=0,scrollbars=1,resizable=1,status=1,titlebar=0,toolbar=0,location=1')
```

**新打开窗口名字可以是自定义的值，此处还可以是以下几个值，与超链接a的target属性的值相同**

| 窗口name的值 | 描述                      |
| ------------ | ------------------------- |
| _blank       | 默认值，在新窗口打开url   |
| _self        | 在当前窗口打开链接url     |
| _parant      | 在父窗口打开链接url       |
| _top         | 在顶级窗口打开url         |
| framename    | 在指定的框架中打开链接url |

> 《DOM, DOCUMENT, BOM, WINDOW 有什么区别?》
>
> 《Window 对象》
>
> 《DOM 与 BOM 分别是什么，有何关联？》
>
> 《JavaScript 学习总结（三）BOM 和 DOM 详解》

## loaction 

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

## navigator

#### navigator.sendBeacon

`navigator.sendBeacon` 方法是 Web API 提供的一个用于异步发送少量数据到服务器的接口。它特别设计用于在页面卸载或页面关闭时确保数据能够被成功发送。以下是 `sendBeacon` 方法的详细介绍：

1. **基本用法**

`navigator.sendBeacon` 主要用于在页面卸载或页面关闭时异步地将数据发送到服务器。它是一个非常适合用于记录用户活动、日志信息或其他需要在页面关闭时发送的数据的方法。

**语法**

```js
navigator.sendBeacon(url, data);
```

- **url**: 需要发送数据的服务器 URL。
- **data**: 要发送的数据。可以是 `String`, `Blob`, `FormData`, `ArrayBuffer`, 或 `URLSearchParams` 类型的数据。

**返回值**

- 返回一个 `boolean` 值，表示数据是否成功地被提交到服务器。返回 `true` 表示数据被成功提交，`false` 表示数据提交失败。

2. **特点和优点**

- **异步操作**: `sendBeacon` 方法是异步的，不会阻塞页面的卸载或关闭过程。这使得它特别适合于在用户关闭页面时发送少量数据。
- **可靠性**: 由于它是专门设计用于页面卸载时的异步操作，`sendBeacon` 可以提高数据发送的成功率，即使在页面即将关闭时。
- **简化操作**: `sendBeacon` 简化了发送数据的过程，通常不需要处理复杂的回调和请求管理。
- **适用于小数据**: `sendBeacon` 适合发送小型数据包，如用户活动日志或简单的跟踪信息。

3. **使用场景**

- **用户行为跟踪**: 记录用户在页面上的行为，例如点击、滚动等，当用户离开页面时使用 `sendBeacon` 发送这些数据到服务器。
- **日志记录**: 在页面卸载时发送应用程序日志或错误信息。
- **数据同步**: 在页面关闭之前确保某些数据被同步到服务器，例如保存用户的草稿数据。

4. **示例代码**

**发送简单的数据**

```js
// 当用户离开页面时，发送数据到服务器
window.addEventListener('unload', () => {
  const data = JSON.stringify({ event: 'page-unload', timestamp: Date.now() });
  navigator.sendBeacon('/log-endpoint', data);
});
```

**发送表单数据**

```js
window.addEventListener('unload', () => {
  const formData = new FormData();
  formData.append('event', 'page-unload');
  formData.append('timestamp', Date.now());

  navigator.sendBeacon('/log-endpoint', formData);
});
```

5. **限制和注意事项**

- **数据大小**: `sendBeacon` 主要设计用于发送小型数据。如果需要发送大量数据，可能需要考虑其他方法，如 `fetch` 或 `XMLHttpRequest`。
- **不支持的场景**: `sendBeacon` 只适用于发送数据到服务器，对于需要处理响应的复杂请求，使用 `fetch` 或 `XMLHttpRequest` 更合适。
- **浏览器支持**: 大多数现代浏览器都支持 `sendBeacon`，但在某些旧版本的浏览器中可能不被支持。在使用前需要检查浏览器的兼容性。

6. **与 fetch 的比较**

- **sendBeacon**: 专注于在页面卸载时发送少量数据，不会阻塞页面关闭过程。适用于发送日志、跟踪数据等。
- **fetch**: 更灵活，可以处理更复杂的请求和响应，适用于发送和接收大数据量或需要处理请求结果的场景。支持更多的 HTTP 方法和功能，但在页面关闭时可能不如 `sendBeacon` 可靠。

总结

`navigator.sendBeacon` 是一个用于在页面卸载时异步发送数据到服务器的简单且高效的工具。它适合用于记录用户行为、日志信息或同步小型数据。它的异步特性确保了页面关闭时数据能够被发送，但应注意其适用场景和数据大小限制。

#### 检测当前网络是否在线（判断是否有网）

```js
const isOnline = navigator.onLine ? "Online" : "Offline";

// 输出：isOnline = 'Online'
```

**代码解析**

**1.** **navigator.onLine? "Online" : "Offline"**

使用了三元运算符来判断当前的网络连接状态。

navigator.onLine 是一个布尔值属性，当浏览器有网络连接时返回true，没有网络连接时返回false。

navigator.onLine只能提供一个大致的网络连接状态，不能准确地判断网络的可用性和稳定性。

例如，即使navigator.onLine为true，也可能存在网络延迟、服务器故障等问题，导致无法正常访问网络资源。

它也不能区分不同类型的网络连接，比如 Wi-Fi、移动数据等。

#### 如何判断用户设备？

判断用户设备类型是 Web 开发中常见的需求，尤其是在做响应式设计和优化用户体验时。你可以通过多种方法来判断用户设备的类型、操作系统、浏览器等信息。下面是一些常用的方法和技术：

1. **使用 navigator 对象**

`navigator` 对象提供了有关浏览器和操作系统的一些基本信息，可以用来初步判断设备类型。

```js
const userAgent = navigator.userAgent;

console.log(userAgent);

// 判断是否为移动设备
const isMobile = /Mobi|Android/i.test(userAgent);
console.log('Is mobile:', isMobile);

// 判断是否为桌面设备
const isDesktop = !isMobile;
console.log('Is desktop:', isDesktop);

// 判断操作系统
const isWindows = /Win/i.test(userAgent);
const isMac = /Mac/i.test(userAgent);
const isLinux = /Linux/i.test(userAgent);
console.log('Is Windows:', isWindows);
console.log('Is Mac:', isMac);
console.log('Is Linux:', isLinux);
```

2. **使用 window.matchMedia**

`window.matchMedia` 可以用来检测特定的媒体查询条件，如屏幕大小、方向等。这对响应式设计非常有用。

```js
// 检测是否为移动设备
const isMobile = window.matchMedia("(max-width: 767px)").matches;
console.log('Is mobile:', isMobile);

// 检测是否为横屏
const isLandscape = window.matchMedia("(orientation: landscape)").matches;
console.log('Is landscape:', isLandscape);
```

3. **使用 navigator.platform**

`navigator.platform` 提供有关操作系统平台的简洁信息，但它不如 `navigator.userAgent` 那样详细。

```js
const platform = navigator.platform;
console.log('Platform:', platform);

// 判断是否为 Windows
const isWindows = /Win/.test(platform);
console.log('Is Windows:', isWindows);
```

4. **使用第三方库**

一些第三方库可以帮助你更准确地检测用户设备和浏览器类型：

- **Modernizr**: 检测浏览器支持的功能，而不仅仅是设备类型。
- **Detect.js**: 提供设备、浏览器和操作系统的检测。
- **Bowser**: 解析用户代理字符串，帮助检测浏览器和设备类型。

```js
// 使用 Bowser 检测设备类型
const bowser = require("bowser");
const browser = bowser.getParser(window.navigator.userAgent);
const result = browser.getResult();

console.log(result);
```

5. **使用 CSS 媒体查询**

在 CSS 中使用媒体查询可以根据设备特性应用不同的样式。这可以帮助你实现响应式设计。

```js
/* 移动设备样式 */
@media (max-width: 767px) {
  .example {
    font-size: 14px;
  }
}

/* 桌面设备样式 */
@media (min-width: 768px) {
  .example {
    font-size: 18px;
  }
}
```

6.**用window.innerWidth 和 window.innerHeight**

检查窗口的宽度和高度可以帮助你判断设备类型，尤其是在实现响应式设计时。

```js
const width = window.innerWidth;
const height = window.innerHeight;

if (width <= 768) {
  console.log('Mobile or tablet');
} else {
  console.log('Desktop');
}
```

7. **利用 navigator.connection**

`navigator.connection` 提供有关网络连接的信息，虽然它不直接提供设备类型，但可以提供连接的速度和类型，这在某些场景下可能会有用。

```js
if (navigator.connection) {
  console.log('Connection type:', navigator.connection.effectiveType);
}
```

总结

选择哪种方法来判断用户设备取决于你的具体需求。你可以使用 `navigator` 对象和 `window.matchMedia` 来检测用户的基本设备信息，也可以使用第三方库来获得更详细的设备和浏览器信息。CSS 媒体查询是实现响应式设计的重要工具，而 `window.innerWidth` 和 `window.innerHeight` 可以帮助你在 JavaScript 中进行设备类型判断。根据实际需要选择合适的方法，能够帮助你提供更好的用户体验。

## 其他

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

## 