# JavaScript 数组新增纯函数API！

## **前言**

在 JavaScript 的世界里，`数组`是处理数据集合的重要工具。

而`纯函数`，作为一种编程范式，它不改变输入数据，也不依赖外部状态，只通过输入参数决定输出结果。

这样的函数易于测试、易于理解，也更易于维护。

## **什么叫纯函数**

`纯函数`（Pure Function）是函数式编程中的一个基本概念。它具有以下两个主要特点：

1. **无副作用（No Side Effects）**：纯函数在执行过程中不会对外部环境产生影响，即不会改变外部状态，也不会产生外部可见的变化，如修改全局变量、修改输入参数、输出到控制台、抛出异常、进行 I/O 操作等。
2. **可预测性（Predictable）**：对于相同的输入，纯函数总是返回相同的输出。这意味着函数的行为不依赖于程序中的状态变化，每次调用时，只要输入参数相同，输出的结果也必然相同。

## **数组新增纯函数API**

在 `JavaScript` 中，许多数组方法都是纯函数，例如：`map`、`filter`、`slice` 等，它们不会改变原始数组，而是返回一个新的数组。

最近，JavaScript 为数组操作带来了几个新的纯函数 API，它们是`Array.toSorted()`、`Array.toReversed()`、`Array.toSpliced()`以及`Array.with()`。

这些新方法不仅保持了纯函数的特性，还提供了更多灵活的操作方式。

## **Array.toSorted()**

`Array.toSorted()`是`sort()`方法的纯函数版本。

它返回一个新数组，其元素按升序排列，而原数组保持不变。

这个方法接受一个可选的比较函数，允许你自定义排序逻辑。

```
const numbers = [5, 3, 2, 8, 1];
const sortedNumbers = numbers.toSorted();
console.log(sortedNumbers); // [1, 2, 3, 5, 8]
console.log(numbers); // [5, 3, 2, 8, 1] 原数组未改变
```

## **Array.toReversed()**

`Array.toReversed()`是`reverse()`方法的纯函数版本。

它返回一个新数组，其元素顺序与原数组相反，原数组同样保持不变。

```
const items = [1, 2, 3, 4];
const reversedItems = items.toReversed();
console.log(reversedItems); // [4, 3, 2, 1]
console.log(items);         // [1, 2, 3, 4] 原数组未改变
```

## **Array.toSpliced()**

`Array.toSpliced()`是`splice()`方法的纯函数版本。

它返回一个新数组，根据指定的索引和删除数量，以及可选的添加元素，来生成新数组。原数组同样不会受到影响。

```
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const newFruits = fruits.toSpliced(1, 1, "Lemon");
console.log(newFruits); // ["Banana", "Lemon", "Apple", "Mango"]
console.log(fruits);     // ["Banana", "Orange", "Apple", "Mango"] 原数组未改变
```

## **Array.with()**

`Array.with()`是一个新的纯函数，用于替换数组中指定索引处的元素。

它返回一个新数组，其中指定索引处的元素被替换为新值，而原数组保持不变。

```
const arr = [1, 2, 3, 4, 5];
const newArr = arr.with(2, 6);
console.log(newArr); // [1, 2, 6, 4, 5]
console.log(arr);    // [1, 2, 3, 4, 5] 原数组未改变
```

这些新的`纯函数 API` 不仅丰富了 JavaScript 的数组操作方法，还使得代码更加简洁和安全。

它们避免了修改原始数据，减少了因数据变更带来的`潜在错误`，是现代 JavaScript 开发中不可或缺的工具。

通过使用这些纯函数，我们可以编写出更加清晰、可维护的代码，同时保持数据的不变性，这是函数式编程的核心原则之一。

随着 JavaScript 语言的不断发展，我们可以期待更多这样的实用功能被引入，以帮助我们更高效地处理数据和解决问题