##  ECMAScript 是什么？

```
ECMAScript 是编写脚本语言的标准，这意味着JavaScript遵循ECMAScript标准中的规范变化，因为它是JavaScript的蓝图。

ECMAScript 和 Javascript，本质上都跟一门语言有关，一个是语言本身的名字，一个是语言的约束条件 只不过发明JavaScript的那个人（Netscape公司），把东西交给了ECMA（European Computer Manufacturers Association），这个人规定一下他的标准，因为当时有java语言了，又想强调这个东西是让ECMA这个人定的规则，所以就这样一个神奇的东西诞生了，这个东西的名称就叫做ECMAScript。

javaScript = ECMAScript + DOM + BOM（自认为是一种广义的JavaScript）

ECMAScript说什么JavaScript就得做什么！

JavaScript（狭义的JavaScript）做什么都要问问ECMAScript我能不能这样干！如果不能我就错了！能我就是对的！
```

### ECMAScript 2015（ES6）新特性

> - 块作用域
> - 类
> - 箭头函数
> - 模板字符串
> - 加强的对象字面
> - 对象解构
> - Promise
> - 模块
> - Symbol
> - 代理（proxy）Set
> - 函数默认参数
> - rest 和展开

### `var`,`let`和`const`

```js
// 1. var声明的变量会挂载在window上，而let和const声明的变量不会：

var a = 100;
console.log(a,window.a);    // 100 100

let b = 10;
console.log(b,window.b);    // 10 undefined

const c = 1;
console.log(c,window.c);    // 1 undefined
```

```js
// 2. var声明变量存在变量提升，let和const不存在变量提升:

console.log(a); // undefined  ===>  a已声明还没赋值，默认得到undefined值
var a = 100;

console.log(b); // 报错：b is not defined  ===> 找不到b这个变量
let b = 10;

console.log(c); // 报错：c is not defined  ===> 找不到c这个变量
const c = 10;
```

```js
// 3. let和const声明形成块作用域


if(1){
  var a = 100;
  let b = 10;
}

console.log(a); // 100
console.log(b)  // 报错：b is not defined  ===> 找不到b这个变量

-------------------------------------------------------------

if(1){
  var a = 100;
  const c = 1;
}
console.log(a); // 100
console.log(c)  // 报错：c is not defined  ===> 找不到c这个变量
```

```js
// 4. 同一作用域下let和const不能声明同名变量，而var可以

var a = 100;
console.log(a); // 100

var a = 10;
console.log(a); // 10
-------------------------------------
let a = 100;
let a = 10;

//  控制台报错：Identifier 'a' has already been declared  ===> 标识符a已经被声明了。
```

```js
// 5. 暂存死区

var a = 100;

if(1){
    a = 10;
    //在当前块作用域中存在a使用let/const声明的情况下，给a赋值10时，只会在当前作用域找变量a，
    // 而这时，还未到声明时候，所以控制台Error:a is not defined
    let a = 1;
}
```

#### const

```js
/*
*   1、一旦声明必须赋值,不能使用null占位。
*
*   2、声明后不能再修改
*
*   3、如果声明的是复合类型数据，可以修改其属性
*
* */

const a = 100; 

const list = [];
list[0] = 10;
console.log(list);  // [10]

const obj = {a:100};
obj.name = 'apple';
obj.a = 10000;
console.log(obj);  // {a:10000,name:'apple'}

```

### 箭头函数

> 箭头函数表达式的语法比函数表达式更简洁，并且没有自己的`this，arguments，super或new.target`。箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

```js
//ES5 Version
var getCurrentDate = function (){
  return new Date();
}

//ES6 Version
const getCurrentDate = () => new Date();
```

```js
// 在本例中，ES5 版本中有function(){}声明和return关键字，这两个关键字分别是创建函数和返回值所需要的。在箭头函数版本中，我们只需要()括号，不需要 return 语句，因为如果我们只有一个表达式或值需要返回，箭头函数就会有一个隐式的返回。
```

```js
//ES5 Version
function greet(name) {
  return 'Hello ' + name + '!';
}

//ES6 Version
const greet = (name) => `Hello ${name}`;
const greet2 = name => `Hello ${name}`;
```

我们还可以在箭头函数中使用与函数表达式和函数声明相同的参数。如果我们在一个箭头函数中有一个参数，则可以省略括号。

```js
const getArgs = () => arguments

const getArgs2 = (...rest) => rest
```

箭头函数不能访问arguments对象。所以调用第一个getArgs函数会抛出一个错误。相反，我们可以使用rest参数来获得在箭头函数中传递的所有参数。

```js
const data = {
  result: 0,
  nums: [1, 2, 3, 4, 5],
  computeResult() {
    // 这里的“this”指的是“data”对象
    const addAll = () => {
      return this.nums.reduce((total, cur) => total + cur, 0)
    };
    this.result = addAll();
  }
};
```

箭头函数没有自己的this值。它捕获词法作用域函数的this值，在此示例中，addAll函数将复制computeResult 方法中的this值，如果我们在全局作用域声明箭头函数，则this值为 window 对象。

### 类

> 类(class)是在 JS 中编写构造函数的新方法。它是使用构造函数的语法糖，在底层中使用仍然是原型和基于原型的继承。

```js
//ES5 Version
   function Person(firstName, lastName, age, address){
      this.firstName = firstName;
      this.lastName = lastName;
      this.age = age;
      this.address = address;
   }

   Person.self = function(){
     return this;
   }

   Person.prototype.toString = function(){
     return "[object Person]";
   }

   Person.prototype.getFullName = function (){
     return this.firstName + " " + this.lastName;
   }  

   //ES6 Version
   class Person {
        constructor(firstName, lastName, age, address){
            this.lastName = lastName;
            this.firstName = firstName;
            this.age = age;
            this.address = address;
        }

        static self() {
           return this;
        }

        toString(){
           return "[object Person]";
        }

        getFullName(){
           return `${this.firstName} ${this.lastName}`;
        }
   }
```

