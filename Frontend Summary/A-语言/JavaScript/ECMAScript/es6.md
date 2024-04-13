## ECMAScript2015

![1712383274914](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712383274914.png)

#### javascript与ecmascript关系

```
javascript是基于ECMAscript规范的脚本语言,并在此基础上实现了自己的封装。

ECMAScript 也是一门脚本语言，缩写为ES，通常看做JavaScript的标准化规范。

但实际上JavaScript是ECMAScript的扩展语言，因为ECMAScript只提供了最基本的语法，通俗点说只是约定了我们的代码如何编写，比如定义变量和函数，循环和分支…它只是停留在语言层面，并不能用来完成我们应用中的实际功能开发。JavaScript实现了ECMAScript的语言标准，并且在这个基础之上做了一些扩展，使得我们可以在浏览器环境中操作DOM 和 BOM，在node环境中可以做读写文件之类的操作。

在浏览器环境中，JavaScript = ECMAScript + BOM + DOM ；

完整的JavaScript实现应该由以下三个部分组成：ECMAScript：核心** **DOM：文档对象模型** **BOM：浏览器对象模型

在node环境中，JavaScript = ECMAScript + Node APIs
```

#### 变量声明

```
let 声明变量 
const 声明常量 

const 常量的实质：是变量指向的那个内存地址不变，基本数据类型的值就是内存地址所以不能改变，而引用数据类型保存的是一个指针（地址），而值则在堆内存中，只要地址不变，值可以改变 

基础数据类型：按值访问；复制的是值 
引用数据类型：按引用地址访问；复制的是地址
```

##### let、const与var的区别： 

```
1. 只在块级作用域有效 
2. 不能重复声明 
3. 不存在变量提升 
4. const声明常量必须初始化。并且值不可改 
5. let const 声明的变量不是window对象的属性 
6. 变量提升：var声明的变量，由于浏览器机制将变量的声明都提升到当期作用域的最顶层 
7. 暂时性死区：在当前作用域内，使用let声明变量之前，该变量都是不可用的
```

####  解构赋值

```
解构赋值：本质就是赋值，左边一种结构，右边一种结构，左右一一对立
```

> **数组的解构赋值：由变量的位置决定 根据索引匹配**
>
> **对象的解构赋值：由属性决定  根据属性相同匹配**

```
其他数据的解构赋值：能当数组的用数组解构赋值，能看做对象的用对象的解构赋值

string 可以作为 对象和数组的 解构赋值

null indefined 不可以

number boolean有tostring方法可以作为对象来精选解构赋值

使用默认值：一个位置未赋值或者赋的值严格等于 undefined 情况下，默认值生效
```

#### 字符串的扩展

```
编码：

es5： let b = '\u4e00' 4e00是16进制，必须够4位，不够用0填充或{}包裹 ，\u 书unicode编码

例子： 字母a 10进制 97 16进制是 61

let str = '\u0061' 或者 '\u{61}'

es6:   \u 后必须是5位
```

##### 字符串的方法：

```
num.toString(16) 转 16进制字符转编码：
	es5 ： str.charCodeAt(索引).toString(16)
	es6 :  str.codePointAt(索引).toString(16)

编码转字符：
    es5 ： String.fromCharCode() 转为 4位编码
    es6 ： String.CodePoint()  转为 5位编码

判断字符串的开头结尾是否有指定值：
    str.startsWith()
    str.endsWith()

字符串的重复：
	str.repeat()  参数为数字，表示重复几次

字符串的自动补全：
	str.padStart(length,value) 开头补全 length长度;用value补全
	str.padEnd(length,value)  结尾补全

字符串去空格：
    str.trimStart() 开头
    str.trimEnd()  结尾

用指定字符串替换所有的匹配字符：
	str.replaceAll(匹配字符,value)

查询字符：返回布尔值  
	str.includes()
```

#### 模板字符串

```js
用反引号 ``

变量用 ${}  包裹

例子:
    let age = 18;
    let text = `your age ${age}`
```

#### 扩展运算符...

```
把数组，类数组展开成一系列用逗号隔开的值

复制数组  [...arr]

合并数组  [...arr1,...arr2]

与解构赋值结合  [a,b,...c] = [1,2,3,4,5,]  // a=1,b=2,c=[3,4,5]
```

#### 数组的扩展

##### 数组的方法：

```
es3:
	pop shift push unshift slice splice sort reverse join indexOf lastIndexOf concat

es5:
	map filter forEach every some reduce reduceRight
```

##### 静态方法：

```
Array.from() 将伪数组转为 数组  Array.from([1,2,3],item=>item+1)  [].slice.call() 

[...argments]

Array.isArray()  判断是不是数组  

Array.of()  将散列 装进 数组
```

##### **数组实例 方法**：

```js
1. fill() 填充 fill(填充的值，起始下标，结束下标(不包括))  填充的元素是对象时，填充的是引用地址，一项改变，每项都变

2. copywithin()  将指定位置的成员复制到其他位置
 copyWithin( 替换起始下标 ，指定起始下标 ， 结束下标 )

3. keys()  values()  entires() 遍历数组

4. find()  找到符合条件的第一个数组成员  arr.find(item => 条件)

5. findIndex() 找到符合条件的第一个元素 下标

6. includes()  数组是否包含 指定的值 includes(检测的值 ，从下边开始)

7. flat()  拉平 flat(Infinity)

8. flatMap()  只能拉平一层

[1,2,3].flatMap( (item,index) => [item,item*2] ) //[1,2,2,4,3,6]
```

##### 判断一个数组是数组

```js
Array.isArray()

arr instanceof Array

arr.constructor === Array

Array.prototype.isprototypeOf(arr)

Object.getPrototypeOf(arr) === Array.prototype

Object.prototype.toString.call(arr) === '[Object Array]'

Array.prototype.constructor.toString.call(arr) === '[Object Array]'
```

##### 数组去重

```js
1.**filter** 

arr.filter( (item,index,arr) => arr.indexOf(item) === index )

2.**Set**

[...new Set(arr)]

3.**reduce**

arr.reduce( (pre,item,index) => 
    if(!pre.includes(item)){ pre.push(item) } return pre
 , [] )

 4.**对象**

 let obj = {};
 arr.forEach( item => {
     if(obj[item]){
         obj[item]++;
     }else{
         obj[item]=1;
     }

 } )

```

##### 数组扁平化

```js
// **数组的扁平化： 将多维数组 展成一维数组**
1.arr.flat(Infinity)

2.let str = JSON.stringify(arr)
arr = str.replace(/(\[\])/g,'').split(',')

3. let str = JSON.stringify(arr)
str = str.replace(/(\[\])/g,'').split(',')
str = '['+str+']'
arr = JSON.parse(str)

4. function flatten(arr){
     return arr.reduce(pre,cur) => {
         return pre.concat(Array.isArray(cur)? flatten(cur):cur )
     }
 }

5.while(arr.some(item => Array.isArray(item) )){
     arr = [].concat(...arr)
 }

6. 递归

let result = [];

let fn = function (arr) { 
    for()let i=0;i<arr.length;i++){
        let item = arr[i];
        if(Array.isArray(arr[i])){
            fn(item)
        }else{
            result.push(item)
        }
    }
 }
```

#### 数值的扩展

```
二进制数值 ： 0b 或 0B 前缀  例如：0b101

八进制数值 ： 0o  0O

十六进制数值：0x 0X
```

##### 数值分隔符（下划线_）

```
1.没有指定间隔的位数，可以隔随意位添加 _      
2.不能在数值开头/结尾
3.不能连续写多个 _     
4.小数点前后不能加 _    
5.所有进制数值使用 _

let num = 1_00;  num === 100 ;// true
```

##### es6对Number的优化

```js
// 将全局的方法放在Number上
1.Number.isNaN() 判断是不是NaN ；参数必须是数值，否者返回 false
与 原来 的 isNaN相比，isNaN 会先把参数转为 数值

2.Number.isFinite() 检测一个数是不是 有限的 参数必须是数值

3.Number.parseInt() 转 整数   '123.45px' --> 123

4.Number.parseFloat() 转小数  '123.45px' --> 123.45
```

##### 新增方法

```js
1.Number.isInteger() 是不是整数

2.Number.EPSILON() 极小数，常量做误差处理 小于成功

3.Number.isSafeInteger() 是不是安全数

Number.MAX_SAFE_INTEGER 最大安全数

Number.MIN_SAFE_INTEGER 最小安全数

安全整数 ： -2^53 ~ 2^53 之间的都是安全整数 不包括 2个断点
```

#### 大整数 BigInt

