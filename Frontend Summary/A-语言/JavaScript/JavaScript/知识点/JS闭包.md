# 掌握JavaScript闭包，轻松提升代码效率

![1723975136690](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723975136690.png)



闭包（Closures）是JavaScript中的一个重要概念，它让你的代码变得强大又灵活，同时保持代码的组织性。对于初学者来说，理解闭包可以帮助你更好地掌握JavaScript，写出更加优雅的代码。

# **什么是闭包？**

闭包这个词听起来有点高深，实际上它来源于一个叫lambda演算的数学术语，但我们不需要深究这些技术细节。让我们用更通俗易懂的方式来理解闭包。

想象一下，你有一个神奇的盒子，这个盒子里装着一些小玩意儿，比如说一把钥匙。这时你又在这个盒子里放了一个更小的盒子，这个小盒子同样可以使用大盒子里的钥匙，即使大盒子已经关上了。这个小盒子就像是一个函数，而大盒子就是它的外部函数，闭包就像是这种神奇的盒子，内层的函数（小盒子）可以访问外层函数（大盒子）的变量（钥匙），即使外层函数已经执行完毕（大盒子关上）。

## **为什么闭包这么重要？**

闭包让我们可以在代码中创建更加灵活和强大的功能。比如说，闭包可以帮助我们：

1. **封装数据**：像是给变量上了锁，只有特定的函数才能访问和修改它们。
2. **创建私有变量**：让某些变量对外部不可见，只有通过特定函数才能访问。
3. **工厂函数**：根据不同的输入生成不同的函数。

## **更具体的解释**

闭包是在一个函数内部定义另一个函数时创建的，内层的函数可以访问外层函数的变量，即使在外层函数执行完毕后，这种行为对于各种编程模式非常重要，在JavaScript中广泛用于封装、数据隐私和函数工厂。

闭包的关键在于函数在程序中的表现方式。简单来说，闭包只适用于函数。对象和类本身没有闭包，但它们的方法可能会有闭包。这主要取决于函数在不同地方调用时的行为。

换句话说，如果一个函数在代码中不同地方调用时表现不同，那就形成了闭包。如果函数的行为在任何地方调用时都一样，那就不是闭包。

# **闭包的基本示例**

为了让大家更容易理解闭包，我们可以用一个更生活化的比方来说明。想象一下，你有一个魔法箱子（相当于`outer`函数），这个箱子里有一本日记（相当于`outerVariable`变量）。在这个魔法箱子里，还藏着一个秘密小盒子（相当于`inner`函数），这个小盒子可以读取并展示日记内容，即使魔法箱子已经关上并收起来。

用代码表示就是：

```
function outer() {
  let outerVariable = '我是外层函数的变量';

  function inner() {
    console.log(outerVariable);
  }
  
  return inner;
}

const innerFunction = outer();
innerFunction(); // 输出：我是外层函数的变量
```

在这个例子中，`outer`函数内部定义了一个`inner`函数，这个`inner`函数能够访问`outer`函数中的`outerVariable`变量。即使`outer`函数已经执行完毕，`inner`函数仍然能够访问并打印出`outerVariable`变量的内容。

这就像是小盒子可以在魔法箱子关闭后，仍然能够读取箱子里的日记，这就是闭包的神奇之处！通过这种方式，我们可以实现数据的封装和隐私保护，让代码更加灵活和安全。

# **闭包的实际应用案例**

## **1. 数据隐私**

闭包常用于创建私有变量。通过在函数内部定义变量并返回一个访问该变量的函数，我们可以控制对该变量的访问。

想象一下，你有一个储钱罐（相当于`createCounter`函数），储钱罐里有一个计数器（相当于`count`变量）。你只能通过一个特定的开口（相当于返回的函数）来增加储钱罐里的钱，而不能直接拿到里面的计数器。

### **代码示例**

```
function createCounter() {
  let count = 0;
  
  return function() {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 输出：1
console.log(counter()); // 输出：2
console.log(counter()); // 输出：3
```

