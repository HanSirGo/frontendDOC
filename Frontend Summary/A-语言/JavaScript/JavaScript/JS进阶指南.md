# JavaScript 进阶指南

- JavaScript 概述
- 函数进阶用法
- JavaScript 命名空间
- 原型
- 错误处理
- JavaScript 模块
- JavaScript 方法链式调用
- 生成器

## **JavaScript 概述**

JavaScript 是一种高级解释型编程语言，用于使网页更具交互性。它是一种非常强大的客户端脚本语言，使你的网页更加生动和互动。

它可以帮助你在网页上实现复杂而美观的设计。如果你希望你的网页看起来更生动，并且不仅仅是静态地展示内容，那么 JavaScript 是必不可少的。

### **JavaScript 的特性：**

- 它是一种脚本语言，与 Java 没有任何关系。最初，它被命名为 **Mocha**，然后改为 **LiveScript**，最后被命名为 **JavaScript**。
- JavaScript 是一种**基于对象**的编程语言，它也支持多态、封装和继承。
- 你不仅可以在**浏览器**中运行 JavaScript，还可以在**服务器**以及任何拥有 JavaScript 引擎的设备上运行它。

之前 JavaScript 基础教程中讨论的一些基础知识包括：

- 变量
- 常量
- 数据类型
- 对象
- 数组
- 函数
- 条件语句
- 循环
- switch case 语句

在本进阶 JavaScript 指南中，我们将深入探讨 JavaScript 的进阶方面。

## **函数进阶用法**

函数是一段组织化的、可重复使用的代码块，用于执行单个相关的操作。函数的一些进阶用法包括：

- 递归
- 闭包
- "new Function"
- 箭头函数
- Rest 参数和展开运算符
- 全局对象
- 函数对象
- SetTimeOut 和 SetInterval
- 函数绑定

### **递归**

递归是一种编程模式，它有助于解决可以自然地分解成多个相同类型但更简单的任务的情况，或者当一个任务可以简化为一个简单的操作和同一个任务的更简单变体时。

在解决任务的过程中，一个函数可以调用许多其他函数。一种特殊情况是函数调用自身，这就是递归。

**示例：**

```
function pow(x, n) {
  if (n == 1) {
    return x;
  } else {
    return x * pow(x, n - 1);
  }
}

alert( pow(2, 4) ); // 16
```

在上面的例子中，递归函数简化了任务并调用自身。

### **闭包**

JavaScript 是一种面向函数的语言。你可以动态地创建一个函数，将其复制到另一个变量，或者作为参数传递给另一个函数，并在之后从完全不同的地方调用它。

闭包是一个函数，它能够记住其外部变量并可以访问它们。在某些语言中，这是不可能的，或者需要以特殊的方式编写函数才能实现。但在 JavaScript 中，所有函数天生都是闭包。

**示例：**

```
var add = (function () {
  var counter = 0;
  return function () {counter += 1; return counter}
})();

add();
add();

// 现在 counter 的值为 2
```

### **"new Function"**

"new Function" 语法是创建函数的另一种方式。它很少使用，但有时别无选择。

**语法：**

```
let func = new Function ([arg1, arg2, ...argN], functionBody);
```

该函数由参数 arg1…argN 和给定的 functionBody 组成。

**示例：**

```
let sum = new Function('a', 'b', 'return a + b');
alert( sum(1, 2) ); // 3
```

在这里，函数是从一个字符串中创建的，该字符串在运行时传递。你需要在脚本中编写函数代码。但 "new Function" 允许将任何字符串转换为函数。

## **Web 开发初级完整课程**

### **箭头函数**

箭头函数是匿名的，并且改变了 this 在函数中的绑定方式。它使我们的代码更简洁，并简化了函数作用域和 this 关键字。

但箭头函数不仅仅是编写小型函数的简写。它们还有一些非常特殊和有用的特性。JavaScript 中存在一些需要编写在其他地方执行的小型函数的情况，例如：

- `arr.forEach(func)` – forEach 为每个数组项执行 func。
- `setTimeout(func)` – 内置调度程序执行 func。

在这些函数中，我们通常不想离开当前上下文。这就是箭头函数派上用场的地方。

**示例：**

```
// 箭头函数:
hello = () => {
  document.getElementById("demo").innerHTML += this;
}
// window 对象调用函数:
window.addEventListener("load", hello);
// 按钮对象调用函数:
document.getElementById("btn").addEventListener("click", hello);
```

### **Rest 参数和展开运算符**

许多 JavaScript 内置函数支持任意数量的参数，例如：

- `Math.max(arg1, arg2, …, argN)` – 返回参数中的最大值。
- `Object.assign(dest, src1, …, srcN)` – 将 src1..N 的属性复制到 dest 中

**Rest 参数**是一种改进的处理函数参数的方式，它允许我们更容易地处理函数中作为参数的各种输入。Rest 参数语法允许我们将不确定数量的参数表示为一个数组。

**示例：**