```js
定义： 数值 + 后缀 n  例子:25n

typeof 100n  ==>  BinInt

作用：

1.解决数据精准度问题（安全整数）

     2n**53n ==>  2**53
     2n**53n+1n ==> 2**53+1

2.解决数据大时，无法表示的问题：

   2**1024 是计算机最大值 显示为 infinity 
   2n**1024n 可以正常显示
```

####  数学对象的扩展

##### es5

```
Math.max()
Math.min()

**放大镜边界问题**：偏移量：L 最大偏移量：W 取值 Math.max(0,Math.min(L,W)) 取代if判断

Math.ceil() 

Math.floor()

Math.round()

Math.random() 0-1随机数

Math.pow(a,b) a的b次方

Math.abs() 绝对值

Math.sqrt() 开平方
```

##### es6 扩展

```
Math.cbrt() 开立方

Math.trunc() 取整

Math.hypot() 所有参数平方和 的开平方

Math.sign() 判断正负数 负数 --> -1 整数 --> +1 0 --> 0  其他值 --> NaN
```

#### 函数的扩展

```
函数的默认值：es6允许函数的参数设置默认值

参数变量是默认值声明的，所以不能用let const再次声明

使用参数默认值时，函数不能有同名参数

带默认值的参数应该是函数的为参数

 

函数带默认值会形成 独立作用域 初始化后消失
```

##### rest参数

```js
获取函数的多余参数，并放入数组中

function fn(a,...arg){}  //arg 之后不能有其他参数，会报错

fn(1,2,3,4)  arg//[2,3,4]
```

#### 箭头函数

```js
let fn = () => {}
```

```js
特点：

1.this指向问题，箭头函数中的this指向定义时的对象，而不是指向使用时的对象

    document.ondblclick = () => console.log(this)  //window

2.箭头函数不能作为构造函数，所以不能用new 命令调用

    function Fn(){}  new Fn() //{}

    let Fn = () => {} new Fn() //error

3.箭头函数内部没有arguments对象，可以用rest参数代替

    function fn(){ console.log(arguments) }  			fn(1,2,3,40) //类数组

    let fn = () => console.log(arguments)  				fn(1,2,3,4)  //error

    let fn = (...arg) => console.log(arg)  
    fn(1,2,3,4)  // [1,2,3,4]

4.箭头函数不能做generator函数 内部不能使用yield命令
    function * gen(){
        yield 1+1
        yield 'abc'
        return 123
    }
    let g = gen()
    g.next() //{value:2,done:false}
    
    let gen1 = () => { yield 'a' }
    gen1()  //error
```

#### 对象的扩展

##### 对象的简洁表达式

```js
let a = 100; const obj = { a:a,  getA:function(){ alert(this.a) } } 

// 简洁
const obj = {  a,  getA(){alert(this.a)} }
```

##### 对象的属性表达式

```js
const obj = { name:'zs',['age'+1]:18 }
```

##### 对象属性的添加方式

```js
// 单个属性： 

Object.defineProperty(obj,key,{ 
	value: xx, 
    writable:false, //修改 默认否 可省略 
	enumerable:false, // 遍历 默认否 可省略 
	comfigurable:false //删除 默认否 可省略 
}) 

// 所有属性： 
Object.defineProperties(obj,{ 属性:{ value... } })
```

##### 获取对象属性的描述对象

```js
单个属性： Object.getOwnPropertyDescriptor(obj,key) 

所有属性： Object.getOwnPropertyDescriptors(obj) 
```

##### 对象的新增方法

```js
Object.is() 
判断 2值是否严格相等（长一样就是true，不一样false） 

注意：
NaN === NaN // false
Object.is(NaN,NaN) // true 
+0 === -0 // true 

Object.assign() // 拷贝 

Object.keys() Object.Values() Object.entries() 

Object.fromEntires() // 将键值对 数组 转为对象；
// 例子：[['a',1],['b',2]] ==> {a:1,b:2}
```

##### 赋值函数与取值函数

```js
//1. 在 Object.defineProperty 与Object.defineProperties 

let person = {} let value = '';

Object.defineProperty(person,'sex',{// 
	set(v) { value = v } //赋值 
	get(v) { return value } //取值 
})

person.sex // '' 

person.sex = '男' 

person.sex //‘男’

// 2. 字面量法定义对象 

const person = { 
    _sex:'',
    get sex(){ return this._sex },
    set sex(value){ this._sex = value } 
} 

person.sex // ''
person.sex = '男' 
person.sex //‘男’
```

##### 对象的循环

```js
for in // 对象自身 和 继承的可枚举属性 不包括symbol 

Object.keys() // 对象自身可枚举属性 不包括symbol 

Object.getOwnPropertyNames() // 自身所有属性的键名 不包括symbol 

Object.getOwnPropertySymbols() // 自身所有symbol 属性 键名 

Reflect.ownkeys() // 自身所有属性的键名
```

#### 运算符的扩展

##### ?. 链式判断运算符

```js
// 使用场景：

对象的属性 a?.b a?.[b]；函数或对象方法的调用 

fn?.(...args) const obj = { x:{a:1} } 

obj.c.a //报错 undefined 没有 a 属性 

obj.c?.a //undefined
```

##### ** 指数运算符

```
2**3**4 从右 到 左 运算
```

##### ?? null判断运算符

```json
// 设置默认值，函数的参数默认值 

let x = a??1 // 若 a 是 null 或 undefined 时，x = 1
```

##### &&= ||= ??= 逻辑赋值运算符

```js
var a = a??1 //  ==> a??=1 和 a = a+1 简写 a+=1
```

#### Symbol

```
Symbol 基础数据类型  表示独一无二的值，是一个普通函数

Symbol  做对象的属性时，用对象的的属性表达式 [Symbol()] 不会担心同名属性覆盖
```

##### 声明方式

```js
1.Symbol() / Symbol('a')  // 便于描述好区分，无实际意义

2.Symbol.for('a') // 全局注册字符串做参数；搜索有没有以该参数命名的Symbol值，有返回Symbol值，没有就声明

// 2种声明方式的区别：Symbol.for('a') 被登记在全局供搜索，第一种不会

Symbol.keyFor() //返回一个已登记的Symbol类型值的key，没有返回undefined

    const s = Symbol.for('abc')
    Symbol.keyFor(s)  // abc
```

##### Symbol 转为其他数据类型

```
    转string ：只能强制转换
      number ：不能转
      boolean:隐式转换，强制转换
```

#### Set

```js
// 创建

\1. let s = new Set()  s.add()

\2. let s = new Set([1,2,3,4,2])属性：

size 成员个数属性

add() 添加；返回 Set本身

has() 查询；返回布尔值

delete() 删除；返回布尔值

clear() 清空；放回undefined
```

```js
// 注意：
//    查询引用数据类型 易错点

    s.add({a:1})  s.has({a:1}) //false  地址不一样

    let obj = {a:1}  s.add(obj)  s.has(obj) // true
//遍历：s.keys() values() entries() forEach()
```

```js
Set 转数组 ===>  [...new Set()]   /  Array.from( new Set() )

数组转 Set ===>  new Set(arr)
```

#### WeakSet

```
成员只能是数组 对对象的引用是弱引用弱引用：垃圾回收机制不考虑 weakSet对 该对象有引用，如果其他对象都不在引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于weakSet中。
```

```js
// 创建：

let ws = new WeakSet()

方法：添加  ws.add({a:1}).add([1,2])

// 因为是弱引用 所以没有size属性，没有clear方法，不能遍历
```

#### Map

```js
// 创建：

\1. let m = new Map()  m.set('a',1).set({name:'zs'},true) //key 可以是任意数据类型

\2. let m = new Map([ ['a',1] , ['b',2,true] ]) //参数是键值对数组 ['b',2,true] 多的元素会被省略，不放到map结构中

// 属性：

size 成员总数

// 方法：

set(key,value) 添加，设置键值对，返回 map结构本身可以链式；key可以是任意数据类型

get(key) 获取；通过键取值

has(key) 查询；返回布尔值

delete(key) 删除；返回布尔值 成功true

clear() 清空；返回undefined

遍历 m.keys() values() entries() forEach()
```

 

```js
// Map 与 对象 的转换

	// 对象 > Map  ==> 
	1. 循环 Object.keys() 把 键值对 用 set 添加
    2. new Map(Object.entries(obj)) 

	// Map > 对象  ==> 
	Object.fromEntries(map)

// Map 与 string 转换：

   Map --> string
   string --> map

// Map 与数组 转换:

    Map --> array ...
    array --> Map  new Map()

// Map 与 对象 的区别：

    1.键值 用 => 链接
    2.任何数据都可以当键
```

#### WeakMap

```js
// 键 只能是对象类型 是弱引用 不能遍历

const wm = new WeakMap()

wm.set({name:'zs'},true)

//方法： get()  set()  has()  delete()
```