在这个例子中，`count`变量被定义在`createCounter`函数内部，并且只能通过返回的函数来访问和修改。即使`createCounter`函数已经执行完毕，`count`变量仍然可以通过闭包机制被访问。这就像储钱罐里的钱只能通过特定的开口增加，确保了数据的隐私和安全。

通过这种方式，我们可以有效地保护变量不被外部直接修改，同时还能灵活地操作这些变量。这是闭包在实际编程中的一个重要应用。

## **2. 函数工厂**

闭包在创建函数工厂时非常有用。函数工厂就是能够生成带有预设配置的其他函数的函数。

想象一下，你有一个模具工厂（相当于`createAdder`函数），这个工厂可以生产带有特定颜色的模具（相当于预设值的函数）。比如你可以告诉工厂生产红色模具或者蓝色模具，然后再用这些模具制作不同颜色的物品（相当于带有不同预设值的函数）。

### **代码示例**

```
function createAdder(x) {
  return function(y) {
    return x + y;
  };
}

const add5 = createAdder(5);
console.log(add5(2)); // 输出：7
console.log(add5(10)); // 输出：15

const add10 = createAdder(10);
console.log(add10(2)); // 输出：12
console.log(add10(10)); // 输出：20
```

在这个例子中，`createAdder`函数生成了新的函数，这些新函数会把特定的值加到它们的输入上。`createAdder(5)`生成了一个每次调用都加5的函数，而`createAdder(10)`生成了一个每次调用都加10的函数。

这就像是工厂根据不同的订单生产不同颜色的模具，然后你可以用这些模具制作出带有这些特定颜色的物品。通过闭包机制，生成的函数保留了对初始参数的访问权，从而实现了函数的灵活创建。

## **3. 事件处理程序**

闭包在事件处理程序中非常常见，它能让我们在事件触发后依然保持对函数作用域的访问。

想象一下，你在一个聚会上有很多朋友（相当于DOM元素），你想告诉某个朋友一个秘密信息（相当于`message`变量），当你拍一下他的肩膀（相当于点击事件）时，他就会告诉你这个秘密信息。

### **代码示例**

```
function attachEventHandler(element, message) {
  element.addEventListener('click', function() {
    alert(message);
  });
}

const button = document.querySelector('button');
attachEventHandler(button, '按钮被点击了！');
```

在这个例子中，`attachEventHandler`函数接受一个元素和一个消息作为参数。当元素被点击时，事件处理程序会弹出一个提示框显示消息。通过闭包机制，事件处理函数保留了对`message`变量的访问权限，因此可以在点击按钮时显示正确的消息。

这就像你在聚会上告诉朋友一个秘密信息，并安排他在你拍他肩膀时告诉你这个信息。无论聚会进行多久，朋友都记得你的秘密信息，并且在你需要时告诉你。这是因为事件处理函数通过闭包机制记住了它的上下文。

## **4. 迭代器**

闭包还可以帮助我们创建自定义的迭代器，让我们能够逐一遍历数组中的元素。

想象一下，你有一本故事书（相当于数组`array`），你希望每次打开这本书时只能看到一页内容（相当于一个数组元素），直到你看完所有的页数（遍历完整个数组）。这个过程就像是一个迭代器在逐一提供内容。

### **代码示例**

```
function createIterator(array) {
  let index = 0;
  return function() {
    if (index < array.length) {
      return array[index++];
    } else {
      return null;
    }
  };
}

const iterator = createIterator([1, 2, 3]);

console.log(iterator()); // 输出：1
console.log(iterator()); // 输出：2
console.log(iterator()); // 输出：3
console.log(iterator()); // 输出：null
```

在这个例子中，`createIterator`函数接受一个数组作为参数，并返回一个闭包函数。这个闭包函数每次调用时，都会返回数组中的下一个元素。当数组遍历完毕后，它会返回`null`。通过闭包机制，返回的函数保留了对`index`变量的访问权限，因此能够正确地逐一遍历数组。

这就像你在阅读一本故事书，每次只看到一页内容，直到看完所有的页数。闭包机制确保你可以记住当前看到的页数，并且能够在每次阅读时接着上次的进度继续。

# **闭包的生命周期与垃圾回收（GC）**

