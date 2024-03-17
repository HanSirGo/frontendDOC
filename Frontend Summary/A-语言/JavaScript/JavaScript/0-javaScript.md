## JavaScript

#### javascript

```
单线程
解释性语言（相对的还有 编译性语言）
```

###### js单线程

```
JavaScript语言的一大特点就是单线程，即同一时间只能做一件事情。
```

> JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

###### 解释性语言&编译性语言

```
js需要翻译成计算机能读取的语言,这个翻译过程分为编译性语言和解释性语言;
编译性语言: 会将语言编译成另一个文件,交给计算机执行
	优点: 执行快
	缺点:	跨平台
解释性语言: 解释一行，计算机执行一行
	优点: 执行慢
	缺点:	不跨平台
```

###### 浏览器

```
组成:
	shell外核
	内核: 渲染引擎、js引擎
主流浏览器及内核
	ie:	trindent
	chrome: webkit/blink
	firfox: gecko
	opera: presto
	saferi: webkit
```

------



#### 逻辑赋值运算符

```
JavaScript 引入了逻辑赋值运算符（&&=、||= 和 ??=），以简洁的方式将逻辑操作与赋值结合起来。

let x = false;
x ||= true;

console.log(x); // Output: true
```



------

#### 闭包

> 闭包是指有权访问另一个函数作用域内变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以 访问到当前函数的局部变量。

```js
闭包有两个常用的用途。

1. 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。
2. 函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。
```

```js
function a(){
    var n = 0;
    function add(){
       n++;
       console.log(n);
    }
    return add;
}
var a1 = a(); //注意，函数名只是一个标识（指向函数的指针），而（）才是执行函数；
a1();    //1
a1();    //2  第二次调用n变量还在内存中


// 其实闭包的本质就是作用域链的一个特殊的应用，只要了解了作用域链的创建过程，就能够理解闭包的实现原理。
```

```
需要人为销毁（赋值 null）

闭包的优点是：
1.变量被保存起来没有被销毁，随时可以被调用
2.只有函数内部的子函数才能读取局部变量,可以避免全局污染
缺点是：
如果闭包使用不当，就会导致变量不会被垃圾回收机制回收，造成内存泄露
```

#### 垃圾回收机制

------



#### JSON的方法

```
1.JSON.parse()
2.JSON.stringify()
3.缺点
```

#### 时间获取

```js
js_date_time(dates) {
            var date = new Date(dates)
            var YY = date.getFullYear() + '-'
            var MM = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
            var DD = date.getDate() < 10 ? '0' + date.getDate() : date.getDate()
            var hh = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':'
            var mm = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
            var ss = date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds()
            return YY + MM + DD + ' ' + hh + mm + ss
        }
```

#### xss攻击

#### 拷贝