#### Set 、Map、数组、对象 对数据的操作对比

```js
const arr = [ {name:'苹果',id:'1001'},{name:'橙子',id:'1002'} ] 

检测以上数据是否有 1002 对应的商品，先得到，在删除； 

let id = '1002'

//1.数组 

//查询 arr.some( item => item.id == id ) 

//取值 let index = arr.findIndex( item => item.id == id ) arr[index] 

//删除 arr.splice(index,1)

//2.对象 
let obj = { 1001:{name:'苹果',id:'1001'},1002:{name:'橙子',id:'1002'} }

//查询 id in obj

//取值 obj[id] 

//删除 delete obj[id]

//3.Set 

let set = new Set(arr) 

//查询 

let set_exist = false; 

for(let v of set.values() ){ if(v.id === id){ set_exist = true; break; } } 

//取值 

let set_val = null; 

for(let v of set.values() ){ if(v.id === id){ set_val = v; break; } } 

//删除 set.delete(set_val)

//4.Map 

let map = new Map() 

map.set('1001',{name:'苹果',id:'1001'}).set('1002',{name:'橙子',id:'1002'}) 

//查询 map.has(id) 

//取值 map.get(id) 

//删除 map.delete(id)
```

#### Reflect

> **Reflect:反射 用来代替object 处理对象的相关操作**

##### 设置属性

```js
Reflect.set(obj,key,value)

Reflect.set(obj,key,value,receiver) 

// receiver 可选参数，如果设置的属性是赋值函数，那么this 指向这个参数
```

##### 获取属性值

```js
Reflect.get(obj,key,receiver)

// receiver 可选参数，如果获取的属性是取值函数，那么this 指向这个参数

let obj = {x:1,y:2}

const obj2 = {
    x:100,
    y:100,
    get z(){
        return this.x+this.y
    }
}

Reflect.get(obj2,z,obj1) //3
```

##### 查询是否有某个属性

```
Reflect.has(obj,key)
```

##### 删除 某个属性

```
Reflect.deleteProperty(obj,key)
```

##### 定义属性

```js
Reflect.defineProperty(obj,key,{value:....})

// 相当于 es5: 
Object.defineProperty()  Object.difineProperties()
```

##### 获取所有的属性名

```js
Reflect.ownkeys(obj)

// 相当于 es5: 
	Object.getOwnPropertyNames() //普通属性
	Object.getOwnPropertySymbols()// symbol 属性
```

##### Reflect反射函数操作

```js
// 1. 调用函数，更改this指向

es5: call/apply/bind

call \ apply 更改this指向并调用函数，bind只更改指向，不调用函数，得手动调用 fn.bind()()

call 参数传散列，apply传参数数组，bind参数最好放在函数调用里

Reflect.apply(func,thisArg,args)
// func 调用函数 thisArg this指向 args 参数[]

// 2. 代替new 关键字用来调用构造函数 创建实例

Reflect.construct( 构造函数，参数[] )

es5: let arr = new Array(10)

let arr1 = Reflect.construct(Array,[10])
```

#### Proxy 代理

> **proxy 代理 在目标对象之前架设一层拦截，对外界的访问进行过滤和改写**

```js
// 用法：
let proxy = new Proxy(target目标对象,handler拦截对象)
```

##### 1. 拦截对象属性的操作

```js
let proxy = new Proxy(obj,{

    get(target,key,proxy){}  // 拦截属性的读取

    set(target,key,value,proxy){}  // 拦截属性的设置

    has(target,key){}  //拦截属性的查询

    deleteProperty(target,key){}  //拦截属性的删除

    defineProperty(target,key){}

    ownKeys(target){} //拦截属性的遍历

}) 
```

##### 2. 拦截函数调用的操作

```js
let proxy = new Proxy(fn,{
    apply(target,context,args){ 
    	target.apply(context,args)
    }
})

// 例子： Reflect.apply(proxy,null,[1,2])
```

##### 3. 拦截new关键字

```js
let ProxyFn = new Proxy(Fn,{
    construct(target,args,proxy)
})

// 例子： Reflect.construct(构造函数，参数[])
```

#### 面向对象 es5

> **面向对象：面向对象是一种编程思想，是用对象时只关注对象，提供的功能，不关注其内部细节**
>
> **面向对象的特性：封装、多态、继承**
>
> **es5 实现面向对象的方式：构造函数内部写属性，原型上写方法**
>

##### 工厂模式创建对象

```js
function createObj(name,age){
    const obj = new Object()
    obj.name = name;
    obj.age = age;
    obj.like = function(){}
    return obj
}

const obj1 = createObj('zs',18)
```

##### 系统创建对象

```js
const arr = new Array()

const date = new Date()

function CreateObj(name,age){
    this.name = name;
    this.age = age;
}

CreateObj.prototype.like = function(){}

const obj = new CreateObj('zs',18)
```

##### new的作用

```js
1.在构造函数内部创建this对象

2.构造函数最好隐式返回this对象

3.this指向new构造函数创建出来的实例

4.让实例的__proto__ ([[prototype]]) 属性指向构造函数的原型prototype
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps49.jpg) 

##### 继承

```js
//通过继承创建的新类 称： 子类 、派生类
//被继承的类 称： 基类、父类、超类

// 子类继承父类：

// 属性继承： 
	Reflect.apply(父类，this,[])

//方法继承：

    function Fn(){}
    Fn.prototype = 父类.prototype
    const f = new Fn()
    子类.prototype = f //缺了constructor属性
    子类.prototype.constructor = 子类

**Object.create()**

const obj = Object.create(A,{})

// 将A对象设置为obj的隐式原型，第二参数：可以扩展自己的属性和方法，类似于Object.defineProperties(obj,{})的第二参数
```



#### 面向对象 es6

```js
class 定义一个 类

// 类的本质 还是构造函数

class 是构造函数的语法糖

class F {
    constructor(){}
    skill(){}
}
```

##### 继承

```js
// 父类：

class Parent {
    constructor(x){ this.x = x }
    getX(){ console.log(this.x) } 
    //实例方法，创建实例在实例调用
    static sayHello(){ console.log('hello') } 
    // 静态方法，构造函数调用
}

// 子类：

class Child extends Parent{
    constructor(x,y){
        super(x)
        this.y = y
    }
    a(){
        super.getX()
        super.sayHello() //报错
    }
    static b(){
        super.getX() //报错
        super.sayHello()
    }

}
```

##### super

```js
super 只能在子类中使用：

1.super 作为函数调用：

	代表的是父类构造函数，在子类的constructor内部使用，用来塑造this对象

2.super作为对象使用：

    a. 在普通函数：代表父类原型
    b. 在静态方法：代表父类 父类构造函数
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps50.jpg) 

#### 模块化 

> **模块化**：将软件分解成若干个独立的，可替换的，具备预定功能的模块，每个模块实现一个功能，个模块通过接口，组合在一起，形成最终程序

##### ESModule/ESM

```js
1.引入到 html中时： <script src=""  type="module"></script>

一般把模块引到 入口文件中，dom中引入入口文件

2.下图

3.as 重命名 抛出或引入都可以重命名
```

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps51.jpg) 

##### commonJs

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps52.jpg) 

##### ESM与CJS区别

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps53.jpg) 

##### 其他模块化

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps54.jpg) 

#### 异步编程--------解决异步问题方法

![1712394918809](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712394918809.png)

##### 1. 回调函数

```
回调函数解决异步问题：把异步操作步骤封装在一个函数中,当异步操作执行完，调用回调函数

回调地狱：回调函数嵌套多层,不易维护 
```

##### 2. Promise

  Promise是一种为简化异步编程而设计的核心语言特性，它是一个对象，表示异步操作的结果。在最简单的情况下，Promise就是一种处理回调的不同方式。不过，使用Promise也有实际的用处，基于回调的异步编程会有一个很现实的问题，那就是**经常出现回调多层嵌套**的情况，会造成代码难以理解。Promise可以让这种嵌套回调以一种更线性的链式形式表达出来，因此更容易阅读和理解。

回调的另一个问题就是**难以处理错误，** 如果一个异步函数抛出异常，则该异常没有办法传播到异步操作的发起者。异步编程的一个基本事实就是它破坏了异常处理。而Promise则标准化了异步错误处理，通过Promise链提供一种让错误正确传播的途经。

实际上，Promise就是一个容器，里面保存着某个未来才会结束的事件（通常是异步操作）的结果。从语法上说，Promise 是一个对象，它可以获取异步操作的消息。Promise 提供了统一的 API，各种异步操作都可以用同样的方法进行处理。

（1）Promise实例有**三个状态**:

- pending 状态：表示进行中。Promise 实例创建后的初始态；
- fulfilled 状态：表示成功完成。在执行器中调用 resolve 后达成的状态；
- rejected 状态：表示操作失败。在执行器中调用 reject 后达成的状态。

（2）Promise实例有**两个过程**：

- pending -> fulfilled : Resolved（已完成）；
- pending -> rejected：Rejected（已拒绝）。

**Promise的特点：**

- 一旦状态改变就不会再变，promise对象的状态改变，只有两种可能：从`pending`变为`fulfilled`，从`pending`变为`rejected`。当 Promise 实例被创建时，内部的代码就会立即被执行，而且无法从外部停止。比如无法取消超时或消耗性能的异步调用，容易导致资源的浪费；
- 如果不设置回调函数，Promise内部抛出的错误，不会反映到外部；
- Promise 处理的问题都是“一次性”的，因为一个 Promise 实例只能 resolve 或 reject 一次，所以面对某些需要持续响应的场景时就会变得力不从心。比如上传文件获取进度时，默认采用的就是事件监听的方式来实现。

下面来看一个例子：

```js
const https = require('https');

function httpPromise(url){
  return new Promise((resolve,reject) => {
    https.get(url, (res) => {
      resolve(data);
    }).on("error", (err) => {
      reject(error);
    });
  })
}

httpPromise().then((data) => {
  console.log(data)
}).catch((error) => {
  console.log(error)
})
```

可以看到，Promise 会接收一个执行器，在这个执行器里，需要把目标异步任务给放进去。在 Promise 实例创建后，执行器里的逻辑会立刻执行，在执行的过程中，根据异步返回的结果，决定如何使用 resolve 或 reject 来改变 Promise实例的状态。

在这个例子里，当用 resolve 切换到了成功态后，Promise 的逻辑就会走到 then 中传入的方法里去；用 reject 切换到失败态后，Promise 的逻辑就会走到 catch 传入的方法中。

这样的逻辑，本质上与回调函数中的成功回调和失败回调没有差异。但这种写法大大地提高了代码的质量。当我们进行大量的异步链式调用时，回调地狱不复存在了。取而代之的是层级简单、赏心悦目的 Promise 调用链：

```js
httpPromise(url1)
    .then(res => {
        console.log(res);
        return httpPromise(url2);
    })
    .then(res => {
        console.log(res);
        return httpPromise(url3);
    })
    .then(res => {
      console.log(res);
      return httpPromise(url4);
    })
    .then(res => console.log(res));
```

###### Promise的创建

Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。

```js
const promise = new Promise((resolve, reject) => {
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

一般情况下，我们会用`new Promise()`来创建Promise对象。除此之外，还也可以使用`promise.resolve`和 `promise.reject`这两个方法来创建：

**（1）Promise.resolve**

`Promise.resolve(value)`的返回值是一个promise对象，我们可以对返回值进行.then调用，如下代码：

```js
Promise.resolve(11).then(function(value){
  console.log(value); // 打印出11
});
```

`resolve(11)`会让promise对象进入确定(`resolve`状态)，并将参数`11`传递给后面`then`中指定的`onFulfilled` 函数；

**（2）Promise.reject**

`Promise.reject` 的返回值也是一个promise对象，如下代码：

```js
Promise.reject(new Error("我错了！"));
```

上面是以下代码的简单形式：

```js
new Promise((resolve, reject) => {
   reject(new Error("我错了！"));
});
```

下面来综合看看resolve方法和reject方法：

```js
function testPromise(ready) {
  return new Promise(resolve,reject) => {
    if(ready) {
      resolve("hello world");
    }else {
      reject("No thanks");
    }
  });
};

testPromise(true).then((msg) => {
  console.log(msg);
},(error) => {
  console.log(error);
});
```

上面的代码给`testPromise`方法传递一个参数，返回一个promise对象，如果为`true`，那么调用Promise对象中的`resolve()`方法，并且把其中的参数传递给后面的`then`第一个函数内，因此打印出 “`hello world`”, 如果为`false`，会调用promise对象中的`reject()`方法，则会进入`then`的第二个函数内，会打印`No thanks`。

######  Promise的作用

在开发中可能会碰到这样的需求：使用ajax发送A请求，成功后拿到数据，需要把数据传给B请求，那么需要这样编写代码：

```js
let fs = require('fs')
fs.readFile('./a.txt','utf8',function(err,data){
  fs.readFile(data,'utf8',function(err,data){
    fs.readFile(data,'utf8',function(err,data){
      console.log(data)
    })
  })
})
```

这段代码之所以看上去很乱，归结其原因有两点：

- **第一是嵌套调用**，下面的任务依赖上个任务的请求结果，并**在上个任务的回调函数内部执行新的业务逻辑**，这样当嵌套层次多了之后，代码的可读性就变得非常差了。
- **第二是任务的不确定性**，执行每个任务都有两种可能的结果（成功或者失败），所以体现在代码中就需要对每个任务的执行结果做两次判断，这种对每个任务都要进行一次额外的错误处理的方式，明显增加了代码的混乱程度。

既然原因分析出来了，那么问题的解决思路就很清晰了：

- 消灭嵌套调用；
- 合并多个任务的错误处理。

这么说可能有点抽象，不过 Promise 解决了这两个问题。接下来就看看 Promise 是怎么消灭嵌套调用和合并多个任务的错误处理的。

`Promise`出现之后，代码可以这样写：

```js
let fs = require('fs')
function read(url){
  return new Promise((resolve,reject)=>{
    fs.readFile(url,'utf8',function(error,data){
      error && reject(error)
      resolve(data)
    })
  })
}
read('./a.txt').then(data=>{
  return read(data) 
}).then(data=>{
  return read(data)  
}).then(data=>{
  console.log(data)
})
```

通过引入 Promise，上面这段代码看起来就非常线性了，也非常符合人的直觉。Promise 利用了三大技术手段来解决回调地狱：**回调函数延迟绑定、返回值穿透、错误冒泡。**

下面来看一段代码：

```js
let readFilePromise = (filename) => {
  fs.readFile(filename, (err, data) => {
    if(err) {
      reject(err);
    }else {
      resolve(data);
    }
  })
}
readFilePromise('1.json').then(data => {
  return readFilePromise('2.json')
});
```

可以看到，回调函数不是直接声明的，而是通过后面的 then 方法传入的，即延迟传入，这就是回调函数延迟绑定。接下来针对上面的代码做一下调整，如下：

```js
let x = readFilePromise('1.json').then(data => {
  return readFilePromise('2.json')  //这是返回的Promise
});
x.then()
```

根据 then 中回调函数的传入值创建不同类型的 Promise，然后把返回的 Promise 穿透到外层，以供后续的调用。这里的 x 指的就是内部返回的 Promise，然后在 x 后面可以依次完成链式调用。这便是返回值穿透的效果，这两种技术一起作用便可以将深层的嵌套回调写成下面的形式。

```js
readFilePromise('1.json').then(data => {
    return readFilePromise('2.json');
}).then(data => {
    return readFilePromise('3.json');
}).then(data => {
    return readFilePromise('4.json');
});
```

这样就显得清爽许多，更重要的是，它更符合人的线性思维模式，开发体验更好，两种技术结合产生了链式调用的效果。

这样解决了多层嵌套的问题，那另外一个问题，即每次任务执行结束后分别处理成功和失败的情况怎么解决的呢？Promise 采用了错误冒泡的方式。下面来看效果：

```js
readFilePromise('1.json').then(data => {
    return readFilePromise('2.json');
}).then(data => {
    return readFilePromise('3.json');
}).then(data => {
    return readFilePromise('4.json');
}).catch(err => {
  // xxx
})
```

这样前面产生的错误会一直向后传递，被 catch 接收到，就不用频繁地检查错误了。从上面的这些代码中可以看到，Promise 解决效果也比较明显：实现链式调用，解决多层嵌套问题；实现错误冒泡后一站式处理，解决每次任务中判断错误、增加代码混乱度的问题。

###### Promise的方法

Promise常用的方法：then()、catch()、all()、race()、finally()、allSettled()、any()。

###### （1）then()

当Promise执行的内容符合成功条件时，调用`resolve`函数，失败就调用`reject`函数。那Promise创建完了，该如何调用呢？这时就该then出场了：

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为`resolved`时调用，第二个回调函数是Promise对象的状态变为`rejected`时调用。其中第二个参数可以省略。

`then`方法返回的是一个新的Promise实例。因此可以采用**链式写法**，即`then`方法后面再调用另一个then方法。当写有顺序的异步事件时，需要串行时，可以这样写：

```js
let promise = new Promise((resolve,reject)=>{
    ajax('first').success(function(res){
        resolve(res);
    })
})
promise.then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    return new Promise((resovle,reject)=>{
        ajax('second').success(function(res){
            resolve(res)
        })
    })
}).then(res=>{
    
})
```

###### （2）catch()

Promise对象的catch方法相当于`then`方法的第二个参数，指向`reject`的回调函数。

不过`catch`方法还有一个作用，就是在执行`resolve`回调函数时，如果出现错误，抛出异常，不会停止运行，而是进入`catch`方法中：

```js
p.then((data) => {
     console.log('resolved',data);
},(err) => {
     console.log('rejected',err);
}); 
```

###### （3）all()

`all`方法可以完成**并行任务**， 它接收一个数组，数组的每一项都是一个`promise`对象。当数组中所有的`promise`的状态都达到`resolved`时，`all`方法的状态就会变成`resolved`，如果有一个状态变成了`rejected`，那么`all`方法的状态就会变成`rejected`：

```js
let promise1 = new Promise((resolve,reject)=>{
 setTimeout(()=>{
       resolve(1);
 },2000)
});
let promise2 = new Promise((resolve,reject)=>{
 setTimeout(()=>{
       resolve(2);
 },1000)
});
let promise3 = new Promise((resolve,reject)=>{
 setTimeout(()=>{
       resolve(3);
 },3000)
});