```js
// 重写方法并从另一个类继承。

//ES5 Version
Employee.prototype = Object.create(Person.prototype);

function Employee(firstName, lastName, age, address, jobTitle, yearStarted) {
  Person.call(this, firstName, lastName, age, address);
  this.jobTitle = jobTitle;
  this.yearStarted = yearStarted;
}

Employee.prototype.describe = function () {
  return `I am ${this.getFullName()} and I have a position of ${this.jobTitle} and I started at ${this.yearStarted}`;
}

Employee.prototype.toString = function () {
  return "[object Employee]";
}

//ES6 Version
class Employee extends Person { //Inherits from "Person" class
  constructor(firstName, lastName, age, address, jobTitle, yearStarted) {
    super(firstName, lastName, age, address);
    this.jobTitle = jobTitle;
    this.yearStarted = yearStarted;
  }

  describe() {
    return `I am ${this.getFullName()} and I have a position of ${this.jobTitle} and I started at ${this.yearStarted}`;
  }

  toString() { // Overriding the "toString" method of "Person"
    return "[object Employee]";
  }
}
```

```js
// 所以我们要怎么知道它在内部使用原型？

class Something {

}

function AnotherSomething(){

}
const as = new AnotherSomething();
const s = new Something();

console.log(typeof Something); // "function"
console.log(typeof AnotherSomething); // "function"
console.log(as.toString()); // "[object Object]"
console.log(as.toString()); // "[object Object]"
console.log(as.toString === Object.prototype.toString); // true
console.log(s.toString === Object.prototype.toString); // true
```

> 《ECMAScript 6 实现了 class，对 JavaScript 前端开发有什么意义？》
>
> 《Class 的基本语法》

### 模板字符串

> 模板字符串是在 JS 中创建字符串的一种新方法。我们可以通过使用反引号使模板字符串化。

```js
//ES5 Version
var greet = 'Hi I\'m Mark';

//ES6 Version
let greet = `Hi I'm Mark`;

// 在 ES5 中我们需要使用一些转义字符来达到多行的效果，在模板字符串不需要这么麻烦：

//ES5 Version
var lastWords = '\n'
  + '   I  \n'
  + '   Am  \n'
  + 'Iron Man \n';


//ES6 Version
let lastWords = `
    I
    Am
  Iron Man   
`;

// 在ES5版本中，我们需要添加\n以在字符串中添加新行。在模板字符串中，我们不需要这样做。

//ES5 Version
function greet(name) {
  return 'Hello ' + name + '!';
}


//ES6 Version
function greet(name) {
  return `Hello ${name} !`;
}

// 在 ES5 版本中，如果需要在字符串中添加表达式或值，则需要使用+运算符。在模板字符串s中，我们可以使用${expr}嵌入一个表达式，这使其比 ES5 版本更整洁。
```

### 对象解构

```js
// 对象析构是从对象或数组中获取或提取值的一种新的、更简洁的方法。假设有如下的对象：

const employee = {
  firstName: "Marko",
  lastName: "Polo",
  position: "Software Developer",
  yearHired: 2017
};

// 从对象获取属性，早期方法是创建一个与对象属性同名的变量。这种方法很麻烦，因为我们要为每个属性创建一个新变量。假设我们有一个大对象，它有很多属性和方法，用这种方法提取属性会很麻烦。
var firstName = employee.firstName;
var lastName = employee.lastName;
var position = employee.position;
var yearHired = employee.yearHired;
```

```js
// 使用解构方式语法就变得简洁多了：

{ firstName, lastName, position, yearHired } = employee;
```

```js
// 我们还可以为属性取别名：

let { firstName: fName, lastName: lName, position, yearHired } = employee;
```

```js
// 当然如果属性值为 undefined 时，我们还可以指定默认值：

let { firstName = "Mark", lastName: lName, position, yearHired } = employee;
```

### `Set`对象

```js
// Set 对象允许你存储任何类型的唯一值，无论是原始值或者是对象引用。

我们可以使用Set构造函数创建Set实例。

const set1 = new Set();
const set2 = new Set(["a","b","c","d","d","e"]);
```

```js
// 我们可以使用add方法向Set实例中添加一个新值，因为add方法返回Set对象，所以我们可以以链式的方式再次使用add。如果一个值已经存在于Set对象中，那么它将不再被添加。

set2.add("f");
set2.add("g").add("h").add("i").add("j").add("k").add("k");
// 后一个“k”不会被添加到set对象中，因为它已经存在了
```

```js
// 我们可以使用has方法检查Set实例中是否存在特定的值。

set2.has("a") // true
set2.has("z") // true

// 我们可以使用size属性获得Set实例的长度。

set2.size // returns 10

// 可以使用clear方法删除 Set 中的数据。

set2.clear();

//我们可以使用Set对象来删除数组中重复的元素。

const numbers = [1, 2, 3, 4, 5, 6, 6, 7, 8, 8, 5];
const uniqueNums = [...new Set(numbers)]; // [1,2,3,4,5,6,7,8]
```

> 另外还有`WeakSet`， 与 `Set` 类似，也是不重复的值的集合。但是 `WeakSet` 的成员只能是对象，而不能是其他类型的值。`WeakSet` 中的对象都是弱引用，即垃圾回收机制不考虑 `WeakSet`对该对象的引用。
>
> - Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
> - WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap 只接受对象作为键名（ null 除外），不接受其他类型的值作为键名。而且 WeakMap 的键名所指向的对象，不计入垃圾回收机制。

### Proxy

```
Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”，即对编程语言进行编程。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
```

