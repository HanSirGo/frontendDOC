# 如何用原生JavaScript检测DOM是否已加载完成？

在前端开发中，我们经常需要知道网页的DOM（文档对象模型）是否已经加载完毕。对于初学者来说，这可能听起来有些复杂，但其实我们可以通过简单的JavaScript代码来实现这一目标，而不需要依赖任何框架或库。本文将带你一步步了解如何实现这一点。

# **什么是DOM？**

在讲具体方法之前，我们先来了解一下什么是DOM。DOM（文档对象模型）是网页的结构化表示，它将HTML文档表示为一个树形结构。浏览器会解析HTML并生成DOM树，我们可以使用JavaScript对这个DOM树进行操作，从而改变网页的内容和样式。

# **检查DOM是否准备好的方法**

要检查DOM是否准备好，我们主要使用两个事件：`DOMContentLoaded`和`load`。它们的区别在于：

- `DOMContentLoaded`事件在初始的HTML被完全加载和解析完成后触发，但不等待样式表、图片等资源加载。
- `load`事件在页面所有资源（包括样式表、图片等）加载完成后触发。

我们可以使用这两个事件来确定页面的加载状态，并结合`document.readyState`属性来判断DOM是否已准备好。

# **具体代码实现**

下面是具体的代码示例：

```
window.addEventListener("DOMContentLoaded", () => {
  if (document.readyState === "complete") {
    console.log('DOM已完全加载');
  } else if (document.readyState === "interactive") {
    console.log('DOM已准备好，但资源仍在加载');
  }
});

window.addEventListener("load", () => {
  if (document.readyState === "complete") {
    console.log('所有资源已加载完成');
  } else if (document.readyState === "interactive") {
    console.log('DOM已准备好，但资源仍在加载');
  }
});
```

在这段代码中，我们使用了`window.addEventListener`来监听`DOMContentLoaded`和`load`事件。当这些事件触发时，会执行相应的回调函数。在回调函数中，我们检查`document.readyState`属性的值：

- 如果值是`'complete'`，表示DOM已经完全加载，所有资源也已经加载完成。
- 如果值是`'interactive'`，表示DOM已准备好，但一些资源（如图片、框架等）仍在加载中。

# **为什么要这样做？**

了解DOM的加载状态对于前端开发非常重要。例如，如果你想在DOM完全加载后执行一些初始化操作，就需要确保这些操作不会在DOM未准备好的情况下执行。通过监听这些事件，你可以确保在合适的时机执行相应的代码，提高代码的稳定性和性能。

# **结束**

在不使用任何JavaScript框架或库的情况下，我们可以通过监听`DOMContentLoaded`和`load`事件，以及检查`document.readyState`属性的值，来确定DOM是否已准备好。这种方法简单高效，非常适合初学者学习和使用。