# 微前端代码隔离方案，手把手实现一个 JS 沙箱隔离！

今天我们一起来探究一下前端 js 沙箱的核心实现逻辑，我们将从以下几个方面来展开讨论：

1. 准备调试环境，探究沙箱需要解决的问题。
2. 创建沙箱环境。
3. 通过 with 语句改变沙箱变量作用域链。
4. 通过 proxy 拦截 with 上下文的get,set操作。

这几个方面一步一步实现一个简易的js沙箱。

## **准备调试环境，探究沙箱需要解决的问题：**

js沙箱大量应用在微前端框架执行子应用代码的场景中。我们这里采用最简单的方式来模拟一下这个场景。

我们准备一个js文件，这个js文件的内容就模拟微前端应用基座应用的执行环境，在里面通过 var 定义一个全局变量:

![1721464514419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464514419.png)

我们准备两段字符串，字符串的内容就是js代码，我们假设这两段代码就分别代表了两个子应用的代码：

![1721464531624](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464531624.png)

假如现在微前端框架需要执行子应用1的代码，那么就会 fetch 子应用1的静态资源服务器，获取到类似于 subCode1 这样的一个代码字符串。执行子应用2也是同理。那么现在第一个问题来了，怎样自动执行字符串内部的js代码？在js中有两种比较常见的方式：

1. Function:
2. eval

Function 是js提供的一个内置构造函数，基于它，可以通过字符串，创建出一个js函数，因此我们可以这样尝试：

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)因为 Function(...)本身就是一个表达式，所以我们可以直接进行调用：

![1721464557815](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464557815.png)

我们查看一下控制台：

![1721464571828](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464571828.png)

可以看到子应用1的代码就被成功执行了，我们尝试子应用2的代码也是同理。

eval是js内置的一个函数，这个函数接收一个字符串，eval函数会将这个字符串作为标准的js代码进行执行：

![1721464669664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464669664.png)

我们查看输出：

![1721464587077](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464587077.png)

