## IntersectionObserver

IntersectionObserver 是一种 JavaScript API，它提供了一种异步监测元素与其祖先容器或视口之间交叉状态的方法。简单来说，它可以告诉我们一个元素是否进入了视口或者与其祖先容器发生了交叉。

通过 IntersectionObserver，我们可以轻松地监听目标元素的可见性变化，进而根据这些变化来实现各种交互效果，比如懒加载图片、实现无限滚动等功能。相较于传统的事件监听方式，IntersectionObserver 更高效、灵活，可以提供更好的用户体验和性能优化。

当我们创建一个 IntersectionObserver 实例时，可以指定一个回调函数，该函数在目标元素进入或离开视口时被触发。回调函数提供了一个入参IntersectionObserverEntry，其中包含了与目标元素相关的信息，例如交叉比例、目标元素的位置和大小等。

IntersectionObserver 还支持设定阈值，即交叉比例的百分比，用于触发回调函数。默认情况下，当目标元素至少有 0% 进入视口时，回调函数会被触发。我们可以通过设置不同的阈值来满足不同的需求。

### IntersectionObserver创建使用

```js
//创建IntersectionObserver实例
const options = {
    root: null, // 根元素，默认为浏览器视口
    rootMargin: '0px', // 根元素的外边距 
    threshold: 0.5 // 交叉比例的阈值，触发回调函数的条件 
}; 
const observer = new IntersectionObserver(callback, options);
//定义回调函数
function callback(entries, observer) { 
    entries.forEach(entry => {
    if (entry.isIntersecting) { 
            // 元素进入了视口
            // 执行相应的操作，比如加载图片、添加动画等 
            } else {
            // 元素离开了视口 
            // 执行相应的操作，比如停止加载、移除动画等
        }
    });
}
//将目标元素与 IntersectionObserver 关联起来：
const targetElement = document.querySelector('.target'); 
observer.observe(targetElement);
```

### 它可以用来做什么？怎么用

**图片懒加载**

IntersectionObserver 可以用来实现图片的懒加载，即只有当图片进入视窗时才开始加载

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      let img = entry.target;
      img.src = img.dataset.src;
      observer.unobserve(img);
    }
  });
});

document.querySelectorAll('img').forEach(img => {
  observer.observe(img);
});
```

电商首页的性能优化常见，待产品进入到了可视区域内，给img赋值src的地址，才回去请求图片进行展示，这样做减少了首页的加载压力

**无限滚动**

IntersectionObserver 也可以用来实现无限滚动，即当用户滚动到页面底部时自动加载更多内容。

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      loadMore();
      observer.unobserve(entry.target);
    }
  });
});

observer.observe(document.querySelector('.footer'));
```

无限滚动的用处比较多 类如 社交媒体网站：在社交媒体网站上，可以使用无限滚动来加载用户的动态或新闻源的文章，使用户能够无限地向下滚动并加载更多内容。

电子商务网站：在商品列表页面，可以使用无限滚动来实现滚动到页面底部时自动加载更多商品，提供更好的浏览体验。

新闻网站：在新闻列表或文章阅读页面，可以使用无限滚动来加载更多新闻或相关文章，使用户可以持续阅读感兴趣的内容。

图片浏览器和相册：在图片浏览器或相册应用中，可以使用无限滚动来加载更多照片，使用户可以连续地查看图片。

博客或论坛：在博客网站或论坛上，可以使用无限滚动来加载更多帖子或博客文章，让用户可以方便地浏览并获取更多信息。

**页面动画**

当元素进入视口时触发动画效果，为页面增添交互与吸引力。

```js
// 创建 IntersectionObserver 实例
const options = {
  root: null,
  rootMargin: '0px',
  threshold: 0.2 // 元素交叉区域达到视口20%时触发回调
};

const observer = new IntersectionObserver(callback, options);

// 定义回调函数
function callback(entries, observer) {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const element = entry.target;
      element.classList.add('animate'); // 添加动画类或样式，触发动画效果

      observer.unobserve(element); // 停止观察该元素
    }
  });
}

// 找到所有需要触发动画效果的元素
const elements = document.querySelectorAll('.animated-element');

// 将元素添加到 IntersectionObserver 中进行观察
elements.forEach(element => {
  observer.observe(element);
});
```

当元素进入视口并与视口的交叉区域达到预设的阈值时，回调函数会被触发。在回调函数中，可以使用 `entry.target` 来访问触发动画效果的元素。在这个示例中，我们使用 `element.classList.add('animate')` 来添加动画类或样式，从而触发元素的动画效果。

通过调整 IntersectionObserver 实例的 `threshold` 属性，可以控制什么时候触发动画效果。例如，将 `threshold` 设置为 0.5 表示元素至少与视口有一半交叉时触发动画。这种页面动画效果可以应用于多种场景，例如：

首屏加载动画：当页面加载完成后，逐个触发元素进入视口时添加动画效果，增添页面的过渡效果。

滚动加载动画：当页面滚动到某个特定区域时，触发该区域内的元素添加动画效果，提高用户体验。

图片懒加载动画：当图片进入视口时，添加动画效果显示图片，提升页面的视觉吸引力。

> 注意，在使用动画效果时要注意性能和用户体验，避免过多或复杂的动画效果影响页面加载速度和流畅度。

### 它带来的好处是什么？

- 更好的性能：传统的监听滚动事件方式可能会导致频繁的计算，影响页面性能。而 IntersectionObserver 是浏览器原生提供的 API，它使用异步执行，可以更高效地监听元素是否进入视口，减少了不必要的计算和性能开销。
- 减少代码复杂性：IntersectionObserver 可以简化代码逻辑。使用传统的方式监听滚动事件需要手动计算元素的位置、判断元素是否进入视口，以及处理滚动事件的节流等。而通过 IntersectionObserver，只需定义回调函数，在元素进入或离开视口时触发相应操作，大大简化了代码
- 支持懒加载和无限滚动：IntersectionObserver 可以实现图片懒加载和无限滚动等常见效果。当元素进入视口时，可以延迟加载图片或触发数据请求，避免不必要的资源加载，提升页面加载速度和性能。
- 更精确的可见性控制：IntersectionObserver 提供了更精确的可见性控制。通过设置合适的阈值（threshold），可以灵活地控制元素与视口的交叉区域达到多少时触发回调。这使得开发者可以根据需求来定义元素何时被认为是进入或离开视口，从而触发相应的操作。

### 需要注意什么？

- `callback` 函数会在页面加载时和每次元素交叉视窗时被调用。
- `callback` 函数接收两个参数：一个是 IntersectionObserverEntry 对象的数组，一个是调用该函数的 IntersectionObserver 对象。
- IntersectionObserverEntry 对象包含了元素的交叉信息，如交叉比例(intersection ratio)和交叉区域的大小。
- IntersectionObserver 随着页面滚动或元素变化来检测元素是否进入、退出视口。你需要考虑触发频率和处理操作的性能影响。避免在回调函数中执行过多的计算或复杂操作，以免降低页面的性能。

### 兼容性问题

```js
if ('IntersectionObserver' in window) {
 // 浏览器支持 IntersectionObserver
} else {
 // 浏览器不支持 IntersectionObserver
}
```

在不支持 IntersectionObserver 的浏览器中，可以使用 polyfill 来提供类似的功能。