```
1.浅拷贝与深拷贝

浅拷贝是创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以如果其中一个对象改变了这个地址，就会影响到另一个对象。
深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且修改新对象不会影响原对象。

浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。

var a1 = {b: {c: {}};

var a2 = shallowClone(a1); // 浅拷贝方法
a2.b.c === a1.b.c // true 新旧对象还是共享同一块内存

var a3 = deepClone(a3); // 深拷贝方法
a3.b.c === a1.b.c // false 新对象跟原对象不共享内存


2.赋值和深/浅拷贝的区别 - (前提都是针对引用类型)

// 对象赋值
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj2 = obj1;
obj2.name = "阿浪";
obj2.arr[1] =[5,6,7] ;
console.log('obj1',obj1) // obj1 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }
console.log('obj2',obj2) // obj2 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

// 浅拷贝
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj3=shallowClone(obj1)
obj3.name = "阿浪";
obj3.arr[1] = [5,6,7] ; // 新旧对象还是共享同一块内存
// 这是个浅拷贝的方法
function shallowClone(source) {
    var target = {};
    for(var i in source) {
        if (source.hasOwnProperty(i)) {
            target[i] = source[i];
        }
    }
    return target;
}
console.log('obj1',obj1) // obj1 { name: '浪里行舟', arr: [ 1, [ 5, 6, 7 ], 4 ] }
console.log('obj3',obj3) // obj3 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

// 深拷贝
let obj1 = {
    name : '浪里行舟',
    arr : [1,[2,3],4],
};
let obj4=deepClone(obj1)
obj4.name = "阿浪";
obj4.arr[1] = [5,6,7] ; // 新对象跟原对象不共享内存
// 这是个深拷贝的方法
function deepClone(obj) {
    if (obj === null) return obj; 
    if (obj instanceof Date) return new Date(obj);
    if (obj instanceof RegExp) return new RegExp(obj);
    if (typeof obj !== "object") return obj;
    let cloneObj = new obj.constructor();
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        // 实现一个递归拷贝
        cloneObj[key] = deepClone(obj[key]);
      }
    }
    return cloneObj;
}
console.log('obj1',obj1) // obj1 { name: '浪里行舟', arr: [ 1, [ 2, 3 ], 4 ] }
console.log('obj4',obj4) // obj4 { name: '阿浪', arr: [ 1, [ 5, 6, 7 ], 4 ] }

3.浅拷贝的实现方式
(1)Object.assign()
Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。
let obj1 = { person: {name: "kobe", age: 41},sports:'basketball' };
let obj2 = Object.assign({}, obj1);
obj2.person.name = "wade";
obj2.sports = 'football'
console.log(obj1); // { person: { name: 'wade', age: 41 }, sports: 'basketball' }
(2)函数库lodash的_.clone方法
该函数库也有提供_.clone用来做 Shallow Copy,后面我们会再介绍利用这个库实现深拷贝。
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.clone(obj1);
console.log(obj1.b.f === obj2.b.f);// true
(3)展开运算符...
展开运算符是一个 es6 / es2015特性，它提供了一种非常方便的方式来执行浅拷贝，这与 Object.assign ()的功能相同。
let obj1 = { name: 'Kobe', address:{x:100,y:100}}
let obj2= {... obj1}
obj1.address.x = 200;
obj1.name = 'wade'
console.log('obj2',obj2) // obj2 { name: 'Kobe', address: { x: 200, y: 100 } 
(4)Array.prototype.concat()
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2 = arr.concat();    
arr2[2].username = 'wade';
console.log(arr); //[ 1, 3, { username: 'wade' } ]
(5)Array.prototype.slice()
let arr = [1, 3, {
    username: ' kobe'
    }];
let arr3 = arr.slice();
arr3[2].username = 'wade'
console.log(arr); // [ 1, 3, { username: 'wade' } ]
4.深拷贝的实现方式
(1)JSON.parse(JSON.stringify())
let arr = [1, 3, {
    username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)

①　缺陷
这也是利用JSON.stringify将对象转成JSON字符串，再用JSON.parse把字符串解析成对象，一去一来，新的对象产生了，而且对象会开辟新的栈，实现深拷贝。
这种方法虽然可以实现数组或对象深拷贝,但不能处理函数和正则，因为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数（变为null）了。
比如下面的例子：
let arr = [1, 3, {
    username: ' kobe'
},function(){}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)

(2)函数库lodash的_.cloneDeep方法
该函数库也有提供_.cloneDeep用来做 Deep Copy
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);// false

(3)jQuery.extend()方法
jquery 有提供一個$.extend可以用来做 Deep Copy
$.extend(deepCopy, target, object1, [objectN])//第一个参数为true,就是深拷贝
var $ = require('jquery');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = $.extend(true, {}, obj1);
console.log(obj1.b.f === obj2.b.f); // false
(4)手写递归方法
递归方法实现深度克隆原理：遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝。
有种特殊情况需注意就是对象存在循环引用的情况，即对象的属性直接的引用了自身的情况，解决循环引用问题，我们可以额外开辟一个存储空间，来存储当前对象和拷贝对象的对应关系，当需要拷贝当前对象时，先去存储空间中找，有没有拷贝过这个对象，如果有的话直接返回，如果没有的话继续拷贝，这样就巧妙化解的循环引用的问题。
function deepClone(obj, hash = new WeakMap()) {
  if (obj === null) return obj; // 如果是null或者undefined我就不进行拷贝操作
  if (obj instanceof Date) return new Date(obj);
  if (obj instanceof RegExp) return new RegExp(obj);
  // 可能是对象或者普通的值  如果是函数的话是不需要深拷贝
  if (typeof obj !== "object") return obj;
  // 是对象的话就要进行深拷贝
  if (hash.get(obj)) return hash.get(obj);
  let cloneObj = new obj.constructor();
  // 找到的是所属类原型上的constructor,而原型上的 constructor指向的是当前类本身
  hash.set(obj, cloneObj);
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      // 实现一个递归拷贝
      cloneObj[key] = deepClone(obj[key], hash);
    }
  }
  return cloneObj;
}
let obj = { name: 1, address: { x: 100 } };
obj.o = obj; // 对象存在循环引用的情况
let d = deepClone(obj);
obj.address.x = 200;
console.log(d);

深拷贝：递归函数
/**
 * This is just a simple version of deep copy
 * Has a lot of edge cases bug
 * If you want to use a perfect deep copy, use lodash's _.cloneDeep
 * @param {Object} source
 * @returns {Object}
 */
function deepClone(source) {
  if (!source && typeof source !== 'object') {
    throw new Error('error arguments', 'deepClone')
  }
  const targetObj = source.constructor === Array ? [] : {}
  Object.keys(source).forEach(keys => {
    if (source[keys] && typeof source[keys] === 'object') {
      targetObj[keys] = deepClone(source[keys])
    } else {
      targetObj[keys] = source[keys]
    }
  })
  return targetObj
}
```

