# 33 个 JavaScript 开发者都应该知道的概念

*你真的认为你了解多少 JavaScript？* 你可能知道如何 **编写函数**，理解 **简单的算法**，甚至可以 **编写类**。但是你知道什么是 *类型化数组* 吗？

你不需要现在就了解所有这些概念，但最终你会在你的职业生涯中用到它们。这就是我建议你 **收藏此列表** 的原因，因为你很可能会遇到其中一个主题，然后你会想要一个教程来完全理解它。

需要注意的是，这份清单的灵感来自以下仓库：

![1724501770709](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501770709.png)

# **目录**

# **1. 调用栈**

![1724501795084](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501795084.png)

在 JavaScript 中，回调函数只是一个作为参数传递给另一个函数并在该函数内部被调用或执行的函数。一个函数需要等待另一个函数执行或返回一个值，这形成了功能链（当 X 完成后，执行 Y，依此类推）。这就是回调函数通常用于 JavaScript 异步操作中以提供同步能力的原因。

**示例：** 下面是回调函数的示例

```
const greeting = (name) => {  
    console.log('你好 ' + name);  
}  
const processUserName = (callback) => {  
    name = '极客邦';  
    callback(name);  
}  
processUserName(greeting);
```

**输出：**

```
你好 极客邦
```

在上面的示例中，注意 `greeting` 作为参数（回调函数）传递给 `processUserName` 函数。在执行 `greeting` 函数之前，它会等待事件 `processUserName` 首先执行。

# **2. 原始类型**

![1724501823144](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501823144.png)

除了对象之外的所有类型都定义了不可变的值（即不能更改的值）。例如（与 C 语言不同），字符串是不可变的。我们将这些类型的值称为“原始值”。

JavaScript 有六种原始数据类型：数字、字符串、布尔值、null、undefined 和符号。这些类型表示简单的值，并且是不可变的，这意味着它们的值不能被更改。了解这些类型的工作原理对于编写高效且无错误的代码至关重要。

```
const age = 25; // 数字  
const name ="John"; // 字符串  
const isDeveloper = true; // 布尔值  
let emptyValue = null; // Null  
let notDefined; // Undefined
```

# **3. 值类型和引用类型**

![1724501840484](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501840484.png)

在 JavaScript 中，变量可以保存值类型或引用类型。值类型包括数字、字符串、布尔值——它们直接存储数据。引用类型（如对象和数组）存储指向数据的引用。

```
// 值类型  
let num1 = 42;  
let num2 = num1; // num2 获取 num1 的副本
// 引用类型  
let arr1 = [1, 2, 3];  
let arr2 = arr1; // arr2 引用与 arr1 相同的数组
```

当你更改 `num1` 时，它不会影响 `num2`。但是，修改 `arr1` 也会更改 `arr2`，因为它们指向同一个数组。

# **4. 隐式、显式、名义、结构化和鸭子类型**

![1724501864895](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501864895.png)

这些术语描述了编程语言中不同的类型系统，影响了变量和数据类型的处理方式。

```
// 结构化类型  
interface Shape {  
  sides: number;  
}  
function getArea(shape: Shape) {  
  // 根据结构计算面积  
}
```

这些概念影响了开发人员如何在代码中处理变量赋值和类型声明，定义了编程语言中类型系统的性质。

# **5. == vs === vs typeof**

![1724501911140](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501911140.png)

JavaScript 提供了两个相等运算符：`==`（松散相等）和 `===`（严格相等）。掌握它们之间的区别至关重要。此外，`typeof` 运算符有助于确定值的类型，这在处理动态数据时非常有用。

```
console.log(5 =="5"); // true (松散相等)  
console.log(5 ==="5"); // false (严格相等)  
console.log(typeof"Hello"); // "string"
```

# **6. 函数作用域、块级作用域和词法作用域**

![1724501930502](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501930502.png)

JavaScript 使用函数作用域和块级作用域。在函数内部声明的变量只能在该函数内部访问（函数作用域）。块级作用域，由 `let` 和 `const` 引入，将变量限制在其块内（花括号内）。