当一个函数闭包引用了一个变量，只要这个函数还在使用中，那个变量就会一直存在。即使有多个函数共享这个变量，只要有一个函数在使用，这个变量就不会被清理掉。但是，一旦最后一个引用这个变量的函数也不再使用了，这个闭包就会消失，垃圾回收器（GC）就可以清理掉这个变量了。

当我们不再需要一个函数及其闭包时，我们可以通过解除对该函数的引用来使其不再使用，从而让垃圾回收器清理相关的内存。以下是一个示例：

```
function createFunction() {
  let message = 'Hello, World!';

  return function() {
    console.log(message);
  };
}

let myFunction = createFunction();
myFunction(); // 输出：Hello, World!

// 现在我们不再需要 myFunction
myFunction = null; // 解除引用

// 此时，闭包中的 message 变量可以被垃圾回收器清理
```

在这个例子中，createFunction返回了一个闭包函数，该闭包函数引用了message变量。当我们第一次调用myFunction时，它会输出Hello, World!。

当我们不再需要myFunction时，可以通过将myFunction设置为null来解除对它的引用。这样，闭包中的message变量就没有任何引用了，垃圾回收器就可以清理掉它，从而释放内存。

在编写高效且运行流畅的程序时，了解这一点非常重要。如果不注意，闭包可能会无意中保留你以为已经不需要的变量，导致内存使用不断增加。因此，当你不再需要某个函数及其闭包时，及时清理它们是非常必要的。



**结束**

闭包可以通过让函数记住已经计算过的数据，避免重复计算，从而提高效率。它还可以通过将某些变量保留在函数实例内，使代码更易于理解和维护。这意味着函数更加专注，并且你不必每次使用它们时都传入相同的信息。

# JavaScript 中的闭包陷阱案发现场

## **setInterval中的闭包**

### **案发现场**

先来看一段代码：

```
import { useState } from 'react';
import './App.css';

function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setInterval(() => {
      setCount(count + 1);
      console.log(count);
    }, 1000);
  };

  return (
    <>
    <button onClick={handleClick}>click button</button>
    <text>{count}</text>
    </>
  );
}

export default App;
```

这段代码的逻辑很简单，点击`button`，触发`handleClick`，然后启动了一个定时器，每1000ms让`count`的值+1。
然后我们观察一下打印的`count`和页面渲染的`count`值是如何变化的？你猜猜结果是什么？
![1724492745969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724492745969.png)

结果是打印的一直是0，页面输出的一直是1。为什么会是这样呢？不应该是打印和页面输出的都是count++以后的值吗？

### **案情分析**

我们来分析一下原因。
你这段代码中，`count` 的打印值一直不变化是因为 `setInterval` 内部的 `count` 值是在首次调用 `handleClick` 时被捕获的，而不会随着状态更新而改变。这是因为 JavaScript 闭包的特性。

- 当你首次点击按钮时，`handleClick` 函数被调用，`setInterval` 开始执行。
- `setInterval` 内部的 `count` 值是 `handleClick` 被调用时的值（初始值为 0），而且由于闭包的关系，这个 `count` 值不会随着组件的重新渲染而更新。
- 所以，虽然 `setCount` 会更新 `count` 的状态，但是 `setInterval` 内部依然引用的是最初的 `count` 值，并且这个值不会随着状态变化而更新。

知道了缘由，如何解决上述问题呢？

### **结案**

我们可以使用函数形式的 `setCount` 来解决这个问题。

> 大家可以参考react官方示例函数形式的用法：https://react.dev/reference/react/useState#examples-updater

这样修改以后，函数形式的 `setCount` 可以获取到最新的状态值：

```
const handleClick = () => {
  setInterval(() => {
    setCount(prevCount => {
      console.log(prevCount + 1);
      return prevCount + 1;
    });
  }, 1000);
};
```

- `setCount` 的函数形式会接收当前状态的前一个值 `prevCount`，并返回更新后的值。
- 这样，`count` 的值在每次更新时都会递增并且正确输出。

通过这种方式，`console.log(prevCount + 1)` 将每秒打印出更新后的值，并且页面渲染的count也是更新后的值。

![1724492792428](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724492792428.png)

