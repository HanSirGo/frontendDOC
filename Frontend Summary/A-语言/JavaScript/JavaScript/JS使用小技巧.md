## 空值合并运算符

空值合并运算符（`??`）用于为 `null` 或 `undefined` 的变量提供默认值。

传统写法：

```js
const fetchUserData = () => {
	return '前端充电宝';
};

const data = fetchUserData();
const username = data !== null && data !== undefined ? data : 'Guest';
```

简化写法：

```js
const fetchUserData = () => {
	return '前端充电宝';
};

const data = fetchUserData();
const username = data ?? 'CUGGZ';
```

除此之外，还有一个空位合并赋值运算符（`??=`），用于在变量为空（`null` 或 `undefined`）时进行赋值操作。

传统写法：

```js
let variable1 = null;
let variable2 = "前端充电宝";

if (variable1 === null || variable1 === undefined) {
  variable1 = variable2;
}
```

简化写法：

```js
let variable1 = null;
let variable2 = "前端充电宝";

variable1 ??= variable2;
```

`??=` 的写法更加简洁和易读。它首先检查变量 `variable1` 是否为 `null` 或 `undefined`，如果是，则将它赋值为 `variable2` 的值。如果 `variable1` 已经有一个非空的值，那么赋值操作就不会发生。

## 逻辑或赋值运算符

逻辑或赋值运算符（`||=`）用于为变量分配默认值。

传统写法：

```js
let count;
if (!count) {
  count = 0;
}
```

简化写法：

```js
let count;
count ||= 0;
```

当 `count` 为假值（例如 undefined、null、false、0、NaN 或空字符串）时，逻辑或赋值运算符将 `count` 赋值为 0。否则，它会保留 `count` 的原始值。

## 多值匹配

对于多个值的匹配，可以将所有的值放入一个数组中，然后使用 `indexOf(`) 方法进行检查。`indexOf() `方法是 JavaScript 数组的一个内置方法，它用于返回指定元素在数组中第一次出现的位置索引。如果数组中不存在该元素，则返回 -1。

传统写法：

```js
if (value === 1 || value === 'one' || value === 2 || value === 'two') {
  // ...
}
```

简化写法：

```js
if ([1, 'one', 2, 'two'].indexOf(value) >= 0) {
   // ...
}
```

更简化写法：

```js
if ([1, 'one', 2, 'two'].includes(value)) { 
    // ...
}
```

## 三元表达式

使用三元表达式表示可以简化`if...else`。

传统写法：

```js
let isAdmin;
if (user.role === 'admin') {
  isAdmin = true;
} else {
  isAdmin = false;
}
```

简化写法：

```js
const isAdmin = user.role === 'admin' ? true : false;
```

更简化写法：

```js
const isAdmin = user.role === 'admin';
```

## 短路求值

当将一个变量的值赋给另一个变量时，可能希望确保源变量不为 null、undefined 或空。可以编写一个包含多个条件的长 if 语句，或者使用短路求值来简化。

```js
if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
    let variable2 = variable1;
}
```

使用短路求值简化后的代码如下：

```js
const variable2 = variable1 || 'new';
```

对于逻辑或（||）操作符，以下值被视为假：

- false
- 0
- 空字符串（"" 或 ''）
- null
- undefined
- NaN

所以，如果本身的值可能就是这些中的一个，就不适合使用短路求值。

短路求值还能在函数调用中避免不必要的函数执行。

传统写法：

```js
function fetchData() {
  if (shouldFetchData) {
    return fetchDataFromAPI();
  } else {
    return null;
  }
}
```

简化写法：

```js
function fetchData() {
  return shouldFetchData && fetchDataFromAPI();
}
```

当 `shouldFetchData` 为真值时，短路求值会继续执行 `fetchDataFromAPI()` 函数并返回其结果。如果 `shouldFetchData` 为假值，短路求值会直接返回假值（`null`），避免了不必要的函数调用。

## 科学计数法

可以使用科学技术法来表示数字，以省略尾部的零。例如，`1e7` 实际上表示 1 后面跟着 7 个零。它表示一个十进制，相当于 10,000,000。

传统写法：

```js
for (let i = 0; i < 10000; i++) {}
```

简化写法：

```js
for (let i = 0; i < 1e7; i++) {}

// 下面的所有比较都将返回 true
1e0 === 1;
1e1 === 10;
1e2 === 100;
1e3 === 1000;
1e4 === 10000;
1e5 === 100000;
```