#### 递归

```
1.递归
(1)缺陷
随着递归深度的增加，创建的堆栈越来越多，导致爆炸堆栈
(2)递归实现九九乘法表
function mul (num) {
If(num == 1) {
Console.log(‘1x1=1’)
} else {
mul(num - 1);
for(var i =1, str = ‘’; i <= num; i ++) {
str += i + ‘x’ + num + ‘=’ + i*num + ‘ ‘
}
Console.log(str)
}
}
mul(9)
2.逆递归（尾递归）
尾递归基于函数的尾调用每个级别的调用都直接返回函数的返回值以更新调用栈，而不创建新的调用栈。
了解尾递归之前，先了解一下尾调用。
尾调用是指一个函数里的最后一个动作是一个函数调用的情形：即这个调用的返回值直接被当前函数返回的情形。这种情形下该调用位置为尾位置。
function foo(data) {
    a(data);
    return b(data);
}   
这里的a(data)和b(data)都是函数调用，但是b(data)是函数返回前的最后运行的东西，所以也是所谓的尾位置。

若一个函数在尾位置调用本身（或是一个尾调用本身的其他函数等），则称这种情况为尾递归，是递归的一种特殊情形。而形式上只要是最后一个return语句返回的是一个完整函数，它就是尾递归。这里注意：尾调用不一定是递归调用，但是尾递归一定是尾调用。
阶乘：
常规的计算阶乘的方法可能是这样的：递归
funtion fac(int n) {
    if (n == 1) {
        return 1;
    }
    return fac(n-1) * n;
}

尾递归的算法是这样的：
function tailfac(int n,int sum) {
    if (n == 1) {
        return sum;
    }
    return fac(n-1, n * sum);
}

缺陷：存在的问题尾部递归优化很好，如果递归深度超过1000,则会报告错误
```

#### 0.1+0.2≠0.3

#### 上拉加载下拉刷新

```
上拉加载、下拉刷新 better-scroll
import BScroll from 'better-scroll'
mounted(){
	// 获取 ref 为wrapper的dom
	let wrapper = this.$refs.wrapper
	let scroll = new BScroll(wrapper,{
		//配置项
		//0:默认值，不触发事件
		//1:屏幕滑动超过一定时间后触发scroll事件
		//2:屏幕在滑动过程中触发scroll事件
		//3:屏幕滑动过程中和momentum滚动动画都会触发scroll事件
		//momentum滚动动画：我们用力滑动时，手放开后屏幕还在滑动
		probeType:1,
		//上拉页面，请求更多数据时，配置
		pullUpLoad:true,
		//解决 每项的点击事件不起作用的问题
		click:true
	})
	scroll.on('scroll',eve=>{})
	scroll.on('pullingUp',()=>{ 
		//'上拉加载' ,但只能执行一次
		//可以多次上拉加载
		setTimeout(()=>{
			scroll.finisPullUp()
		},2000)
	 })
}
```

#### 上传

```
接口：export function postInfoList(params: any) {
  console.log(params)
  return serveHttp.post({ url: `${Api.UploadImages}`, params })
}

上传：
handler: async (files: File[]) => {
              console.log(files)
              const formData = new FormData()
              // 'formFiles' 必须和后端 需求的一致，不然返回不出正确数据
              formData.append('formFiles', files[0])
              console.log(formData)
              // You can use any AJAX library you like
              let res = await postInfoList(formData)
              console.log(res)
            },

上传时的name与后端返回的name 一致时
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174351384.png" alt="1709174351384" style="zoom:50%;" />![1709174357269](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174357269.png)

```
上传时的name与后端返回的name 不一致时
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174452173.png" alt="1709174452173" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174467685.png" alt="1709174467685" style="zoom:50%;" />