Promise.all([promise1,promise2,promise3]).then(res=>{
    console.log(res);  //结果为：[1,2,3] 
})
```

调用`all`方法时的结果成功的时候是回调函数的参数也是一个数组，这个数组按顺序保存着每一个promise对象`resolve`执行时的值。

###### （4）race()

`race`方法和`all`一样，接受的参数是一个每项都是`promise`的数组，但与`all`不同的是，当最先执行完的事件执行完之后，就直接返回该`promise`对象的值。

如果第一个`promise`对象状态变成`resolved`，那自身的状态变成了`resolved`；反之，第一个`promise`变成`rejected`，那自身状态就会变成`rejected`。

```js
let promise1 = new Promise((resolve,reject) => {
 setTimeout(() =>  {
       reject(1);
 },2000)
});
let promise2 = new Promise((resolve,reject) => {
 setTimeout(() => {
       resolve(2);
 },1000)
});
let promise3 = new Promise((resolve,reject) => {
 setTimeout(() => {
       resolve(3);
 },3000)
});
Promise.race([promise1,promise2,promise3]).then(res => {
 console.log(res); //结果：2
},rej => {
    console.log(rej)};
)
```

那么`race`方法有什么实际作用呢？当需要执行一个任务，超过多长时间就不做了，就可以用这个方法来解决：

```js
Promise.race([promise1, timeOutPromise(5000)]).then(res => console.log(res))
```

###### （5）finally()

`finally`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

```js
promise.then(result => {···})
    .catch(error => {···})
       .finally(() => {···});
```

上面代码中，不管`promise`最后的状态如何，在执行完`then`或`catch`指定的回调函数以后，都会执行`finally`方法指定的回调函数。

下面来看例子，服务器使用 Promise 处理请求，然后使用`finally`方法关掉服务器。

```js
server.listen(port)
  .then(function () {
    // ...
  })
  .finally(server.stop);
```

`finally`方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是`fulfilled`还是`rejected`。这表明，`finally`方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

`finally`本质上是`then`方法的特例：

```js
promise
.finally(() => {
  // 语句
});

// 等同于
promise
.then(
  result => {
    // 语句
    return result;
  },
  error => {
    // 语句
    throw error;
  }
);
```

上面代码中，如果不使用`finally`方法，同样的语句需要为成功和失败两种情况各写一次。有了`finally`方法，则只需要写一次。

###### （6）allSettled()

Promise.allSettled 的语法及参数跟 Promise.all 类似，其参数接受一个 Promise 的数组，返回一个新的 Promise。唯一的不同在于，执行完之后不会失败，也就是说当 Promise.allSettled 全部处理完成后，我们可以拿到每个 Promise 的状态，而不管其是否处理成功。

下面使用 allSettled 实现的一段代码：

```js
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const allSettledPromise = Promise.allSettled([resolved, rejected]);
allSettledPromise.then(function (results) {
  console.log(results);
});
// 返回结果：
// [
//    { status: 'fulfilled', value: 2 },
//    { status: 'rejected', reason: -1 }
// ]
```

可以看到，Promise.allSettled 最后返回的是一个数组，记录传进来的参数中每个 Promise 的返回值，这就是和 all 方法不太一样的地方。你也可以根据 all 方法提供的业务场景的代码进行改造，其实也能知道多个请求发出去之后，Promise 最后返回的是每个参数的最终状态。

###### （7）any()

any 方法返回一个 Promise，只要参数 Promise 实例有一个变成 fullfilled 状态，最后 any 返回的实例就会变成 fullfilled 状态；如果所有参数 Promise 实例都变成 rejected 状态，包装实例就会变成 rejected 状态。

下面对上面 allSettled 这段代码进行改造，来看下改造完的代码和执行结果：

```js
const resolved = Promise.resolve(2);
const rejected = Promise.reject(-1);
const allSettledPromise = Promise.any([resolved, rejected]);
allSettledPromise.then(function (results) {
  console.log(results);
});
// 返回结果：2
```

可以看出，只要其中一个 Promise 变成 fullfilled 状态，那么 any 最后就返回这个 Promise。由于上面 resolved 这个 Promise 已经是 resolve 的了，故最后返回结果为 2。

###### Promise的异常处理

错误处理是所有编程范型都必须要考虑的问题，在使用 JavaScript 进行异步编程时，也不例外。如果我们不做特殊处理，会怎样呢？来看下面的代码，先定义一个必定会失败的方法

```js
let fail = () => {
    setTimeout(() => {
 throw new Error("fail");
    }, 1000);
};
```

调用：

```js
console.log(1);
try {
    fail();
} catch (e) {
    console.log("captured");
}
console.log(2);
```

可以看到打印出了 1 和 2，并在 1 秒后，获得一个“Uncaught Error”的错误打印，注意观察这个错误的堆栈：

```js
Uncaught Error: fail
    at <anonymous>:3:9
```

可以看到，其中的 setTimeout (async) 这样的字样，表示着这是一个异步调用抛出的堆栈。但是，captured”这样的字样也并未打印，因为母方法 fail() 本身的原始顺序执行并没有失败，这个异常的抛出是在回调行为里发生的。 从上面的例子可以看出，对于异步编程来说，我们需要使用一种更好的机制来捕获并处理可能发生的异常。

Promise 除了支持 resolve 回调以外，还支持 reject 回调，前者用于表示异步调用顺利结束，而后者则表示有异常发生，中断调用链并将异常抛出：

```js
const exe = (flag) => () => new Promise((resolve, reject) => {
    console.log(flag);
    setTimeout(() => {
        flag ? resolve("yes") : reject("no");
    }, 1000);
});
```

上面的代码中，flag 参数用来控制流程是顺利执行还是发生错误。在错误发生的时候，no 字符串会被传递给 reject 函数，进一步传递给调用链：

```js
Promise.resolve()
       .then(exe(false))
       .then(exe(true));
```

上面的调用链，在执行的时候，第二行就传入了参数 false，它就已经失败了，异常抛出了，因此第三行的 exe 实际没有得到执行，执行结果如下：

```js
false
Uncaught (in promise) no
```

这就说明，通过这种方式，调用链被中断了，下一个正常逻辑 exe(true) 没有被执行。 但是，有时候需要捕获错误，而继续执行后面的逻辑，该怎样做？这种情况下就要在调用链中使用 catch 了：

```js
Promise.resolve()
       .then(exe(false))
       .catch((info) => { console.log(info); })
       .then(exe(true));
```

这种方式下，异常信息被捕获并打印，而调用链的下一步，也就是第四行的 exe(true) 可以继续被执行。将看到这样的输出：

```js
false
no
true
```

###### Promise的实现

这一部分就来简单实现一下Promise及其常用的方法。

###### （1）Promise

```js
const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";

function MyPromise(fn) {
  // 保存初始化状态
  var self = this;

  // 初始化状态
  this.state = PENDING;

  // 用于保存 resolve 或者 rejected 传入的值
  this.value = null;

  // 用于保存 resolve 的回调函数
  this.resolvedCallbacks = [];

  // 用于保存 reject 的回调函数
  this.rejectedCallbacks = [];

  // 状态转变为 resolved 方法
  function resolve(value) {
    // 判断传入元素是否为 Promise 值，如果是，则状态改变必须等待前一个状态改变后再进行改变
    if (value instanceof MyPromise) {
      return value.then(resolve, reject);
    }

    // 保证代码的执行顺序为本轮事件循环的末尾
    setTimeout(() => {
      // 只有状态为 pending 时才能转变，
      if (self.state === PENDING) {
        // 修改状态
        self.state = RESOLVED;

        // 设置传入的值
        self.value = value;

        // 执行回调函数
        self.resolvedCallbacks.forEach(callback => {
          callback(value);
        });
      }
    }, 0);
  }

  // 状态转变为 rejected 方法
  function reject(value) {
    // 保证代码的执行顺序为本轮事件循环的末尾
    setTimeout(() => {
      // 只有状态为 pending 时才能转变
      if (self.state === PENDING) {
        // 修改状态
        self.state = REJECTED;

        // 设置传入的值
        self.value = value;

        // 执行回调函数
        self.rejectedCallbacks.forEach(callback => {
          callback(value);
        });
      }
    }, 0);
  }

  // 将两个方法传入函数执行
  try {
    fn(resolve, reject);
  } catch (e) {
    // 遇到错误时，捕获错误，执行 reject 函数
    reject(e);
  }
}