```
// 函数作用域  
function exampleFunction() {  
 let localVar = "我是局部的!";  
 console.log(localVar);  
}  
// 块级作用域  
function blockExample() {  
 if (true) {  
 let blockVar = "我在一个块里!";  
 console.log(blockVar);  
 }  
 // console.log(blockVar); // 错误：blockVar 在块外未定义  
}
```

词法作用域意味着函数可以访问其外部（封闭）作用域中的变量。

# **7. 表达式 vs 语句**

![1724501952889](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501952889.png)

表达式是产生值的代码，而语句是执行操作的代码。

```
// 表达式  
let sum = 3 + 4;  
// 语句  
if (sum === 7) {  
 console.log("总和是 7。");  
}
```

表达式可以是语句的一部分，例如 `if` 语句中的 `3 + 4`。区分它们有助于理解代码结构。

# **8. IIFE、模块和命名空间**

![1724501981779](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724501981779.png)

`IIFE`（立即执行函数表达式）、`模块` 和 `命名空间` 有助于代码组织和封装。

```
// IIFE (立即执行函数表达式)  
(function () {  
  const privateVar = "我是私有的!";  
  console.log(privateVar);  
})();  
// 模块  
// module.js  
export function add(a, b) {  
  return a + b;  
}  
// main.js  
import { add } from "./module.js";  
console.log(add(2, 3));  
// 命名空间  
const myNamespace = {  
  variable: "我在命名空间里",  
  log() {  
    console.log(this.variable);  
  }  
};  
myNamespace.log();
```

IIFE 确保私有作用域，模块促进模块化代码组织，命名空间防止全局变量冲突。

# **9. 消息队列和事件循环**

![1724502000514](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502000514.png)

JavaScript 使用事件循环以事件驱动的方式执行代码。消息队列保存要处理的事件。下面是一个简单的异步示例：

```
console.log("开始");  
setTimeout(() => {  
 console.log("在 Timeout 里面");  
}, 1000);  
console.log("结束");
```

即使 `setTimeout` 设置为 1 秒，`结束` 也会先被记录，因为 `setTimeout` 回调函数进入消息队列，事件循环在栈清空后处理它。

# **10. setTimeout、setInterval 和 requestAnimationFrame**

![1724502024397](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502024397.png)

这些函数——`setTimeout`、`setInterval` 和 `requestAnimationFrame`——对于管理 JavaScript 中的异步操作和动画至关重要。

```
// setTimeout  
setTimeout(() => {  
  console.log("延迟日志");  
}, 1000);  
// setInterval  
const intervalId = setInterval(() => {  
  console.log("重复日志");  
}, 2000);  
// requestAnimationFrame  
function animate() {  
  console.log("动画中...");  
  requestAnimationFrame(animate);  
}  
animate();
```

`setTimeout` 安排一个函数在指定的延迟后运行，`setInterval` 以固定的时间延迟重复调用一个函数，而 `requestAnimationFrame` 通常用于流畅和高效的动画。

# **11. JavaScript 引擎**

![1724502083509](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502083509.png)

为 Web 编写代码有时感觉有点神奇，因为开发人员编写了一系列字符，就像魔术一样，这些字符在浏览器中变成了具体的图像、文字和动作。了解这项技术可以帮助开发人员更好地调整他们作为程序员的技能。

# **12. 位运算符、类型化数组和数组缓冲区**

![1724502094512](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502094512.png)

好的，所以从技术上讲，对于计算机来说，一切都归结为 1 和 0。它不使用数字、字符或字符串进行操作，它只使用二进制数字（位）。这种解释的简短版本是，所有内容都以二进制形式存储。然后计算机使用 UTF-8 等编码将保存的位组合映射到字符、数字或不同的符号。

# **13. DOM 和布局树**

![1724502108743](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502108743.png)

文档对象模型，通常称为 DOM，是使网站具有交互性的重要组成部分。它是一个允许编程语言操作网站内容、结构和样式的接口。JavaScript 是连接到互联网浏览器中 DOM 的客户端脚本语言。