#### 上传文件并获取内容

```
let reader = new FileReader();
        // readAsText第一个参数不是文件而是选择文件后得到的原生文件对象
        reader.readAsText(fileList._rawValue[0].originFileObj)
        reader.onload = function(e) {
          let data = e.target.result;
          // console.log(data)
          valueRef.value = data
          loading.value = false
          // createMessage.success('导入成功!');
        }
```

#### 读文件FileReader()用法

```
HTML5定义了FileReader作为文件API的重要成员用于读文件，根据W3C的定义，FileReaderr接口提供了读取文件的方法和包含读取 结果的事件模型。
FileReader的方法使用比较简单，可以按照以下步骤创建FileReader对象并调用其他的方法。
1.检测浏览器对FileReader的支持。
例：

```

![1709174730970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174730970.png)



```

2.调用FileReader对象的方法
FileReader的实例拥有四种方法，其中三个用于读取文件，用一个来中断读取，下面的表格列出了这些方法以及他们的参数和功能，需要注意的是，无论读取成功或着失败，方法并不会返回读取结果，这一结果存储在result中。
例：
```

![1709174765141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174765141.png)



```
readAsText：该方法有两个参数，其中第二个参数是文本的编码方式，默认值为 UTF­8。这个方法非常容易理解，将文件以文本方式 读取，读取的结果即是这个文本文件中的内容。
readAsBinaryString：该方法将文件读取为二进制字符串，通常我们将它传送到后端，后端可以通过这段字符串存储文件。
readAsDataURL：这是例子程序中用到的方法，该方法将文件读取为一段以 data: 开头的字符串，这段字符串的实质就是 Data URL， Data URL是一种将小文件直接嵌入文档的方案。这里的小文件通常是指图像与 html 等格式的文件。
3.处理事件
FileReader包含了一套完整的事件模型，用于捕获读取文件时的状态，下面的这个表格归纳了这些事件。
例：
```

![1709174794947](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174794947.png)



```
文件一旦开始读取，无论成功或失败，实例的result都会被填充。如果读取失败，则result的值问Null值，否则就是读取成功的结果，绝大多数的程序都会在成功读取文件的时候，抓取这个值。
例：
```

![1709174817623](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174817623.png)

```
下面通过一个上传图片预览和带进度条上传来展示FileReader的使用。
例：
```

![1709174841066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174841066.png)

```
如果要上传文件的类型，可以通过文件选择器获取文件对象并通过Type属性来检查文件类型。
例：
```

![1709174871008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709174871008.png)

```
不难发现这个检测是基于正则表达式的，因此可以进行各种复杂的匹配。
```

#### 函数防抖与节流

```js
函数防抖和函数节流：优化高频率执行js代码的一种手段，js中的一些事件如浏览器的resize、scroll，鼠标的mousemove、mouseover，input输入框的keypress等事件在触发时，会不断地调用绑定在事件上的回调函数，极大地浪费资源，降低前端性能。为了优化体验，需要对这类事件进行调用次数的限制。
﻿
1.函数防抖
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。
// 这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。
var timer; // 维护同一个timer
function debounce(fn, delay) {
    clearTimeout(timer);
    timer = setTimeout(function(){
        fn();
    }, delay);
}

在上面的代码中，会出现一个问题，var timer只能在setTimeout的父级作用域中，这样才是同一个timer，并且为了方便防抖函数的调用和回调函数fn的传参问题，我们应该用闭包来解决这些问题。
function debounce(fn, delay) {
    var timer; // 维护一个 timer
    return function () {
        var _this = this; // 取debounce执行作用域的this
        var args = arguments;
        // 如果此时存在定时器的话，则取消之前的定时器重新记时
        if (timer) {
            clearTimeout(timer);
        }
        timer = setTimeout(function () {
            fn.apply(_this, args); // 用apply指向调用debounce的对象，相当于_this.fn(args);
        }, delay);
    };
}
﻿
2.函数节流
每隔一段时间，只执行一次函数。
// 函数节流 是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。
function throttle(fn, delay) {
    var timer;
    return function () {
        var _this = this;
        var args = arguments;
        if (timer) {
            return;
        }
        timer = setTimeout(function () {
            fn.apply(_this, args);
            timer = null; // 在delay后执行完fn之后清空timer，此时timer为假，throttle触发可以进入计时器
        }, delay)
    }
}

函数节流的目的，是为了限制函数一段时间内只能执行一次。因此，定时器实现节流函数通过使用定时任务，延时方法执行。在延时的时间内，方法若被触发，则直接退出方法。从而，实现函数一段时间内只执行一次。
﻿
3.异同比较
相同点：
都可以通过使用 setTimeout 实现。
目的都是，降低回调执行频率。节省计算资源。
不同点：
函数防抖，在一段连续操作结束后，处理回调，利用clearTimeout 和 setTimeout实现。函数节流，在一段连续操作中，每一段时间只执行一次，频率较高的事件中使用来提高性能。
函数防抖关注一定时间连续触发的事件只在最后执行一次，而函数节流侧重于一段时间内只执行一次。
﻿
4.常见应用场景
函数防抖的应用场景
连续的事件，只需触发一次回调的场景有：
搜索框搜索输入。只需用户最后一次输入完，再发送请求
手机号、邮箱验证输入检测
窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。
函数节流的应用场景
间隔一段时间执行一次回调的场景有：
滚动加载，加载更多或滚到底部监听
谷歌搜索框，搜索联想功能
高频点击提交，表单重复提交
```