MyPromise.prototype.then = function(onResolved, onRejected) {
  // 首先判断两个参数是否为函数类型，因为这两个参数是可选参数
  onResolved =
    typeof onResolved === "function"
      ? onResolved
      : function(value) {
          return value;
        };

  onRejected =
    typeof onRejected === "function"
      ? onRejected
      : function(error) {
          throw error;
        };

  // 如果是等待状态，则将函数加入对应列表中
  if (this.state === PENDING) {
    this.resolvedCallbacks.push(onResolved);
    this.rejectedCallbacks.push(onRejected);
  }

  // 如果状态已经凝固，则直接执行对应状态的函数

  if (this.state === RESOLVED) {
    onResolved(this.value);
  }

  if (this.state === REJECTED) {
    onRejected(this.value);
  }
};
```

###### （2）Promise.then

`then` 方法返回一个新的 `promise` 实例，为了在 `promise` 状态发生变化时（`resolve` / `reject` 被调用时）再执行 `then` 里的函数，我们使用一个 `callbacks` 数组先把传给then的函数暂存起来，等状态改变时再调用。

**那么，怎么保证后一个 then 里的方法在前一个 then（可能是异步）结束之后再执行呢？**

可以将传给 `then` 的函数和新 `promise` 的 `resolve` 一起 `push` 到前一个 `promise` 的 `callbacks` 数组中，达到承前启后的效果：

- 承前：当前一个 `promise` 完成后，调用其 `resolve` 变更状态，在这个 `resolve` 里会依次调用 `callbacks` 里的回调，这样就执行了 `then` 里的方法了
- 启后：上一步中，当 `then` 里的方法执行完成后，返回一个结果，如果这个结果是个简单的值，就直接调用新 `promise` 的 `resolve`，让其状态变更，这又会依次调用新 `promise` 的 `callbacks` 数组里的方法，循环往复。。如果返回的结果是个 `promise`，则需要等它完成之后再触发新 `promise` 的 `resolve`，所以可以在其结果的 `then` 里调用新 `promise` 的 `resolve`

```js
then(onFulfilled, onReject){
    // 保存前一个promise的this
    const self = this; 
    return new MyPromise((resolve, reject) => {
      // 封装前一个promise成功时执行的函数
      let fulfilled = () => {
        try{
          const result = onFulfilled(self.value); // 承前
          return result instanceof MyPromise? result.then(resolve, reject) : resolve(result); //启后
        }catch(err){
          reject(err)
        }
      }
      // 封装前一个promise失败时执行的函数
      let rejected = () => {
        try{
          const result = onReject(self.reason);
          return result instanceof MyPromise? result.then(resolve, reject) : reject(result);
        }catch(err){
          reject(err)
        }
      }
      switch(self.status){
        case PENDING: 
          self.onFulfilledCallbacks.push(fulfilled);
          self.onRejectedCallbacks.push(rejected);
          break;
        case FULFILLED:
          fulfilled();
          break;
        case REJECT:
          rejected();
          break;
      }
    })
   }