```
// 获取对必要 DOM 元素的引用   
const commentsListElement = document.getElementById('commentsList');   
const postCommentButton = document.getElementById('postComment');   
    
// 将事件监听器附加到“发布评论”按钮   
postCommentButton.addEventListener('click', function() {   
  // 创建一个新的评论项   
  const comment = document.createElement('li');   
  comment.textContent = '新评论';   
    
  // 将新评论追加到评论列表   
  commentsListElement.appendChild(comment);   
});
```

# **14. 工厂函数和类**

![1724502166872](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502166872.png)

JavaScript 提供了两种创建对象的主要方法：工厂函数和类。工厂函数是生成并返回对象的函数，而类提供了更结构化和面向对象的蓝图。

```
// 工厂函数  
function createPerson(name, age) {  
  return {  
    name,  
    age,  
    greet() {  
      console.log(`你好，我的名字是 ${this.name}.`);  
    },  
  };  
}  
// 类  
class Person {  
  constructor(name, age) {  
    this.name = name;  
    this.age = age;  
  }  
  greet() {  
    console.log(`你好，我的名字是 ${this.name}.`);  
  }  
}  
const person1 = createPerson("John", 25);  
const person2 = new Person("Alice", 30);  
person1.greet();  
person2.greet();
```

虽然这两种方法都能 achieve the same result，但类提供了一种更有条理、更常规的方式来构造对象，尤其是在涉及继承的情况下。

# **15. this、call、apply 和 bind**

![1724502239745](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502239745.png)

在 JavaScript 中，`call`、`apply` 和 `bind` 是用于控制函数中 `this` 关键字值的方法。它们提供了指定函数应在哪个上下文中执行的灵活性。

```
const person = {  
  name: "John",  
  greet(message) {  
    console.log(`${message}, ${this.name}!`);  
  }  
};  
const anotherPerson = {  
  name: "Alice",  
};  
// call  
person.greet.call(anotherPerson, "你好");  
// apply  
person.greet.apply(anotherPerson, ["嗨"]);  
// bind  
const greetAlice = person.greet.bind(anotherPerson, "嘿");  
greetAlice();
```

当你想要在调用函数时显式设置 `this` 的值时，这些方法特别有用。

# **16. new、构造函数、instanceof 和实例**

![1724502258923](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502258923.png)

每个 JavaScript 对象都有一个原型。JavaScript 中的所有对象都从它们的原型继承方法和属性。

# **17. 原型继承和原型链**

![1724502292309](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502292309.png)

JavaScript 使用基于原型的继承，其中对象可以通过 `原型链` 从其他对象继承属性和方法。

```
// 原型  
function Animal(name) {  
  this.name = name;  
}  
Animal.prototype.sound = function () {  
  console.log("一些通用的声音");  
};  
// 继承  
function Dog(name, breed) {  
  Animal.call(this, name);  
  this.breed = breed;  
}  
Dog.prototype = Object.create(Animal.prototype);  
Dog.prototype.constructor = Dog;  
Dog.prototype.sound = function () {  
  console.log("汪汪!");  
};  
const myDog = new Dog("Buddy", "拉布拉多");  
myDog.sound(); // 汪汪!
```

理解原型继承对于在 JavaScript 中创建高效且可重用的代码结构至关重要。

# **18. Object.create 和 Object.assign**

![1724502332103](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502332103.png)

`Object.create` 方法是在 JavaScript 中创建新对象的其中一种方法。

# **19. map、reduce、filter**

![1724502343931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502343931.png)

即使你不知道什么是函数式编程，你可能也一直在使用 `map`、`filter` 和 `reduce`，因为它们非常有用，并且通过允许你编写更清晰的逻辑来减少代码的糟糕程度。

# **20. 纯函数、副作用、状态突变和事件传播**

![1724502357113](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502357113.png)

我们的许多错误都源于与 IO 相关的、数据突变的、带有副作用的代码。这些错误在我们的代码库中随处可见——从诸如接受用户输入、通过 http 调用接收意外响应或写入文件系统之类的事情。不幸的是，这是一个残酷的现实，我们应该习惯于处理它。或者，真的如此吗？

# **21. 闭包**