```
// es6 rest 参数
function fun(...input){
  let sum = 0;
  for(let i of input){
    sum+=i;
  }
  return sum;
}
console.log(fun(1,2)); //3
console.log(fun(1,2,4)); //7
console.log(fun(1,2,4,6)); //13
```

**展开运算符**允许在预期 0+ 个参数的地方展开可迭代对象。它主要用于预期有多个值的变量数组中。它允许我们从数组中获取参数列表。

**示例：**

```
// 展开运算符执行连接操作
let arr = [1,2,3];
let arr2 = [4,5,6];

arr = [...arr,...arr2];
console.log(arr); // [ 1, 2, 3, 4, 5, 6 ]
```

### **全局对象**

全局对象提供可在任何地方使用的变量和函数。默认情况下，这些变量和函数内置于语言或环境中。最近，globalThis 被添加到该语言中，作为全局对象的标准化名称，它应该在所有环境中都受到支持。

可以直接访问全局对象的所有属性：

```
alert("Hello");
// 与以下代码相同
window.alert("Hello");
```

### **函数对象**

在 JavaScript 中，函数是对象。不同的属性包括：

- **name** – 函数名称。通常取自函数定义，但如果没有，JavaScript 会尝试从上下文（例如赋值）中猜测它。
- **length** – 函数定义中的参数数量。不计算 Rest 参数。

如果函数被声明为函数表达式，并且它带有名称，那么它被称为命名函数表达式。该名称可以在内部用于引用自身，用于递归调用等。

**示例：**

```
function sayHi() {
  alert("Hi");
}

alert(sayHi.name); // sayHi
function f2(a, b) {}
function many(a, b, ...more) {}
alert(f2.length); // 2
alert(many.length); // 2
```

### **SetTimeOut 和 SetInterval**

如果你想在稍后的某个时间执行一个函数，这被称为调度调用。有两种方法可以实现：

- **setTimeout** 允许我们在一段时间间隔后运行一次函数。
- **setInterval** 允许我们重复运行一个函数，在一段时间间隔后开始，然后以该间隔连续重复。

这些方法不是 JavaScript 规范的一部分。但大多数环境都有一个内部调度程序并提供这些方法。

**setTimeout**

**语法：**

```
let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
```

**示例：**

```
function sayHi() {
  alert('Hello');
}

setTimeout(sayHi, 1000);
```

**setInterval**

**语法：**

```
let timerId = setInterval(func|code, [delay], [arg1], [arg2], ...)
```

**示例：**

```
// 每 2 秒重复一次
let timerId = setInterval(() => alert('tick'), 2000);

// 5 秒后停止
setTimeout(() => { clearInterval(timerId); alert('stop'); }, 5000);
```

### **函数绑定**

将对象方法作为回调传递时，例如传递给 setTimeout，存在一个众所周知的“丢失 this”问题。函数提供了一个内置方法 bind，允许修复 this。

**语法：**

```
// 更复杂的语法稍后介绍
let boundFunc = func.bind(context);
```

func.bind(context) 的结果是一个特殊的类似函数的“exotic object”，它可以作为函数调用，并透明地将调用传递给 func，并将 this 设置为 context。

**示例：**

```
let user = {
  firstName: "John"
};

function func() {
  alert(this.firstName);
}

let funcUser = func.bind(user);
funcUser(); // John
```

这些是一些函数进阶用法的示例。现在，让我们继续学习这个进阶 JavaScript 指南，并了解命名空间。

## **JavaScript 命名空间**

JavaScript 不支持命名空间。但是命名空间很重要，因为它们有助于减少添加到应用程序全局作用域中的变量、对象和函数的标识符数量。JavaScript 是一种灵活的语言，有一些方法可以绕过此限制并实现你自己的命名空间。

那么为什么我们需要命名空间呢？在 JavaScript 中，代码共享一个全局命名空间，它只是一个全局对象，将所有全局变量和函数作为属性保存。在浏览器中，这是 window 对象，如果对象太多，它往往会污染全局作用域。

**示例：**

```
let num = 5;
var obj = {};
var str = "Hello Edureka!";
function sum(x, y){
  total = x + y;
  return total;
}
numr = sum(3,3);
```

在上面的例子中，标识符 num、obj、str 和 sum 使用 var 和 let 关键字正确声明。但是函数作用域变量 total 缺少 var，numr 是 num 的拼写错误。在这里，JavaScript 将 total 和 numr 添加到全局命名空间，这很可能不是你想要的。

## **进阶 JavaScript 指南：原型**

在 JavaScript 中，对象有一个特殊的隐藏属性，称为原型，它表示 null 或引用另一个对象。现在，该对象被称为原型。在本进阶 JavaScript 指南中，我们将讨论原型的两个重要特性：

- 原型继承
- 原型方法，没有 __proto__ 的对象

### **原型继承**

在编程中，我们经常想要获取一些东西并扩展它。假设你有一个带有其属性和方法的用户对象，并且你想将管理员和访客作为它的略微修改的变体。在这里，你想重用用户中已有的内容，而不是复制它的方法，只需在其基础上构建一个新对象即可。