> 《轻松理解 JS 函数节流和函数防抖》
>
> 《JavaScript 事件节流和事件防抖》
>
> 《JS 的防抖与节流》

#### Eventloop

```
1.Eventloop
Eventloop即事件循环，是指浏览器或者node的一种解决javascript单线程运行时不会阻塞的一种机制，也是我们经常使用的异步的原理。
无论是同步还是异步任务，都是在主线程执行
2.宏任务（macrotask）与微任务（microtask）
JS代码中的异步任务可进一步分为宏任务（macrotask）与微任务（microtask）。 
宏任务包括：script代码、setTimeout、setInterval、I/O、UI render 
微任务包括：promise.then、Object.observe(已废弃)、MutationObserver
3.执行顺序
1.同步任务加入同步任务队列，放到主线程中执行，宏任务和微任务会被加入各自的队列中。 
2.当主线程执行完毕后，浏览器会先去清空微任务队列，依次取出微任务队列中的微任务执行，执行过程中如果产生新的微任务，则追加到当前微任务队列的末尾等待执行。 
3.当微任务队列清空后，浏览器会从宏任务队列中取出一个宏任务执行。
4.宏任务执行完毕再去清空微任务队列，微任务队列清空后再取出一个宏任务来执行。
5.如此反复，直至宏任务队列和微任务队列全部清空。
题1：
 console.log(1);
 setTimeout(() => {
   console.log(2);
   Promise.resolve().then(() => {
     console.log(3);
   });
 }, 0);
 new Promise((resolve, reject) => {
   console.log(4);
   resolve(5);
 }).then((data) => {
   console.log(data);
 });
 setTimeout(() => {
   console.log(6);
 }, 0);

答案是：1 4 5 2 3 6
题2：
 setTimeout(function(){
   console.log('1')
 });
 new Promise(function(resolve){
   console.log('2');
   resolve();
 }).then(function(){
   console.log('3')
 });
 console.log('4');

答案：2 4 3 1
Promise对象的resolve部分的代码是当前主线程/宏任务的一部分，并不是微任务，Promise对象的then和catch代码段才是微任务。因此最先输出的是2和4，然后才是微任务队列中的3，最后是宏任务中的1。

题3：
 async function async1() {
   console.log(1); 
   const result = await async2(); 
   console.log(3); 
 } 
 async function async2() { 
   console.log(2); 
 } 
 Promise.resolve().then(() => { 
   console.log(4); 
 }); 
 setTimeout(() => { 
   console.log(5); 
 }); 
 async1(); 
 console.log(6);

答案：1 2 6 4 3 5
该题需要注意的是，由于await方法返回的是一个Promise对象，因此await方法执行完毕后续的代码都应该归入微任务队列，因此**console.log(3)**应该被加入微任务队列等待执行。
题4：
 function sleep(time) {
   let startTime = new Date()
   while (new Date() - startTime < time) {}
   console.log('1s over')
 }
 setTimeout(() => {
   console.log('setTimeout - 1')
   setTimeout(() => {
     console.log('setTimeout - 1 - 1')
     sleep(1000)
   })
   new Promise(resolve => {
    console.log('setTimeout - 1 - resolve')
     resolve() 
   }).then(() => {
     console.log('setTimeout - 1 - then')
     new Promise(resolve => resolve()).then(() => {
         console.log('setTimeout - 1 - then - then')
     })
   })
   sleep(1000)
})
setTimeout(() => {
   console.log('setTimeout - 2')
   setTimeout(() => {
     console.log('setTimeout - 2 - 1')
     sleep(1000)
   })
   new Promise(resolve => resolve()).then(() => {
     console.log('setTimeout - 2 - then')
     new Promise(resolve => resolve()).then(() => {
         console.log('setTimeout - 2 - then - then')
    })
   })
   sleep(1000)
})

浏览器输出为：
setTimeout - 1
setTimeout - 1 - resolve
1s over
setTimeout - 1 - then
setTimeout - 1 - then - then 
setTimeout - 2
1s over
setTimeout - 2 - then
setTimeout - 2 - then - then
setTimeout - 1 - 1
1s over
setTimeout - 2 - 1
1s over
该题需要注意的是微任务执行过程中产生的新的微任务也是追加在当前微任务队列末尾等待执行。


需要注意的是：
宏任务队列和微任务队列都遵循先进先出原则，先被加入队列的任务优先被取出执行
Promise对象的resolve部分不是微任务，then和catch部分才是，即
new Promise((resolve,reject) => {
  console.log('同步');
  resolve(); 
}).then(() => {
  console.log('异步');
}) 
微任务执行过程中产生的新的微任务追加到当前微任务队列队尾等待本轮事件循环执行
await方法返回的是一个Promise对象，因此await方法执行完毕，后续代码都应归入微任务队列(可结合题4理解)
Node环境的事件循环与浏览器不完全一致
```