![1724502370940](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502370940.png)

当一个函数即使在其外部函数已经执行完毕后仍然保留对其外部作用域中变量的访问权限时，就会发生闭包。这个概念对于创建私有变量和在函数调用之间维护状态至关重要。

```
function outerFunction() {  
  let outerVar = "我来自外部!";  
  function innerFunction() {  
    console.log(outerVar);  
  }  
  return innerFunction;  
}  
const closureExample = outerFunction();  
closureExample(); // 打印：我来自外部!
```

在这个例子中，`innerFunction` 闭包了 `outerVar`，即使在 `outerFunction` 完成执行后，它仍然可以访问 `outerVar`。

# **22. 高阶函数**

![1724502389344](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502389344.png)

它们是将其他函数作为参数或返回函数的函数。它们提倡更函数式的编程风格，增强了代码的可读性和可维护性。

```
const numbers = [1, 2, 3, 4, 5];  
// Map - 将函数应用于数组中的每个元素  
const squared = numbers.map((num) => num * num);  
// Filter - 创建一个包含满足条件的元素的新数组  
const evenNumbers = numbers.filter((num) => num % 2 === 0);  
// Reduce - 将数组 reduce 为单个值  
const sum = numbers.reduce((acc, num) => acc + num, 0);  
console.log(squared); // [1, 4, 9, 16, 25]  
console.log(evenNumbers); // [2, 4]  
console.log(sum); // 15
```

高阶函数可以使用简洁且富有表现力的代码来处理数组和数据转换。

# **23. 递归**

![1724502410161](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502410161.png)

递归是一种函数调用自身的技术，通常用于解决可以分解成更小子问题的问题。

```
function factorial(n) {
  if (n === 0) {
    return 1;
  }
  return n * factorial(n - 1);
}
console.log(factorial(5)); // 120
```

在这个例子中，`factorial` 函数递归调用自身来计算阶乘。理解递归对于解决某些类型的问题至关重要。

# **24. 集合和生成器**

![1724502426964](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502426964.png)

JavaScript 提供了各种集合，例如数组和集合，用于存储和管理多个值。此外，`生成器` 是可以暂停和恢复的特殊函数，允许更灵活和高效的迭代。

```
// 数组  
const colors = ["red", "green", "blue"];  
// 集合  
const uniqueColors = new Set(colors);  
// 生成器  
function* generateNumbers() {  
  yield 1;  
  yield 2;  
  yield 3;  
}  
const numbersGenerator = generateNumbers();  
console.log([...uniqueColors]); // ["red", "green", "blue"]  
console.log([...numbersGenerator]); // [1, 2, 3]
```

集合有助于组织和操作数据，而生成器有助于创建可迭代的序列，尤其适用于延迟加载或处理大型数据集。

# **25. Promise**

![1724502455109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502455109.png)

我们理解回调的概念，但是如果你的代码中有回调嵌套回调，并且一直这样下去会发生什么？嗯，这种回调的递归结构被称为“回调地狱”，而 Promise 则有助于解决这类问题。当我们需要执行两个或多个背靠背操作（或链接回调）时，Promise 在异步 JavaScript 操作中非常有用，其中每个后续函数在之前的函数完成后才开始。Promise 是一个可能在未来某个时间产生单个值的对象，该值可以是已解析的值，也可以是未解析的原因（已拒绝）。

Promise 解决了“回调地狱”的问题，这只不过是回调的递归结构（回调嵌套回调，依此类推）。

Promise 可能处于三种可能的状态...

- **已完成：** 当操作成功完成时。
- **已拒绝：** 当操作失败时。
- **待定：** 初始状态，既未完成也未拒绝。

**示例：** 让我们通过一个示例来讨论如何在 JavaScript 中创建 Promise。

```
const promise = new Promise((resolve, reject) => {  
    isNameExist = true;  
    if (isNameExist) {  
        resolve("用户名存在")  
    } else {  
        reject("错误")  
    }  
})  
promise.then(result => console.log(result)).catch(() => {  
            console.log('错误!')  
            })
```

**输出：**