```

**注意：**

- 连续多个 `then` 里的回调方法是同步注册的，但注册到了不同的 `callbacks` 数组中，因为每次 `then` 都返回新的 `promise` 实例（参考上面的例子和图）
- 注册完成后开始执行构造函数中的异步事件，异步完成之后依次调用 `callbacks` 数组中提前注册的回调

###### （3）Promise.all

该方法的参数是 Promise 的实例数组, 然后注册一个 then 方法。 待数组中的 Promise 实例的状态都转为 fulfilled 之后则执行 then 方法.，这里主要就是一个计数逻辑, 每当一个 Promise 的状态变为 fulfilled 之后就保存该实例返回的数据, 然后将计数减一, 当`计数器`变为 `0` 时, 代表数组中所有 Promise 实例都执行完毕.

```js
Promise.all = function (arr) {
  let args = Array.prototype.slice.call(arr)
  return new Promise(function (resolve, reject) {
    if (args.length === 0) return resolve([])
    let remaining = args.length
    function res(i, val) {
      try {
        if (val && (typeof val === 'object' || typeof val === 'function')) {
          let then = val.then
          if (typeof then === 'function') {
            then.call(val, function (val) { // 这里如果传入参数是 promise的话需要将结果传入 args, 而不是 promise实例
              res(i, val) 
            }, reject)
            return
          }
        }
        args[i] = val
        if (--remaining === 0) {
          resolve(args)
        }
      } catch (ex) {
        reject(
```

###### （4）Promise.race

该方法的参数是 Promise 实例数组, 然后其 then 注册的回调方法是数组中的某一个 Promise 的状态变为 fulfilled 的时候就执行. 因为 Promise 的状态**只能改变一次**, 那么我们只需要把 Promise.race 中产生的 Promise 对象的 resolve 方法, 注入到数组中的每一个 Promise 实例中的回调函数中即可：

```js
oPromise.race = function (args) {
  return new oPromise((resolve, reject) => {
    for (let i = 0, len = args.length; i < len; i++) {
      args[i].then(resolve, reject)
    }
  })
}
```

##### 3. Generator函数 

###### Generator 概述

###### （1）Generator

Generator（生成器）是 ES6 中的关键词，通俗来讲 Generator 是一个带星号的函数（它并不是真正的函数），可以配合 yield 关键字来暂停或者执行函数。先来看一个例子：

```js
function* gen() {
  console.log("enter");
  let a = yield 1;
  let b = yield (function () {return 2})();
  return 3;
}
var g = gen()           // 阻塞，不会执行任何语句
console.log(typeof g)   // 返回 object 这里不是 "function"
console.log(g.next())
console.log(g.next())
console.log(g.next())
console.log(g.next()) 
```

输出结果如下：

```js
object
enter
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: true }
{ value: undefined, done: true }
```

Generator 中配合使用 yield 关键词可以控制函数执行的顺序，每当执行一次 next 方法，Generator 函数会执行到下一个存在 yield 关键词的位置。

总结，Generator 的执行的关键点如下：

- 调用 gen() 后，程序会阻塞，不会执行任何语句；
- 调用 g.next() 后，程序继续执行，直到遇到 yield 关键词时执行暂停；
- 一直执行 next 方法，最后返回一个对象，其存在两个属性：value 和 done。

###### （2）yield

yield 同样也是 ES6 的关键词，配合 Generator 执行以及暂停。yield 关键词最后返回一个迭代器对象，该对象有 value 和 done 两个属性，其中 done 属性代表返回值以及是否完成。yield 配合着 Generator，再同时使用 next 方法，可以主动控制 Generator 执行进度。

下面来看看多个 Generator 配合 yield 使用的情况：

```js
function* gen1() {
    yield 1;
    yield* gen2();
    yield 4;
}
function* gen2() {
    yield 2;
    yield 3;
}
var g = gen1();
console.log(g.next())
console.log(g.next())
console.log(g.next())
console.log(g.next())
```

执行结果如下：

```js
{ value: 1, done: false }
{ value: 2, done: false }
{ value: 3, done: false }
{ value: 4, done: false }
{value: undefined, done: true}
```

可以看到，使用 yield 关键词的话还可以配合着 Generator 函数嵌套使用，从而控制函数执行进度。这样对于 Generator 的使用，以及最终函数的执行进度都可以很好地控制，从而形成符合你设想的执行顺序。即便 Generator 函数相互嵌套，也能通过调用 next 方法来按照进度一步步执行。

###### （3）生成器原理

其实，在生成器内部，如果遇到 yield 关键字，那么 V8 引擎将返回关键字后面的内容给外部，并暂停该生成器函数的执行。生成器暂停执行后，外部的代码便开始执行，外部代码如果想要恢复生成器的执行，可以使用 result.next 方法。

那 V8 是怎么实现生成器函数的暂停执行和恢复执行的呢？

它用到的就是**协程**，协程是—种比线程更加轻量级的存在。我们可以把协程看成是跑在线程上的任务，一个线程上可以存在多个协程，但是在线程上同时只能执行一个协程。比如，当前执行的是 A 协程，要启动 B 协程，那么 A 协程就需要将主线程的控制权交给 B 协程，这就体现在 A 协程暂停执行，B 协程恢复执行; 同样，也可以从 B 协程中启动 A 协程。通常，如果从 A 协程启动 B 协程，我们就把 A 协程称为 B 协程的父协程。

正如一个进程可以拥有多个线程一样，一个线程也可以拥有多个协程。每一时刻，该线程只能执行其中某一个协程。最重要的是，协程不是被操作系统内核所管理，而完全是由程序所控制（也就是在用户态执行）。这样带来的好处就是性能得到了很大的提升，不会像线程切换那样消耗资源。

###### Generator 和 thunk 结合

下面先来了解一下什么是 thunk 函数，以判断数据类型为例：

```js
let isString = (obj) => {
  return Object.prototype.toString.call(obj) === '[object String]';
};
let isFunction = (obj) => {
  return Object.prototype.toString.call(obj) === '[object Function]';
};
let isArray = (obj) => {
  return Object.prototype.toString.call(obj) === '[object Array]';
};
....
```

可以看到，这里出现了很多重复的判断逻辑，平常在开发中类似的重复逻辑的场景也同样会有很多。下面来进行封装：

```js
let isType = (type) => {
  return (obj) => {
    return Object.prototype.toString.call(obj) === `[object ${type}]`;
  }
}
```

封装之后就可以这样使用，从而来减少重复的逻辑代码：

```js
let isString = isType('String');
let isArray = isType('Array');
isString("123");    // true
isArray([1,2,3]);   // true
```

相应的 isString 和 isArray 是由 isType 方法生产出来的函数，通过上面的方式来改造代码，明显简洁了不少。像 isType 这样的函数称为 thunk 函数，它的**基本思路都是接收一定的参数，会生产出定制化的函数，最后使用定制化的函数去完成想要实现的功能。**

这样的函数在 JS 的编程过程中会遇到很多，抽象度比较高的 JS 代码往往都会采用这样的方式。那 Generator 和 thunk 函数的结合是否能带来一定的便捷性呢？

下面以文件操作的代码为例，看一下 Generator 和 thunk 的结合能够对异步操作产生的效果：

```js
const readFileThunk = (filename) => {
  return (callback) => {
    fs.readFile(filename, callback);
  }
}
const gen = function* () {
  const data1 = yield readFileThunk('1.txt')
  console.log(data1.toString())
  const data2 = yield readFileThunk('2.txt')
  console.log(data2.toString)
}
let g = gen();
g.next().value((err, data1) => {
  g.next(data1).value((err, data2) => {
    g.next(data2);
  })
})
```

readFileThunk 就是一个 thunk 函数，上面的这种编程方式就让 Generator 和异步操作关联起来了。上面第三段代码执行起来嵌套的情况还算简单，如果任务多起来，就会产生很多层的嵌套，可读性不强，因此有必要把执行的代码进行封装优化：

```js
function run(gen){
  const next = (err, data) => {
    let res = gen.next(data);
    if(res.done) return;
    res.value(next);
  }
  next();
}
run(g);
```

可以看到， run 函数和上面的执行效果其实是一样的。代码虽然只有几行，但其包含了递归的过程，解决了多层嵌套的问题，并且完成了异步操作的一次性的执行效果。这就是通过 thunk 函数完成异步操作的情况。

###### Generator 和 Promise 结合

其实 Promise 也可以和 Generator 配合来实现上面的效果。还是利用上面的输出文件的例子，对代码进行改造，如下所示：

```js
const readFilePromise = (filename) => {
  return new Promise((resolve, reject) => {
    fs.readFile(filename, (err, data) => {
      if(err) {
        reject(err);
      }else {
        resolve(data);
      }
    })
  }).then(res => res);
}
// 这块和上面 thunk 的方式一样
const gen = function* () {
  const data1 = yield readFilePromise('1.txt')
  console.log(data1.toString())
  const data2 = yield readFilePromise('2.txt')
  console.log(data2.toString)
}
// 这里和上面 thunk 的方式一样
function run(gen){
  const next = (err, data) => {
    let res = gen.next(data);
    if(res.done) return;
    res.value(next);
  }
  next();
}
run(g);
```

可以看到，thunk 函数的方式和通过 Promise 方式执行效果本质上是一样的，只不过通过 Promise 的方式也可以配合 Generator 函数实现同样的异步操作。

###### co 函数库

co 函数库用于处理 Generator 函数的自动执行。核心原理其实就是通过和 thunk 函数以及 Promise 对象进行配合，包装成一个库。它使用起来非常简单，比如还是用上面那段代码，第三段代码就可以省略了，直接引用 co 函数，包装起来就可以使用了，代码如下：

```js
const co = require('co');
let g = gen();
co(g).then(res =>{
  console.log(res);
})
```

这段代码比较简单，几行就完成了之前写的递归的那些操作。那么为什么 co 函数库可以自动执行 Generator 函数，它的处理原理如下：

1. 因为 Generator 函数就是一个异步操作的容器，它需要一种自动执行机制，co 函数接受 Generator 函数作为参数，并最后返回一个 Promise 对象。
2. 在返回的 Promise 对象里面，co 先检查参数 gen 是否为 Generator 函数。如果是，就执行该函数；如果不是就返回，并将 Promise 对象的状态改为 resolved。
3. co 将 Generator 函数的内部指针对象的 next 方法，包装成 onFulfilled 函数。这主要是为了能够捕捉抛出的错误。
4. 关键的是 next 函数，它会反复调用自身。

##### 4. async/await 函数  

###### async/await 的概念

ES7 新增了两个关键字： async和await，代表异步JavaScript编程范式的迁移。它改进了生成器的缺点，提供了在不阻塞主线程的情况下使用同步代码实现异步访问资源的能力。其实 async/await 是 Generator 的语法糖，它能实现的效果都能用then链来实现，它是为优化then链而开发出来的。

从字面上来看，async是“异步”的简写，await则为等待，所以 async 用来声明异步函数，这个关键字可以用在函数声明、函数表达式、箭头函数和方法上。因为异步函数主要针对不会马上完成的任务，所以自然需要一种暂停和恢复执行的能力，使用await关键字可以暂停异步代码的执行，等待Promise解决。async 关键字可以让函数具有异步特征，但总体上代码仍然是同步求值的。

它们的用法很简单，首先用 async 关键字声明一个异步函数：

```js
async function httpRequest() {}
```

然后就可以在这个函数内部使用 await 关键字了：

```js
async function httpRequest() {
  let res1 = await httpPromise(url1)
  console.log(res1)
}
```

这里，await关键字会接收一个期约并将其转化为一个返回值或一个抛出的异常。通过情况下，我们不会使用await来接收一个保存期约的变量，更多的是把他放在一个会返回期约的函数调用面前，比如上述例子。这里的关键就是，await关键字并不会导致程序阻塞，代码仍然是异步的，而await只是掩盖了这个事实，这就意味着任何使用await的代码本身都是异步的。

下面来看看async函数返回了什么：

```js
async function testAsy(){
   return 'hello world';
}
let result = testAsy(); 
console.log(result)
```

![1712396311437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712396311437.png)

可以看到，async 函数返回的是 Promise 对象。如果异步函数使用return关键字返回了值（如果没有return则会返回undefined），这个值则会被 `Promise.resolve()` 包装成 Promise 对象。异步函数始终返回Promise对象。

######  await 到底在等啥？

**那await到底在等待什么呢？**

一般我们认为 await 是在等待一个 async 函数完成。不过按语法说明，await 等待的是一个表达式，这个表达式的结果是 Promise 对象或其它值。

因为 async 函数返回一个 Promise 对象，所以 await 可以用于等待一个 async 函数的返回值——这也可以说是 await 在等 async 函数。但要清楚，它等的实际是一个返回值。注意，await 不仅用于等 Promise 对象，它可以等任意表达式的结果。所以，await 后面实际是可以接普通函数调用或者直接量的。所以下面这个示例完全可以正确运行：

```js
function getSomething() {
    return "something";
}
async function testAsync() {
    return Promise.resolve("hello async");
}
async function test() {
    const v1 = await getSomething();
    const v2 = await testAsync();
    console.log(v1, v2);
}
test(); // something hello async
```

await 表达式的运算结果取决于它等的是什么：

- 如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的内容；
- 如果它等到的是一个 Promise 对象，await 就就会阻塞后面的代码，等着 Promise 对象 resolve，然后将得到的值作为 await 表达式的运算结果。

下面来看一个例子：

```js
function testAsy(x){
   return new Promise(resolve=>{setTimeout(() => {
       resolve(x);
     }, 3000)
    }
   )
}
async function testAwt(){    
  let result =  await testAsy('hello world');
  console.log(result);    // 3秒钟之后出现hello world
  console.log('cuger')   // 3秒钟之后出现cug
}
testAwt();
console.log('cug')  //立即输出cug
```

这就是 await 必须用在 async 函数中的原因。async 函数调用不会造成阻塞，它内部所有的阻塞都被封装在一个 Promise 对象中异步执行。await暂停当前async的执行，所以'cug''最先输出，hello world'和 cuger 是3秒钟后同时出现的。

###### async/await的优势

单一的 Promise 链并不能凸显 async/await 的优势。但是，如果处理流程比较复杂，那么整段代码将充斥着 then，语义化不明显，代码不能很好地表示执行流程，这时async/await的优势就能体现出来了。

假设一个业务，分多个步骤完成，每个步骤都是异步的，而且依赖于上一个步骤的结果。首先用 `setTimeout` 来模拟异步操作：

```js
/**
 * 传入参数 n，表示这个函数执行的时间（毫秒）
 * 执行的结果是 n + 200，这个值将用于下一步骤
 */
function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}
function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}
function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}
function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
```

现在用 Promise 方式来实现这三个步骤的处理：

```js
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}
doIt();
// c:\var\test>node --harmony_async_await .
// step1 with 300
// step2 with 500
// step3 with 700
// result is 900
// doIt: 1507.251ms
```

输出结果 `result` 是 `step3()` 的参数 `700 + 200` = `900`。`doIt()` 顺序执行了三个步骤，一共用了 `300 + 500 + 700 = 1500` 毫秒，和 `console.time()/console.timeEnd()` 计算的结果一致。

如果用 async/await 来实现呢，会是这样：

```js
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}
doIt();
```

结果和之前的 Promise 实现是一样的，但是这个代码看起来会清晰得多，几乎和同步代码一样。

async/await对比Promise的优势就显而易见了：

- 代码读起来更加同步，Promise虽然摆脱了回调地狱，但是then的链式调⽤也会带来额外的理解负担；
- Promise传递中间值很麻烦，⽽async/await⼏乎是同步的写法，⾮常优雅；
- 错误处理友好，async/await可以⽤成熟的try/catch，Promise的错误捕获比较冗余；
- 调试友好，Promise的调试很差，由于没有代码块，不能在⼀个返回表达式的箭头函数中设置断点，如果在⼀个.then代码块中使⽤调试器的步进(step-over)功能，调试器并不会进⼊后续的.then代码块，因为调试器只能跟踪同步代码的每⼀步。

###### async/await 的异常处理

利用 async/await 的语法糖，可以像处理同步代码的异常一样，来处理异步代码，这里还用上面的示例：

```js
const exe = (flag) => () => new Promise((resolve, reject) => {
    console.log(flag);
    setTimeout(() => {
        flag ? resolve("yes") : reject("no");
    }, 1000);
});


