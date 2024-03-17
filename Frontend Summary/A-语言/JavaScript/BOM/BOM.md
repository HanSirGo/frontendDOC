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