```
用户名存在  Promise {<resolved>: undefined}
```

考虑上面的代码作为示例 Promise，假设像异步执行 `isNameExist` 操作一样，在 Promise 对象参数中，有两个函数 `resolve` 和 `reject`。如果操作成功，这意味着 `isNameExist` 为 `true`，则它将被解析并显示输出“用户名存在”，否则操作将失败或被拒绝，并将显示结果“错误！”。你可以在 Promise 中轻松执行链接操作，其中将执行第一个操作，并将第一个操作的结果传递给第二个操作，这将继续进行下去。

# **26. async/await**

![1724502478179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502478179.png)

**停止并等待直到某些事情被解决**。Async & await 只是 Promise 之上的语法糖，就像 Promise 一样，它也提供了一种更同步地维护异步操作的方法。所以在 JavaScript 中，异步操作可以用不同的版本来处理…

- ES5 -> 回调
- ES6 -> Promise
- ES7 -> async & await

你可以使用 Async/Await 来执行 Rest API 请求，你希望在将数据推送到视图之前完全加载数据。对于 Node.js 和浏览器程序员来说，async/await 是一个很棒的语法改进。它帮助开发人员在 JavaScript 中实现函数式编程，并且还提高了代码的可读性。

**示例：** 下面是示例。

```
const showPosts = async () => {  
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');  
    const posts = await response.json();  
    console.log(posts) ;  
  }  
  showPosts();
```

**输出：**

![1724502531605](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502531605.png)

为了通知 JS 我们正在使用 Promise，我们需要将 `await` 包裹在一个 `async` 函数中。在上面的示例中，我们 (a) 等待两件事：响应和帖子。在我们能够将响应转换为 JSON 格式之前，我们需要确保我们已经获取了响应，否则我们最终可能会转换一个尚不存在的响应，这很可能会导致错误。

# **27. 数据结构**

![1724502544326](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502544326.png)

Javascript 每天都在发展。随着 React、Angular、Vue、Node.js、Electron、React Native 等框架和平台的快速发展，将 Javascript 用于大型应用程序变得非常普遍。

# **28. 昂贵的操作和 Big O 表示法**

![1724502556216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502556216.png)

“什么是 Big O 表示法？”这是开发人员面试中一个非常常见的问题。简而言之，它是算法运行时间与输入长度关系的数学表达式，通常指的是最坏情况下的场景。

# **29. 算法**

![1724502567125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502567125.png)

在数学和计算机科学中，算法是定义明确的指令的有限序列，通常用于解决一类特定问题或执行计算。

# **30. 继承、多态和代码复用**

![1724502580916](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502580916.png)

类继承是一种让一个类扩展另一个类的方法，因此我们可以在现有功能的基础上创建新功能。

JavaScript 使用基于原型的继承，其中对象可以通过 `原型链` 从其他对象继承属性和方法。

```
// 原型  
function Animal(name) {  
  this.name = name;  
}  
Animal.prototype.sound = function () {  
  console.log("一些通用的声音");  
};  
// 继承  
function Dog(name, breed) {  
  Animal.call(this, name);  
  this.breed = breed;  
}  
Dog.prototype = Object.create(Animal.prototype);  
Dog.prototype.constructor = Dog;  
Dog.prototype.sound = function () {  
  console.log("汪汪!");  
};  
const myDog = new Dog("Buddy", "拉布拉多");  
myDog.sound(); // 汪汪!
```

理解原型继承对于在 JavaScript 中创建高效且可重用的代码结构至关重要。

# **31. 设计模式**

![1724502601700](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502601700.png)

每个开发人员都努力编写可维护、可读和可重用的代码。随着应用程序变得更大，代码结构变得更加重要。设计模式被证明是解决这一挑战的关键——为特定情况下常见问题提供组织结构。

# **32. 部分应用、柯里化、组合和管道**

![1724502613515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502613515.png)

函数组合是一种将多个简单函数组合起来构建更复杂函数的机制。

# **33. 简洁代码**

![1724502625266](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724502625266.png)

编写干净、易懂和可维护的代码是每个开发人员都必须掌握的技能。