#### 跨页面通信

```

```

#### new 过程

> `new` 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。`new` 关键字会进行如下的操作：
>
> 1. 创建一个空的简单JavaScript对象（即{}）；
> 2. 链接该对象（即设置该对象的构造函数）到另一个对象 ；
> 3. 将步骤1新创建的对象作为this的上下文 ；
> 4. 如果该函数没有返回对象，则返回this。

```js
function Dog(name, color, age) {
  this.name = name;
  this.color = color;
  this.age = age;
}

Dog.prototype={
  getName: function() {
    return this.name
  }
}

var dog = new Dog('大黄', 'yellow', 3)
```

```js
// 我们来看最后一行带new关键字的代码，按照上述的1,2,3,4步来解析new背后的操作。

//第一步：创建一个简单空对象

var obj = {}

//第二步：链接该对象到另一个对象（原型链）
// 设置原型链
obj.__proto__ = Dog.prototype

// 第三步：将步骤1新创建的对象作为 this 的上下文
// this指向obj对象
Dog.apply(obj, ['大黄', 'yellow', 3])

// 第四步：如果该函数没有返回对象，则返回this
// 因为 Dog() 没有返回值，所以返回obj
var dog = obj
dog.getName() // '大黄'

// 要注意的是如果 Dog() 有 return 则返回 return的值

var rtnObj = {}
function Dog(name, color, age) {
  // ...
  //返回一个对象
  return rtnObj
}

var dog = new Dog('大黄', 'yellow', 3)
console.log(dog === rtnObj) // true
```

```js
//接下来我们将以上步骤封装成一个对象实例化方法，即模拟new的操作：

function objectFactory(){
    var obj = {};
    //取得该方法的第一个参数(并删除第一个参数)，该参数是构造函数
    var Constructor = [].shift.apply(arguments);
    //将新对象的内部属性__proto__指向构造函数的原型，这样新对象就可以访问原型中的属性和方法
    obj.__proto__ = Constructor.prototype;
    //取得构造函数的返回值
    var ret = Constructor.apply(obj, arguments);
    //如果返回值是一个对象就返回该对象，否则返回构造函数的一个实例对象
    return typeof ret === "object" ? ret : obj;
}
```

#### webworker

#### setTimeout、Promise、async/await 三者之间异步解决方案的区别？

#### JavaScript 继承的几种方式及优缺点？

#### fetch 是否可以共享 Cookie?两个 then 分别对应着什么？

#### async/await的优缺点



#### 闭包、垃圾回收和内存泄漏、数组方法、数组乱序, 数组扁平化、事件委托、事件监听、事件模型

#### 前端安全（CSRF、XSS）