## 位运算符

双位非运算符有一个非常实用的用途，可以用它来替代`Math.floor()`函数，它在执行相同的操作时速度更快。

传统写法：

```js
Math.floor(4.9) === 4  //true
```

简化写法：

```js
~~4.9 === 4  //true
```

## 指数幂运算

指数幂运算可以使用  * 来简化。

传统写法：

```js
Math.pow(2,3); // 8
Math.pow(2,2); // 4
Math.pow(4,3); // 64
```

简化写法：

```js
2**3 // 8
2**4 // 4
4**3 // 64
```

从 ES7（ECMAScript 2016）开始，JavaScript 引入了指数幂运算符 `**`，使指数幂运算更加简洁。

## 双非未运算符

在 JavaScript 中，双非位运算符 `~~` 可以用于将数字向下取整，类似于 `Math.floor()` 方法的功能。

传统写法：

```js
const floor = Math.floor(6.8); // 6
```

简化写法：

```js
const floor = ~~6.8; // 6
```

注意：双非位运算符只适用于 32 位整数，即范围为 -(2^31) 到 (2^31)-1，即 -2147483648 到 2147483647。对于大于 2147483647 或小于 0 的数字，双非位运算符（`~~`）会给出错误的结果，因此建议在这种情况下使用 `Math.floor()` 方法。

## 对象属性

ES6 提供了一种更简洁的方式来给对象赋值属性。如果变量名与对象的键名相同，可以利用简写符号来进行赋值。

传统写法：

```js
const name = '微信公众号：前端充电宝';
const age = 18;

const person = {
  name: name,
  age: age
};
```

简化写法：

```js
const name = '微信公众号：前端充电宝';
const age = 30;

const person = {
  name,
  age
};
```

## 箭头函数

箭头函数可以简化经典函数的写法。

传统写法：

```js
function sayHello(name) {
  console.log('Hello', name);
}

setTimeout(function() {
  console.log('Loaded')
}, 2000);

list.forEach(function(item) {
  console.log(item);
});
```

简化写法：

```js
sayHello = name => console.log('Hello', name);

setTimeout(() => console.log('Loaded'), 2000);

list.forEach(item => console.log(item));
```

如果箭头函数只有一条语句，它会隐式地返回其求值的结果，这时可以省略 `return` 关键字：

传统写法：

```js
function calcCircumference(diameter) {
  return Math.PI * diameter
}

```

简化写法： 

```js
calcCircumference = diameter => (
  Math.PI * diameter;
)
```

## 参数解构

如果正在使用一些流行的 Web 框架，比如 React、Vue，可能会使用数组或对象字面量形式的数据来在组件之间传递信息。在组件中要想使用数据对象，就需要对其进行解构。

传统写法：

```js
const observable = require('mobx/observable');
const action = require('mobx/action');
const runInAction = require('mobx/runInAction');

const store = this.props.store;
const form = this.props.form;
const loading = this.props.loading;
const errors = this.props.errors;
const entity = this.props.entity;
```

简化写法：

```js
import { observable, action, runInAction } from 'mobx';

const { store, form, loading, errors, entity } = this.props;
```

还可以为变量赋予新的变量名：

```js
const { store, form, loading, errors, entity:contact } = this.props;
```

## 扩展运算符

在ES6中引入的扩展运算符可以简化数组和对象的一些操作。

传统写法：

```js
// 合并数组
const odd = [1, 3, 5];
const nums = [2, 4, 6].concat(odd);
// 克隆数组
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()
```

简化写法：

```js
// 合并数组
const odd = [1, 3, 5];
const nums = [2, 4, 6].concat(odd);
// 克隆数组
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()
```

与 `concat()` 函数不同，可以使用扩展运算符在另一个数组的任意位置插入一个数组。

```js
const odd = [1, 3, 5];
const nums = [2, ...odd, 4, 6];
```

还可以将扩展运算符与ES6的解构语法结合使用：

```js
const { a, b, ...z } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a) // 1
console.log(b) // 2
console.log(z) // { c: 3, d: 4 }
```

扩展运算符还能用于合并对象：

传统写法：

```js
let fname = { firstName : '前端' };
let lname = { lastName : '充电宝'}
let full_names = Object.assign(fname, lname);
```

简化写法：

