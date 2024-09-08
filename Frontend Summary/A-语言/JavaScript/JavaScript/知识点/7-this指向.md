#### globalThis 全局对象

```js
globalThis对象提供了一种在不同环境下（包括浏览器和Node.js）访问全局对象的一致方式。

console.log(globalThis === window); // 在浏览器场景下: true
console.log(globalThis === global); // 在 Node.js 中: outputs: true
```

#### This指向问题

![1710583890208](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710583890208.png)

```
执行上下文对象 context

this 执行上下文对象，永远指向当前函数的调用者

1.全局环境下的this 是window  全局环境是 onload 方法执行产生的，而onload 的使用者是window
2.当一个函数使用的时候看前面有没有 . ,有指向.前边的对象，没有就是window
3.构造函数的this 指向 new 出来的实例对象
4.计时器中this 指向 window
5.箭头函数中没有 this 指向问题，指向父级函数的调用者
6.call bind apply 改变 this 指向
```

```js
const obj1 = {
	foo(){
		console.log(this)
	}
}
const fn = obj1.foo
fn() // window

const obj2 = {
    foo: function() {
        function bar() {
            console.log(this)
        }
        bar()
    }
}
obj2.foo() // window

```

```js
关于this的总结：
	
	1. 沿着作用域向上找最近的一个function (不是箭头函数),看这个function最终是怎样执行的;
	2. this的指向取决于所属function的调用方式，而不是定义;
	3. function调用一般分为以下几种情况:
	
		1>. 作为函数调用,即: foo()
			指向全局对象(globalThis,浏览器环境是window,node环境global);
			注意严格模式问题,严格模式下是undefined;
			
		2>. 作为方法调用,即: foo.bar()/foo.bar.baz()/foo['bar']/foo[0]()
			指向最终调用这个方法的对象
			
		3>. 作为构造函数调用,即: new Foo()
			指向新对象
			
		4>. 特殊调用,即: foo.call()/foo.apply()/foo.bind()
			参数指定成员
			
	4. 找不到所属的function,就是全局对象
```

```js
var length = 10
function fn () {
    console.log(this.length)
}

const obj = {
    length:5,
    method (fn) {
        fn()
        arguments[0]()
    }
}
obj.method(fn,1,2) // 10 3
```

```
nodejs 环境
	由于文件代码中最顶层的作用域实际上是一个伪全局环境,所以最顶层的this并不指向全局对象
严格模式
	严格模式下原本应该指向全局的this都会指向undefined
	
```

> 「前端料包」一文彻底搞懂JavaScript中的this、call、apply和bind

#### 手写call、apply及bind函数

##### call 函数的实现步骤

> - 1.判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
> - 2.判断传入上下文对象是否存在，如果不存在，则设置为 window 。
> - 3.处理传入的参数，截取第一个参数后的所有参数。
> - 4.将函数作为上下文对象的一个属性。
> - 5.使用上下文对象来调用这个方法，并保存返回结果。
> - 6.删除刚才新增的属性。
> - 7.返回结果。

```js
// call函数实现
Function.prototype.myCall = function(context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }

  // 获取参数
  let args = [...arguments].slice(1),
    result = null;

  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;

  // 将调用函数设为对象的方法
  context.fn = this;

  // 调用函数
  result = context.fn(...args);

  // 将属性删除
  delete context.fn;

  return result;
};
```

##### apply 函数的实现步骤

> - 1. 判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
> - 2. 判断传入上下文对象是否存在，如果不存在，则设置为 window 。
> - 3. 将函数作为上下文对象的一个属性。
> - 4. 判断参数值是否传入
> - 5. 使用上下文对象来调用这个方法，并保存返回结果。
> - 6. 删除刚才新增的属性
> - 7. 返回结果

```js
// apply 函数实现

Function.prototype.myApply = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }

  let result = null;

  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;

  // 将函数设为对象的方法
  context.fn = this;

  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }

  // 将属性删除
  delete context.fn;

  return result;
};
```

##### bind 函数的实现步骤

> - 1.判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
> - 2.保存当前函数的引用，获取其余传入参数值。
> - 3.创建一个函数返回
> - 4.函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。

```js
// bind 函数实现
Function.prototype.myBind = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }

  // 获取参数
  var args = [...arguments].slice(1),
    fn = this;

  return function Fn() {
    // 根据调用方式，传入不同绑定值
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
```

> 《手写 call、apply 及 bind 函数》
>
> 《JavaScript 深入之 call 和 apply 的模拟实现》

