## vue-virtual-scroller

> vue-virtual-scroller 是一个基于Vue.js的虚拟滚动列表组件，用于优化大数据量渲染时的性能。它可以在滚动时动态地加载和卸载列表项，从而减少页面的 DOM 元素数量，提高渲染效率，同时也能够提高用户体验。
>
> vue-virtual-scroller 具有以下特点：
>
> - **无限滚动**：可以像普通滚动列表一样滚动，即使数据量非常大，也不会有性能问题。
> - **性能优秀**：只渲染当前可见区域内的数据，而不是整个列表，因此能够降低浏览器的负载，提高页面渲染性能。
> - **支持动态高度/宽度**：可以根据不同的数据项设置不同的高度或宽度。
> - **支持多种滚动方式**：支持垂直和水平两种滚动方式，同时也支持鼠标滚轮、触控滑动等多种滚动方式。
> - **易于使用**：支持通过简单的配置即可实现虚拟滚动列表，同时也可以自定义列表项的样式和渲染方式。

## vue-virtual-scroll-list

> vue-virtual-scroll-list 是一个支持高性能滚动的 Vue 组件，可以用于处理包含大量数据项的列表。它能够根据当前视窗的大小，只渲染可见部分的数据项，并在滚动时动态更新列表内容，从而实现高效的渲染和滚动性能。
>
> vue-virtual-scroll-list 具有以下特点：
>
> - **高性能**：只渲染用户可见部分的数据，大大减少了渲染时间和内存占用，并提高了滚动时的渲染性能，特别是处理包含大量数据的列表时更为明显。
> - **大型数据支持**：可以处理非常大的列表数据，在无需占用过多内存和处理器资源的同时，仍可以保证流畅的滚动体验。
> - **灵活定制**：该库提供了许多可配置的选项，如列表容器的高度和宽度、渲染行数、数据项组件等，使开发者可以根据自己的需求定制出更符合项目要求的虚拟滚动列表。
> - **可扩展性**：由于该库是基于 Vue 组件封装的，因此可以很方便地与其他 Vue 组件进行集成，适用于各种前端应用场景。
> - **轻量级**：代码简洁，不依赖任何第三方库，压缩后仅有几 kb 的大小，易于集成和使用。