```js
let full_names = {...fname, ...lname};
```

## 强制参数

在传统的 JavaScript 写法中，为了确保函数参数被传入一个有效值，我们需要使用条件语句来抛出一个错误。可以通过使用强制参数简写的写法实现相同的逻辑。

传统写法：

```js
function foo(bar) {
  if(bar === undefined) {
    throw new Error('Missing parameter!');
  }
  return bar;
}
```

简化写法：

```js
function foo(bar) {
  if(bar === undefined) {
    throw new Error('Missing parameter!');
  }
  return bar;
}
```

这里定义了一个名为 `mandatory` 的函数，用于抛出一个错误，表示函数参数未被传入。然后，在函数 `foo` 的参数列表中，使用赋默认值的方式来将 `bar` 参数设置为 `mandatory()` 的调用结果，如果 `bar` 参数未被传入或者传入了假值，就会触发 `mandatory()` 函数的执行。

## 转为布尔值

可以使用双重逻辑非操作符将任何值转换为布尔值。

```js
!!23 // TRUE
!!"" // FALSE
!!0 // FALSE
!!{} // TRUE
```

单一的逻辑非操作符已经可以将值转换为布尔类型并对其进行取反，所以第二个逻辑非操作符会再次对其进行取反，从而将其恢复为原始含义，并保持为布尔类型。

## 变量交换

可以使用数组解构来轻松实现变量交换。

传统写法（使用临时变量完成两个变量的交换）：

```js
let a = 5;
let b = 10;

const temp = a;
a = b;
b = temp;
```

简化写法（使用数组解构赋值完成两个变量交换）：

```js
let a = 5;
let b = 10;

[a, b] = [b, a];
```

这里创建了一个包含两个元素的数组 `[b, a]`，然后使用数组解构赋值将其中的值分别赋给变量 `a` 和 `b`。由于左侧的数组和右侧的数组结构相同，所以两个值会进行交换。

## 变量声明

当需要同时声明多个变量时，可以使用变量声明的简写方法来节省时间和空间。

传统写法：

```js
let x;
let y;
let z = 3;
```

简化写法：

```js
let x, y, z = 3;
```

不过，这个优化有些争议，很多人认为这么写会影响代码的可读性，因为许多变量写到了一行，不如一个变量一行更清晰明了，所以可以选择性采用。

如果有多个变量需要赋相同的值，则可以使用连等来实现。

传统写法：

```js
let a = 100;
let b = 100;
let c = 100;
```

简化写法：

```js
let a = b = c = 100;
```

## For 循环

JavaScript 中传统的 for 循环语法使用数组的长度作为迭代器来遍历数组。还有很多 for 循环的快捷方式提供了在数组中迭代对象的不同方法，例如：

- `for...of`：用于遍历内置字符串、数组、类数组对象、NodeList。
- `for...in`：用于访问数组的索引和对对象字面量进行遍历，并记录属性名称和值的字符串。
- `Array.forEach`：使用回调函数对数组元素及其索引执行操作。

传统写法：

```js
for (let i = 0; i < arr.length; i++) {
  console.log("item: ", arr[i]);}
}
```

简化写法：

```js
for (let str of arr) {
  console.log("item: ", str);
}

arr.forEach((str) => {
  console.log("item: ", str);
});

for (let index in arr) {
  console.log(index, arr[index]);
}
```

对于对象字面量，也可以使用 `for...in` 来遍历:

```js
const obj = { a: 1, b: 3, c: 5 };

for (let key in obj) {
  console.log(key, obj[key]);
}
```

## **变量交换**

```
let a = 1,
  b = 2;

[a, b] = [b, a];

// 输出：a = 2，b = 1
```

## **对象解构，数据访问更便捷**

```
const { name, age } = { name: "Riki", age: 22 };

// 输出：name = 'Riki', age = 22
```

## **克隆对象**

> 当然这里是浅拷贝

```
const Obj = { name: "Riki", age: 22 };
const clonedObj = { ...Obj };

// 输出：clonedObj = {name: 'Riki', age: 22}
```

## **合并对象**

```
const obj1 = { name: "Riki" };
const obj2 = { age: 22 };

const mergedObj = { ...obj1, ...obj2 };

// 输出：mergedObj = {name: 'Riki', age: 22}
```

## **清理数组中的空值**

```
const arr = [0, 1, false, 2, "", 3];

const cleanedArray = arr.filter(Boolean);

// 输出：cleanedArray = [1, 2, 3]
```

