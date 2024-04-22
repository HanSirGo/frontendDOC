# 判断函数是否标记为async

```js
/**
 * 字节面试题
 * 判断函数是否标记为async
 */
function isAsyncFunction(func){}

isAsyncFunction(async () => {})

isAsyncFunction(() => {})
```

## 一、分析

我们看一下标记的async的函数和没有标记async的函数到底有什么样的区别，打印看一下。![1713688543221](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688543221.png)展开之后，你会发现区别，普通的函数，它的原型是function啊，这是大家都知道的，每一个函数它的原型是function,function的原型呢是Object，它是这么一条原型链。

但是标记了async的函数呢，你看一下，它的原型变了，变成了AsyncFunction形，而这个AsyncFunction，它的原型才是function，也就是说在原形链上，它在中间夹了一层AsyncFunction<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688558083.png" alt="1713688558083" style="zoom:50%;" />

所以说这个判断呢，好像还挺简单的，就判断这个函数它的原型，也就是判断这个函数它的constructor是不是AsyncFunction就完事了。

## 二、怎么判断是否为async function

我们打印看一下的constructor。它的构造函数，因为函数也是对象，每个对象都有产生它的构造函数，它这个构造函数是不是等于AsyncFunction。

```js
function isAsyncFunction(func){
    console.log(func.constructor === AsyncFunction);
}
isAsyncFunction(async () => {})

isAsyncFunction(() => {})
```

![1713688588263](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688588263.png)保存看一下，这个时候你会发现报错了，这个AsyncFunction，它是存在，但是它没有暴露给js，你在js里边是拿不到这个构造函数的。

### 1.Symbol.toStringTag

我们再一次去看一下它的对象结构，再看一下这个标记的AsyncFunction的函数，它里边还有个特点，它里边有一个特殊的符号属性，它的值呢是AsyncFunction，而普通函数呢，它没有这个符号属性![1713688643484](C:\文件\seec3\frontendDOC\frontendDOC\Frontend Summary\1713688643484.png)

那它会影响啥？它影响的是object toString这个方法，我们打印一下下面这个看一下结果。

```js
function isAsyncFunction(func){
  console.log(Object.prototype.toString.call(func));
}

isAsyncFunction(async () => {})

isAsyncFunction(() => {})
```

![1713688671846](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688671846.png)

会发现，它会影响这个async，而普通函数，打印出来是Function。

那为什么会这样呢，原因是由于标记了async的函数，它有一个知名符号叫做Symbol.toStringTag，而这个符号的值呢，是AsyncFunction，于是，在使用上面方式进行打印的时候，它会用Symbol.toStringTag这个符号的值替换掉Function，它就影响这个。![1713688691218](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713688691218.png)

### 2、判断方法实现

所以，我们使用上面方法进行判断。

```js
/**
 * 字节面试题
 * 判断函数是否标记为async
 */
function isAsyncFunction(func){
    const str = Object.prototype.toString.call(func)
    console.log( str === '[object AsyncFunction]');
}
// true
isAsyncFunction(async () => {})
// false
isAsyncFunction(() => {})
```

或者

```js
/**
 * 字节面试题
 * 判断函数是否标记为async
 */
function isAsyncFunction(func){
    console.log(func[Symbol.toStringTag] === 'AsyncFunction');
}
// true
isAsyncFunction(async () => {})
// false
isAsyncFunction(() => {})
```