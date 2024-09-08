# Proxy可以监听基本数据类型吗？

Proxy无法直接监听基本数据类型（如字符串、数字、布尔值等），因为它们是不可变的。Proxy只能在对象级别上进行操作，而不是基本数据类型。

当我们尝试使用Proxy包装基本数据类型时，会得到一个TypeError错误，因为基本数据类型不具有属性和方法。

```
const value = 'Hello';
const handler = {  set(target, property, value) {    console.log(`Setting property '${property}' to '${value}'`);    target[property] = value;    return true;  }};
const proxyValue = new Proxy(value, handler); // TypeError: Cannot create proxy with a non-object as target
```

我们无法直接追踪对上述示例中 `value` 局部变量的读写，原生 JavaScript 没有提供任何机制能做到这一点。但是，我们是可以追踪对象属性的读写的。



如果要监听基本数据类型的更改，最好使用其他方式，例如通过触发事件或调用回调函数来通知更改。可以创建一个自定义的数据包装器，将基本数据类型包装在对象中，并在该对象上实现监听逻辑。或者直接给对象属性赋值基本类型，然后给对象添加Proxy以监听其属性的更改。

```
function ValueWrapper(value) {    this.value = value;    this.onChange = null;}
ValueWrapper.prototype.setValue = function (newValue) {    this.value = newValue;    if (typeof this.onChange === 'function') {      this.onChange(this.value);    }}
const wrapper = new ValueWrapper('Hello');wrapper.onChange = newValue => {    console.log(`Value changed: ${newValue}`);}wrapper.setValue('Hi');// 输出：Value changed：Hi
// 给对象添加Proxy以监听其属性的更改const obj= {    value: 'Hello'}
const handler = {  set(target, property, value) {    console.log(`Setting property '${property}' to '${value}'`);    target[property] = value;    return true;  }};
const proxyObj = new Proxy(obj, handler); proxyObj.value = 'Hi';// 输出：Setting property 'value' to 'Hi'
```



当你在 Vue 模板中使用了一个 ref，然后改变了这个 ref 的值时，Vue 会自动检测到这个变化，并且相应地更新 DOM。这是通过一个基于依赖追踪的响应式系统实现的。当一个组件首次渲染时，Vue 会追踪在渲染过程中使用的每一个 ref。然后，当一个 ref 被修改时，它会触发追踪它的组件的一次重新渲染。



在标准的 JavaScript 中，检测普通变量的访问或修改是行不通的。然而，我们可以通过 getter 和 setter 方法来拦截对象属性的 get 和 set 操作。



该 .value 属性给予了 Vue 一个机会来检测 ref 何时被访问或修改。在其内部，Vue 在它的 getter 中执行追踪，在它的 setter 中执行触发。

# proxy 能够监听到对象中的对象的引用吗？

JavaScript 中的 `Proxy` 对象可以监听到嵌套在对象中的对象的引用，但需要注意的是，`Proxy` 本身只能直接监听它所代理的对象。对于对象中的嵌套对象，默认情况下，`Proxy` 是无法直接监听到的。

如果要监听嵌套对象的变化，你需要递归地为嵌套对象也创建一个 `Proxy`。这样，当访问嵌套对象的属性时，相关的操作（如 `get`、`set`）也会被代理。举个例子：

```
function deepProxy(target, handler) {
  // 如果目标是对象或数组，我们递归处理它
  if (typeof target === 'object' && target !== null) {
    for (let key in target) {
      if (typeof target[key] === 'object' && target[key] !== null) {
        target[key] = deepProxy(target[key], handler);
      }
    }
  }
  
  // 创建并返回一个新的 Proxy
  return new Proxy(target, handler);
}

const handler = {
  get(target, prop, receiver) {
    console.log(`Getting ${prop}`);
    return Reflect.get(target, prop, receiver);
  },
  set(target, prop, value, receiver) {
    console.log(`Setting ${prop} to ${value}`);
    return Reflect.set(target, prop, value, receiver);
  }
};

const nestedObject = {
  a: 1,
  b: {
    c: 2,
    d: 3
  }
};

const proxy = deepProxy(nestedObject, handler);

proxy.b.c = 10;  // 这将触发代理的 set 处理器
console.log(proxy.b.c);  // 这将触发代理的 get 处理器
```

在这个例子中，`deepProxy` 函数会递归地为嵌套对象创建 `Proxy`，这样即使是对象中的对象的引用也会被代理监听到。

如果你不进行递归处理，只是简单地为外层对象创建 `Proxy`，那么嵌套对象的修改是不会触发 `Proxy` 的处理器的。