## **将 NodeList 转换为数组**

```
const nodesArray = [...document.querySelectorAll("div")];
// 或者
const nodesArray = Array.form(document.querySelectorAll("div"));
```

## **检查数组是否符合特定条件**

```
const arr = [1, 2, 3, -1, 4];

// 检查数组中是否有一项符合预期
const hasNegativeNumbers = arr.some((num) => num < 0);
// 输出：hasNegativeNumbers = true

// 检查数组中是否所有项符合预期
const allPositive = arr.every((num) => num > 0);
//输出：allPositive = false
```

## **将文本复制到剪贴板**

```
navigator.clipboard.writeText("要复制的文本");
```

## **数组去重复**

```
const arr = [1, 2, 2, 3, 4, 4, 5];
const unique = [...new Set(arr)];
```

## **查找数组的交集**

```
const arr1 = [1, 2, 3, 4];
const arr2 = [2, 4, 6, 8];

const intersection = arr1.filter((value) => arr2.includes(value));

// 输出：intersection = [2, 4]
```

## **数组值的总和**

```
const arr = [1, 2, 3, 4];

const sum = arr.reduce((total, value) => total + value, 0);
// 输出：sum = 10
```

## **条件对象属性**

```
const condition = true;
const value = "Hello World";

const newObject = { ...(condition && { key: value }) };
// 输出：newObject = { key: 'Hello World' }
```

## **动态对象属性**

```
const dynamicKey = "name";
const value = "Riki Doe";

const obj = { [dynamicKey]: value };
// 输出：obj = { name: 'Riki Doe' }
```

## **在线状态检查器**

```
const isOnline = navigator.onLine ? "Online" : "Offline";
// 输出：isOnline = 'Online'
```

## **离开页面前确认**

```
window.onbeforeunload = () => "Are you sure you want to leave?";
```

## **按键计算对象值之和**

```
const arrayOfObjects = [{ x: 1 }, { x: 2 }, { x: 3 }];

const sumBy = (arr, key) => arr.reduce((acc, obj) => acc + obj[key], 0);

sumBy(arrayOfObjects, "x");

// 输出：6
```

## **将 query string 解析为对象**

```
const query = "name=Riki&age=30";

const parseQuery = (query) =>Object.fromEntries(new URLSearchParams(query));

// 输出：parseQuery = { name: 'Riki', age: '30' }
```

## **将秒转换为时间字符串**

```
const seconds = 3661;

const toTimeString = (seconds) =>newDate(seconds * 1000).toISOString().substr(11, 8);

toTimeString(seconds);
// 输出：'01:01:01'
```

## **对象中的最大值**

```
const scores = { math: 95, science: 99, english: 88 };

const maxObjectValue = (obj) =>Math.max(...Object.values(obj));

maxObjectValue(scores);
// 输出：99
```

## **检查对象是否包含值**

```
const person = { name: "John", age: 30 };

const hasValue = (obj, value) =>Object.values(obj).includes(value);

hasValue(person, 30);
// 输出：true
```

## **有条件的修改项**

```
const scores = [45, 75, 62, 55, 90];

const updatedScores = scores.map((score) => (score < 60 ? score + 20 : score));
// 输出：updatedScores = [65, 75, 62, 75, 90]
```

## **生成数组**

```
const range = Array.from({ length: 5 }, (_, i) => i + 1);
// 输出：range = [1, 2, 3, 4, 5]
```

## **超时请求**

```
const timeout = (promise, ms) =>Promise.race([promise, newPromise((_, reject) => setTimeout(() => reject(newError("Timeout")), ms))]);

timeout(fetch("https://api.riki.wang"), 5000)
  .then(() =>"handleResponse")
  .catch(() =>"handleError");

// 如果在指定的毫秒数内未返回，则拒绝并显示“超时”错误
```

## **提取文件扩展名**

```
const fileName = "example.png";

const getFileExtension = (str) => str.slice(((str.lastIndexOf(".") - 1) >>> 0) + 2);
// 输出：getFileExtension = 'png'
```

## **检查 tab 是否激活**

```
const isTabFocused = () =>document.hasFocus();
```

## **切换元素的类名**

```
const element = document.querySelector(".my-element");

const toggleClass = (el, className) => el.classList.toggle(className);

toggleClass(element, "active");
```