原型继承是一种语言特性，可以帮助你实现这一点。

**示例：**

```
let pet = {
  eats: true
};
let dog = {
  jumps: true
};

dog.__proto__ = pet; // (*)

// 我们现在可以在 dog 中找到这两个属性：
alert( dog.eats ); // true (**)
alert( dog.jumps ); // true
```

在这里，如果你在 dog 中寻找一个属性，并且它不存在，JavaScript 会自动从 pet 中获取它。

### **原型方法，没有 __proto__ 的对象**

在原型继承中，我们使用了 __proto__，但这现在是一种过时的方法。在本进阶 JavaScript 指南中，我们将了解设置原型的现代方法：

- **Object.create(proto[, descriptors])** – 它创建一个空对象，并将给定的 proto 作为 [[Prototype]] 和可选的属性描述符。
- **Object.getPrototypeOf(obj)** – 这将返回 obj 的 [[Prototype]]。
- **Object.setPrototypeOf(obj, proto)** – 此方法将 obj 的 [[Prototype]] 设置为 proto。

**示例：**

```
let pet = {
  eats: true
};

// 创建一个以 pet 为原型的
let dog = Object.create(pet);

alert(dog.eats); // true

alert(Object.getPrototypeOf(dog) === pet); // true

Object.setPrototypeOf(dog, {}); // 将 dog 的原型更改为 {}
```

现在让我们继续学习这个进阶 JavaScript 指南，并了解错误处理。

## **错误处理**

无论你的编程能力如何，你的脚本都可能包含错误。它们可能是由于我们的错误、意外的用户输入、错误的服务器响应或任何其他原因而发生的。

通常情况下，脚本会在出现错误时立即停止，并将错误打印到控制台。现在，有一个语法结构 **try..catch** 允许捕获错误，并且在程序崩溃之前做一些更合理的事情。

### **Try..Catch 语法**

try..catch 结构有两个主要块：

- **Try**
- **Catch**

```
try {

  // 代码...

} catch (err) {

  // 错误处理

}
```

- 首先，执行 **try {…}** 中的代码。
- 如果没有错误，则忽略 **catch(err)**。执行到达 try 的末尾并继续，跳过 catch。
- 如果发生错误，则 try 执行停止，并且控制流转到 catch(err) 的开头。

**示例：**

```
try {
  alert('开始 try 运行'); // (1) <--
  // ...这里没有错误
  alert('结束 try 运行'); // (2) <--
} catch(err) {
  alert('由于没有错误，因此忽略 Catch'); // (3)
}
```

本进阶 JavaScript 教程的下一个方面是关于 JavaScript 中的模块。

## **JavaScript 中的模块**

模块是一个独立的代码块，它将语义相关的变量和函数分组在一起。模块不是 JavaScript 中的内置结构。但是 JavaScript 模块模式提供了一种创建模块的方法，这些模块具有明确定义的接口，这些接口暴露给模块的客户端。

模块的一个重要优势是，你可以在必要时修改内部功能，而不会影响程序的其余部分。这促进了封装和信息隐藏。要在 JavaScript 中定义模块，你可以通过创建一个匿名立即函数来利用匿名闭包。

**示例：**

```
var MODULE = (function () {
  var module = {};
  var privateVariable = 7;

  function privateMethod() {
    // ..
  }

  module.moduleProperty = 1;
  module.moduleMethod = function () {
    // ...
  };
  return module;
}());
```

现在让我们继续学习这个进阶 JavaScript 教程，并了解 JavaScript 方法的链式调用。

## **JavaScript 方法链式调用**

JavaScript 允许你在单个表达式中对一个对象调用多个方法。为了调用多个方法，我们有链式调用。链式调用是将方法调用与它们之间的点串在一起的过程。

**语法：**

```
object.method1().method2().method3();
```

在构建链时，对象只命名一次，然后在其上调用多个方法。要使其工作，你的方法必须返回它们操作的对象。每个方法都对对象进行操作，并在完成后将其返回给下一个调用。

**示例：**

```
account.number("11324567").setBalance(15000).applyCredit(200);
```

在上面的例子中，你学习了如何设置一个银行账户，它由账号、余额和信用额度组成。

JavaScript 中的链式调用可以提高性能和可读性。在这里，jQuery 库广泛使用了链式调用。让我们举个例子，看看如何链接 jQuery 选择器方法：

```
$("#myDiv").removeClass("off").addClass("on").css("background": "red");
```

现在让我们继续学习这个进阶 JavaScript 指南，并了解什么是生成器。

## **生成器**

生成器是一类特殊的函数，它们简化了编写迭代器的任务。因此，此函数生成一系列值，而不是单个值。

因此，在 JavaScript 中，生成器是一个函数，它返回一个你可以调用 next() 的对象。因此，每次调用 next() 都会返回一个形状的对象。

**示例：**

```
{
  value: Any,
  done: true|false
}
```

value 属性将包含该值。done 属性为 true 或 false。当 done 变为 true 时，生成器停止并且不会生成更多值。