const run = async () => {
 try {
  await exe(false)();
  await exe(true)();
 } catch (e) {
  console.log(e);
 }
}
run();
这里定义一
```

这里定义一个异步方法 run，由于 await 后面需要直接跟 Promise 对象，因此通过额外的一个方法调用符号 () 把原有的 exe 方法内部的 Thunk 包装拆掉，即执行 exe(false)() 或 exe(true)() 返回的就是 Promise 对象。在 try 块之后，使用 catch 来捕捉。运行代码会得到这样的输出：

```js
false
no
```

这个 false 就是 exe 方法对入参的输出，而这个 no 就是 setTimeout 方法 reject 的回调返回，它通过异常捕获并最终在 catch 块中输出。就像我们所认识的同步代码一样，第四行的 exe(true) 并未得到执行。

###  `Iterator`

> Iterator（迭代器）是一种接口，也可以说是一种规范。为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

#### Iterator语法

```js
const obj = {
    [Symbol.iterator]:function(){}
}
```

> `[Symbol.iterator]` 属性名是固定的写法，只要拥有了该属性的对象，就能够用迭代器的方式进行遍历。

> 迭代器的遍历方法是首先获得一个迭代器的指针，初始时该指针指向第一条数据之前，接着通过调用 next 方法，改变指针的指向，让其指向下一条数据 每一次的 next 都会返回一个对象，该对象有两个属性
>
> - value 代表想要获取的数据
> - done 布尔值，false表示当前指针指向的数据有值，true表示遍历已经结束

> **Iterator 的作用有三个：**
>
> 1. 为各种数据结构，提供一个统一的、简便的访问接口；
> 2. 使得数据结构的成员能够按某种次序排列；
> 3. ES6 创造了一种新的遍历命令for…of循环，Iterator 接口主要供for…of消费。

> **遍历过程：**
>
> 1. 创建一个指针对象，指向当前数据结构的起始位置。也就是说，遍历器对象本质上，就是一个指针对象。
>
> 2. 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
>
> 3. 第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
>
> 4. 不断调用指针对象的next方法，直到它指向数据结构的结束位置。
>
>    每一次调用next方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含value和done两个属性的对象。其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。

```js
let arr = [{num:1},2,3]
let it = arr[Symbol.iterator]() // 获取数组中的迭代器
console.log(it.next())  // { value: Object { num: 1 }, done: false }
console.log(it.next())  // { value: 2, done: false }
console.log(it.next())  // { value: 3, done: false }
console.log(it.next())  // { value: undefined, done: true }
```

### `Generator`函数

> Generator函数可以说是Iterator接口的具体实现方式。Generator 最大的特点就是可以控制函数的执行。

```js
function *foo(x) {
  let y = 2 * (yield (x + 1))
  let z = yield (y / 3)
  return (x + y + z)
}
let it = foo(5)
console.log(it.next())   // => {value: 6, done: false}
console.log(it.next(12)) // => {value: 8, done: false}
console.log(it.next(13)) // => {value: 42, done: true}
```

> 上面这个示例就是一个Generator函数，我们来分析其执行过程：
>
> - `首先 Generator 函数调用时它会返回一个迭代器`
> - `当执行第一次 next 时，传参会被忽略，并且函数暂停在 yield (x + 1) 处，所以返回 5 + 1 = 6`
> - `当执行第二次 next 时，传入的参数等于上一个 yield 的返回值，如果你不传参，yield 永远返回 undefined。此时 let y = 2 * 12，所以第二个 yield 等于 2 * 12 / 3 = 8`
> - `当执行第三次 next 时，传入的参数会传递给 z，所以 z = 13, x = 5, y = 24，相加等于 42`
>
> `Generator` 函数一般见到的不多，其实也于他有点绕有关系，并且一般会配合 co 库去使用。当然，我们可以通过 `Generator` 函数解决回调地狱的问题。

### `async/await`

> `async/await`是一种建立在Promise之上的编写异步或非阻塞代码的新方法，被普遍认为是 JS异步操作的最终且最优雅的解决方案。相对于 Promise 和回调，它的可读性和简洁度都更高。毕竟一直then()也很烦。
>
> `async` 是异步的意思，而 `await` 是 `async wait`的简写，即异步等待。
>
> 所以从语义上就很好理解 async 用于声明一个 function 是异步的，而await 用于等待一个异步方法执行完成。
>
> 一个函数如果加上 async ，那么该函数就会返回一个 Promise

```js
async function test() {
  return "1"
}
console.log(test()) // -> Promise {<resolved>: "1"}

// 可以看到输出的是一个Promise对象。所以，async 函数返回的是一个 Promise 对象，如果在 async 函数中直接 return 一个直接量，async 会把这个直接量通过 PromIse.resolve() 封装成Promise对象返回。
```

```js
// 相比于 Promise，async/await能更好地处理 then 链

function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}

function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}

function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}


//现在分别用 Promise 和async/await来实现这三个步骤的处理。
```

```js
// 使用Promise

function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
        });
}
doIt();
// step1 with 300
// step2 with 500
// step3 with 700
// result is 900
```

```js
// 使用async/await

async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
}
doIt();

// 结果和之前的 Promise 实现是一样的，但是这个代码看起来是不是清晰得多，优雅整洁，几乎跟同步代码一样。
```

> await关键字只能在async function中使用。在任何非async function的函数中使用await关键字都会抛出错误。await关键字在执行下一行代码之前等待右侧表达式(可能是一个Promise)返回。

#### 优缺点

```
`async/await`的优势在于处理 then 的调用链，能够更清晰准确的写出代码，并且也能优雅地解决回调地狱问题。当然也存在一些缺点，因为 await 将异步代码改造成了同步代码，如果多个异步代码没有依赖性却使用了 await 会导致性能上的降低。
```

> 「硬核JS」深入了解异步解决方案