![图片](data:image/svg+xml,%3C%3Fxml version='1.0' encoding='UTF-8'%3F%3E%3Csvg width='1px' height='1px' viewBox='0 0 1 1' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink'%3E%3Ctitle%3E%3C/title%3E%3Cg stroke='none' stroke-width='1' fill='none' fill-rule='evenodd' fill-opacity='0'%3E%3Cg transform='translate(-249.000000, -126.000000)' fill='%23FFFFFF'%3E%3Crect x='249' y='126' width='1' height='1'%3E%3C/rect%3E%3C/g%3E%3C/g%3E%3C/svg%3E)可以看到子应用代码同样被正常执行了。 这样很方便呀，实际上微前端框架内部也是使用类似的方式来执行子应用的js代码的，但是这样会有什么问题呢？

1. 目前子应用中的变量定义会污染全局环境：

目前我们在子应用中定义的变量在全局环境中可以直接访问，所以我们需要调整一下 subCode1 以及 subCode2 两个子应用的测试代码：

![1721464640813](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464640813.png)

我们将两端子应用的测试代码包裹到了一个立即执行函数中，这样子应用内部定义的变量就不会污染全局了。大部分构建工具构建的产物也是这种方式。

1. 在子应用内部可以赤裸裸的访问 window 对象，并且可以直接对它们进行操作：

![1721464691358](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464691358.png)

![1721464704392](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464704392.png)

我们可以直接在子应用2中通过访问 window 对象来改变 window 对象的内容。如何拦截子应用直接污染浏览器宿主环境的 window 对象是实现js沙箱最核心的问题。

## **创建沙箱环境**

我们先编写一个沙箱构造器函数：

```
var a = 1

const sandbox = () => {
  return (subCode1) => {
     
  }
}
```

我们编写了一个高阶函数，这个高阶函数返回了一个可以接收并且执行子应用代码的函数。我们期望如果需要执行子应用的结果代码，只需要这样进行调用：

```
const box = sandbox()

box(子应用1代码)
box(子应用2代码)
box(子应用3代码)
```

接下来就是要在高阶函数中去编写执行子应用的代码了，我们沿着沙箱的需求来思考解决方案。 我们的需求就是屏蔽浏览器window对象，为了实现这个需求，我们可以在返回的高阶函数中编写如下的逻辑：

```
const boxCtx = {}

return (subCode1) => {
  const boxFn = `(function (window) {
    ${code}
  })(boxCtx)`
  // 通过eval函数执行子应用代码
  eval(boxFn)
}
```

实际上就是我们在父函数中创建了一个沙箱的上下文环境，在高阶函数中，将子应用代码放到一个立即执行函数中去进行执行，立即执行函数的参数就是一个window变量，这样，当我们在子应用代码内部访问 window 的时候，因为作用域链的原理，就只会访问到我们在沙箱环境中设置的 window 参数，而无法访问到浏览器全局的 window 对象了。我们可以测试一下：

```
sandbox()('(function() { window.testVal1 = "testVal1"; window.a = 2; })()')
console.log(' window.testVal1',  window.testVal1)
```

![1721464751359](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464751359.png)

这样做了之后我们就已经可以拦截子应用中对于 window 对象属性的 set 操作了。 但是如果我们在子应用中尝试直接通过变量通过 a 来访问刚刚设置的值的时候，却是这样的结果：

```
sandbox('(function() { window.a = 2; console.log("ssss", a); })()')
```

![1721464767872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464767872.png)

依然访问的是全局变量a，原因其实很简单，因为我们目前构建的沙箱函数内部只有一个 window 变量，并没有 a 变量。所以js在执行这里的时候只能沿着作用域链访问到全局作用域中的a变量了。要解决这个问题，我们必须确保，我们设置的 window 对象上的所有的属性全部挂载到沙箱的作用域中。

## **通过 with语句改变沙箱变量作用域链：**

with 语句的能力可以这样理解：在js运行时将指定代码段的执行上下文设置为指定的对象，从而调整该代码端的作用域链。 使用with，我们可以这样改动并且测试沙箱代码：

```
const boxCtx = { b: "b" }

return (subCode1) => {
  const boxFn = `(function boxFn (window) {
    with(window) { ${code} }
  })(boxCtx)`
  // 通过eval函数执行子应用代码
  eval(boxFn)
}

sandbox('(function sub1() { window.a = 2; console.log("b", b) })()')
```

![1721464826372](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464826372.png)

此时 with 语句使得沙箱部分的作用域链变成了类似于这样的结构：

```
sub1(子应用函数上下文) ---> window(with语句设置的代码上下文) ---> boxFn 函数执行上下文 ---> 全局执行上下文
```

因此当我们直接在子应用内部访问 a 变量的时候：

![1721464840546](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464840546.png)

就可以正常访问子应用内部设置的全局变量了。

但是依然有问题没有解决，比如我们尝试执行以下的函数：

```
sandbox('(function() { window.a = 2; window.console.log("ssss", a); })()')
```

![1721464857345](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464857345.png)

原因其实很简单，因为此时的沙箱的window对象并没有 console 属性，要解决这个问题，我们可以尝试扩展以下沙箱的上下文的内容，一个很暴力的解法就是：

```
  const boxCtx = {    console  }
```

我们可能会想到一个更加简单的扩展沙箱上下文的方法：

```
  const boxCtx = {    ...window  }
```

这样做了之后我们再利用 window.console 来输出 a 的值：

![1721464875242](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464875242.png)

为什么会是1呢？如果我们此时在控制台中访问一下window.a的值会看到一个更加诡异的现象：

![1721464900664](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464900664.png)

全局的window的a属性被修改了。

全局window的a属性被修改的原因其实很简单，因为浏览器宿主的 window 上嵌套了一个 window 属性，并且这个属性指向了全局window对象，因此window.window是一个无限嵌套的循环引用：

![1721464911833](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464911833.png)

正是因为这样，所以当我们直接使用 ... 浅拷贝一个 window 对象的时候，实际上 嵌套的 window 属性也就被拷贝过来了，因此此时沙箱上下文是一个类似于这样的对象:

```
{
  ...,
  window: {
    ...,
    window
  }
}
```

因此，当沙箱中查找 window 变量的时候是可以直接在 boxCtx 上查找到，而查找的结果就是全局 window 对象的浅拷贝。因此就导致了我们在沙箱内部对于 window 的所有操作实际上操作在了全局window对象上。因此直接拷贝 window 对象来扩展沙箱上下文的方式是不行的。我们得找其他更加简单的方案。

## **通过 proxy 拦截 with 上下文的get,set操作：**

我们可以提出这样的设想，我们在沙箱内部通过 window.xxx 进行 get 的时候，如果 xxx 在沙箱内部的 window 上存在，那么就直接使用该值，如果不存在，就前往 window 对象上去查找 xxx 的值，这样是不是就可以解决这个问题了呢？而做到这一步的关键就是拦截沙箱上下文的 get 操作，因此首先我们将传递给 with 语句的上下文对象变成一个 Proxy 代理对象：

```
const createSandboxCtxProxy = () => {
  return new Proxy({}, {
    get(target, key, receiver) {
      
    },
    set(target, key, value, receiver) {

    }
  })
}

const boxCtx = createSandboxCtxProxy()

```

紧接着可以这样去拦截 get 和 set 操作：

```
get(target, key, receiver) {
      console.log('get', key)
      //优先从代理对象上取值
      if(Reflect.has(target,key)){
         return Reflect.get(target,key);
      }

      //如果找不到，就直接从window对象上取值
      const rawValue = Reflect.get(window,key);
      //其他情况直接返回
      return rawValue
    },
    set(target, key, value, receiver) {
      return Reflect.set(target, key, value)
    },

```

这样操作之后我们再次来进行测试:

```
sandbox('(function() { window.a = 2; window.b = "b"; })()')
```

![1721464949346](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464949346.png)

这样就达到了和改造之前一样的效果了，紧接着我们这样测试：

```
 sandbox('(function() { window.a = 2; window.b = "b"; window.console.log("a,b", a, b) })()')
```

![1721464965010](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464965010.png)

可以看到 console 对象已经可以正常的访问到了。那么是不是就大功告成了呢？我们再来试一下 window.alert 这类方法：

```
sandbox('(function() { window.a = 2; window.b = "b"; window.alert(`a, b: ${a}, ${b}`) })()')
```

![1721464981972](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464981972.png)

这个错误是因为浏览器 window 上的方法在调用的时候函数内部的 this 一定要指向浏览器 window 对象。比如我们使用一个更好理解的方式来复原这个错误：

![1721464995116](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721464995116.png)

定位到了错误的原因，我们就很容易可以解决了，只需要调整 get 方法：

```
get(target, key, receiver) {
      console.log('get', key)
    //优先从代理对象上取值
    if(Reflect.has(target,key)){
      return Reflect.get(target,key);
    }

    //如果找不到，就直接从window对象上取值
    const rawValue = Reflect.get(window,key);

    //如果兜底的是一个函数，需要绑定window对象，比如window.addEventListener
    if(typeof rawValue === 'function'){
      const valueStr = rawValue.toString();
      if(!/^function\s+[A-Z]/.test(valueStr) && !/^class\s+/.test(valueStr)){
        return rawValue.bind(window); // 所有 window 上非构造函数调用时候的 this 绑定window对象
      }
    }

    //其他情况直接返回
    return rawValue
    },

```

再次测试：

![1721465016253](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1721465016253.png)