> 源代码地址：https://stackblitz.com/edit/vitejs-vite-nsxni1?file=src%2FApp.tsx



## **其他作案现场**

在其他的一些闭包使用场景中，闭包陷阱经常存在，由于变量的作用域、生命周期等原因，导致代码行为与预期不一致的情况。

### **1. \**循环中的闭包\****

在循环中创建闭包，闭包中的变量总是引用循环中的最后一个值，而不是当时循环的值。

```
function createButtons() {
  const buttons = [];
  for (var i = 0; i < 5; i++) {
    buttons.push(function() {
      console.log('Button', i);
    });
  }
  return buttons;
}

const buttons = createButtons();
buttons[0](); // 打印 5，而不是 0
buttons[1](); // 打印 5，而不是 1
```

在上述代码中，`i` 是使用 `var` 声明的，它在整个函数作用域内是共享的。当循环结束时，`i` 的值变为 5，闭包引用的也是这个值。因此，无论点击哪个按钮，都会打印 5。
解决方案：使用 `let` 替代 `var`，或者使用立即执行函数（IIFE）。

```
// 使用let
function createButtons() {
  const buttons = [];
  for (let i = 0; i < 5; i++) {
    buttons.push(function() {
      console.log('Button', i);
    });
  }
  return buttons;
}


// 使用 IIFE：
function createButtons() {
  const buttons = [];
  for (var i = 0; i < 5; i++) {
    (function(i) {
      buttons.push(function() {
        console.log('Button', i);
      });
    })(i);
  }
  return buttons;
}
```



### **2. \**异步操作中的闭包\****

场景：在异步操作中使用闭包时，闭包捕获的变量值可能与预期不同，导致错误的结果。
示例代码：

```
function fetchData() {
  for (var i = 0; i < 3; i++) {
    setTimeout(function() {
      console.log('Fetching data for index:', i);
    }, 1000);
  }
}

fetchData();
// 1秒后连续打印：Fetching data for index: 3
//               Fetching data for index: 3
//               Fetching data for index: 3
```

原因：因为 `i` 是用 `var` 声明的，所以它在每次循环中是同一个变量。在 `setTimeout` 执行时，`i` 的值已经是 3。
解决方案：同样可以使用 `let` 或 IIFE。
使用 `let`：

```
function fetchData() {
  for (let i = 0; i < 3; i++) {
    setTimeout(function() {
      console.log('Fetching data for index:', i);
    }, 1000);
  }
}
```



### **3. \**函数返回值中的闭包\****

场景：当一个函数返回一个闭包时，该闭包引用的外部变量在函数调用后仍然存在。
示例代码：

```
function createCounter() {
  let count = 0;
  return function() {
    count++;
    console.log('Count:', count);
  };
}

const counter1 = createCounter();
const counter2 = createCounter();

counter1(); // Count: 1
counter1(); // Count: 2
counter2(); // Count: 1
```

原因：`count` 变量在 `createCounter` 函数作用域内，但闭包（返回的函数）仍然持有对 `count` 的引用。因此，每次调用 `counter1` 或 `counter2`，它们都会更新并打印各自的 `count` 值。
解决方案：这是闭包的典型用法，不需要改变。但需小心管理好这些引用的生命周期，以防止意外内存泄漏或难以追踪的状态。

### **4. \**嵌套函数中的闭包\****

场景：嵌套函数中的闭包可能会导致变量意外共享，产生错误结果。
示例代码：

```
function outer() {
  let count = 0;
  
  function inner() {
    console.log(count);
  }
  
  count++;
  return inner;
}

const fn = outer();
fn(); // 打印 1
```

原因：`inner` 函数持有对 `outer` 函数作用域中 `count` 变量的引用。当 `outer` 函数返回后，`count` 的值仍然保存在 `inner` 的闭包中。

## **总结**

闭包是 JavaScript 中强大且常用的特性，但也容易导致陷阱。常见的闭包陷阱通常与变量的作用域、生命周期以及 JavaScript 的异步行为相关。理解这些场景并使用合适的方式（如 `let`、IIFE、函数式编程）来规避陷阱，可以帮助你写出更健壮的代码。