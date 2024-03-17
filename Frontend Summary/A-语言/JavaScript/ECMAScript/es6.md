## ECMAScript

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

##### 1. 回调函数

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps55.jpg) 

##### 2. Promise

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps56.jpg) 

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml1892\wps57.jpg) 

##### 3. Generator函数 

##### 4. async/await 函数  

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