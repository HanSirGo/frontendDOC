# Vue3自定义指令



# **一、什么是指令**

在`vue`中提供了一套为数据驱动视图更为方便的操作，这些操作被称为指令系统。我们看到的`v-`开头的行内属性，都是指令，不同的指令可以完成或实现不同的功能，**除了核心功能默认内置的指令 (v-model 和 v-show)，Vue 也允许注册自定义指令。**

# **二、如何实现自定义指令**

注册一个自定义指令有全局注册与局部注册

## **全局注册**

**全局注册主要是通过Vue.directive方法进行注册。**

`Vue.directive`第一个参数是指令的名字（不需要写上`v-`前缀），第二个参数可以是对象数据，也可以是一个指令函数

```
// 注册一个全局自定义指令 `v-focus`Vue.directive('focus', {  // 当被绑定的元素插入到 DOM 中时……  inserted: function (el) {    // 聚焦元素    el.focus()  // 页面加载完成之后自动让输入框获取到焦点的小功能  }})
```

## **局部注册**

**局部注册通过在组件options选项中设置directive属性。**

```
directives: {  focus: {    // 指令的定义    inserted: function (el) {      el.focus() // 页面加载完成之后自动让输入框获取到焦点的小功能    }  }}
```

然后你可以在模板中任何元素上使用新的 `v-focus` property，如下：

```
<input v-focus />
```

## **钩子函数**

自定义指令也像组件那样存在钩子函数：

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)
- `update`：所在组件的 `VNode` 更新时调用，但是可能发生在其子 `VNode` 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新
- `componentUpdated`：指令所在组件的 `VNode` 及其子 `VNode` 全部更新后调用
- `unbind`：只调用一次，指令与元素解绑时调用

所有的钩子函数的参数都有以下：

- `el`：指令所绑定的元素，可以用来直接操作 `DOM`

- `binding`：一个对象，包含以下 `property`：

- - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`

- `vnode`：`Vue` 编译生成的虚拟节点

- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用

> 除了 `el` 之外，其它参数都应该是只读的，切勿进行修改。如果需要在钩子之间共享数据，建议通过元素的 `dataset` 来进行

举个例子：

```
<div v-demo="{ color: 'white', text: 'hello!' }"></div><script>    Vue.directive('demo', function (el, binding) {    console.log(binding.value.color) // "white"    console.log(binding.value.text)  // "hello!"    })</script>
```

# **三、自定义指令的应用场景**

使用自定义指令可以满足我们日常一些场景，这里给出几个自定义指令的案例：

- 表单防止重复提交
- 图片懒加载
- 一键 Copy的功能

## **表单防止重复提交**

表单防止重复提交这种情况设置一个`v-throttle`自定义指令来实现

举个例子：

```
// 1.设置v-throttle自定义指令Vue.directive('throttle', {  bind: (el, binding) => {    let throttleTime = binding.value; // 节流时间    if (!throttleTime) { // 用户若不设置节流时间，则默认2s      throttleTime = 2000;    }    let cbFun;    el.addEventListener('click', event => {      if (!cbFun) { // 第一次执行        cbFun = setTimeout(() => {          cbFun = null;        }, throttleTime);      } else {        event && event.stopImmediatePropagation();      }    }, true);  },});// 2.为button标签设置v-throttle自定义指令<button @click="sayHello" v-throttle>提交</button>
```

## **图片懒加载**

设置一个`v-lazy`自定义指令完成图片懒加载

```
// 创建一个 Vue 自定义指令Vue.directive('lazy', {  // 当被绑定的元素插入到 DOM 中时  inserted: function (el, binding) {    // 使用 IntersectionObserver 监听元素是否进入可视区域    let observer = new IntersectionObserver(function (entries, observer) {      entries.forEach(entry => {        if (entry.isIntersecting) {          // 如果元素进入可视区域，则加载图片          el.src = binding.value;          observer.unobserve(el);        }      });    });    // 开始监听目标元素    observer.observe(el);  }});
```

## **一键 Copy的功能**

```
// 创建一个 Vue 自定义指令Vue.directive('copy', {  bind(el, binding) {    el.addEventListener('click', () => {      // 创建一个临时的文本域      const textField = document.createElement('textarea');      textField.innerText = binding.value;      document.body.appendChild(textField);            // 选中文本域中的文本      textField.select();            // 尝试复制文本到剪贴板      try {        document.execCommand('copy');        alert('文本已成功复制到剪贴板');      } catch (err) {        alert('无法复制文本到剪贴板，请手动复制');      } finally {        // 移除临时文本域        textField.remove();      }    });  }});
```





