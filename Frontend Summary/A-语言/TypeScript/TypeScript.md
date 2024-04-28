## TypeScript

| JS                         | TS                               |
| -------------------------- | -------------------------------- |
| 动态语言                   | 具有静态语言的特点               |
| 编译性语言运行时报错       | 编译期间报错                     |
| 弱类型语言，没有类型       | 强类型语言                       |
| 不支持模块、接口、泛型     | 支持模块、接口、泛型             |
| 基本数据类型、引用数据类型 | 更多的基本数据类型、引用数据类型 |
| 在浏览器中直接执行         | 编译为js后才能在浏览器执行       |



```ts
# void类型
// 严格模式下 可以将 undefined 赋值给 void类型的变量
// 非严格模式下 可以将 null、undefined 赋值给void类型的变量

# null undefined
// 非严格模式下 可以将 null、undefiend赋值给number string类型的变量

# 枚举
// 枚举类型的本质就是数值类型，所以赋值数值不会报错
// 枚举类型的取值默认从0开始，自上而下递增

# 问题1：

interface Person {
    name: string
    age: number
}
let pa:Person = {
    name:'zs',
    age:18,
    height:180
}
// 接口中没有height属性，所以会报错
// 怎么解决
// 1. 使用变量（不推荐）
let info = {
    name:'zs',
    age:18,
    height:180
}
let pa:Person = info
// 2. 类型断言
let pa:Person = {
    name:'zs',
    age:18,
    height:180
} as Person
// 3. 索引签名 [props:string]:string
interface Person {
    name: string
    age: number
    [props:string]:string
}
let pa:Person = {
    name:'zs',
    age:18,
    height:180
}
```

#### class

```ts
# static 关键字
// 用于定义类的数据成员为静态的，静态成员可以直接通过类名调用。
class T {
    static name: string
    static say():void {
        T.name
    }
}
T.name
T.say()

# instanceof 运算符
// 用于判断对象是否是指定的类型，是返回true 否则false
class Person {}
let p = new Person()
p instenceof Person // p是Person的实例 true

class People extends Person {}
let s = new People()
s instenceof Person // true

# 类修饰符
// public(默认) : 公有,可以在任何地方被访问
// protected : 受保护的,可以被其自身及其子类访问
// private : 私有,只能被其定义所在的类访问
// readonly : 可以使用readonly关键字将属性设置为只读的,只读属性必须在声明时或构造函数里被初始化

# 如果给 constructor 添加 protected 会怎样？
class Person {
    protected constructor(){}
}
class Student extends Person {
    
}
// Person 不能使用new，Student可以使用new
// 这样可以定义一个基类，

# 存取器 getter setter
// 通过 getter、setter来截取对对象成员的访问
// 注意：如果存在get但是没有set则该属性自动是只读的。
// 		如果没有指定setter参数的类型，它将从getter的返回类型中推断出来
// 		访问器和设置器必须有相同的成员可见性

# 抽象类
// 抽象类作为其他派生类的基类使用，一般不会直接实例化
// 抽象类是专门用于定义那些不希望被外界直接创建的类
// 抽象类和接口一样用于约束子类
// 抽象类与接口的区别：
// 抽象类必须包含 abstract 关键字并且可以包含访问修饰符
// 接口只能定义约束，不能定义具体实现。而抽象类中既可以定义约束也可以定义具体实现
abstract class Person {
    abstract name: string;
    abstract say():string;
    
    showName() {
        
    }
}
class Student extends Person {
    name:string = 'x'
    show(){ }
}
let p = new Person('x') // 报错 这个和constructor前边加 protected 作用一样
```

```ts
# implements 子句
// 类可以实现接口，使用关键字 implements
// 可以使用implements子句来检查一个类，是否满足了一个特定的接口，如果一个类不能正确的实现它，就会发出一个错误
// 注意：
// 实现一个带有可选属性的接口并不能创建该属性
// 只要一个接口继承了某个类，那么就会继承这个类中所有的属性和方法，但是只会继承属性和方法的声明，不会继承属性和方法的实现
// 与extends区别：
// extends继承某个类，继承之后可以使用父类的方法，也可以重写父类的方法
// implements继承某个类，必须重写才可以使用

// interface IPer extends Person {}
// class Person implements IPer {}
// class Person extends People implements IPer {}
```

```ts
# 类的初始化顺序
// 基类的字段被初始化
// 基类构造函数运行
// 子类的字段被初始化
// 子类构造函数运行
```

#### 条件类型

```ts
// 条件类型优化函数重载
interface IName = {
    name:string
}
interface IAge = {
    age:number
}

// 优化前
function reload(name:string):IName
function reload(age:number):IAge
function reload(nameorage:string|number):IName|IAge
function reload(nameorage:string|number):IName|IAge {
    throw ""
}
// 优化后
type Params<T> = T extends string ? IName : IAge
function reload<T extends number | string>(nameorage:T):Params<T> {
    throw ""
}
```

#### infer关键字

```ts
// 推断类型
type FOO<T> = T extends {a: infer U; b: infer U} ? U:never

type TT = FOO<{a:string;b:number}>
// TT string|number
```

#### TS兼容性

##### 自动类型推论

```ts
// 1.根据初始值进行类型推论
let a = 123
a = '123' // 会报错 不能将string类型赋值给number类型

// 初始化时不赋值，ts不会进行类型推论
let a;
a = 123
a = '123'

// 2.上下文类型推论
// mouseEvent 已经 有了具体类型
window.onmousedown = function(mouseEvent) {
    mouseEvent.button
}
```

##### 对象类型的兼容性

```ts
// 1.可多不可少

interface IPerson {
    name: string
}

let p1 = { name:'li' }
let p2 = { name:'wa',age:18 }
let p3 = { age:19 }

let p0:IPerson;
p0 = p1
p0 = p2
p0 = p3 // 报错，类型{name:string}中缺少 name属性

// 2.会进行递归检查

interface IPerson {
    name: string
    children: {
        age: number
    }
}

let p1 = {name:'li',children:{age:18}}
let p2 = {name:'wa',children:{age:true}}

let p0:IPerson;

let p0 = p1
let p0 = p2// children.age的类型不兼容，不能将boolena类型赋值给number类型
```

##### 函数类型的兼容性

```ts
// 1.参数个数 : 可少不可多，参数多的兼容参数少的  

let fn1 = (a:number,b:number)=>{}
let fn2 = (a:number)=>{}

fn1 = fn2
fn2 = fn1 // 报错

// 2.参数类型 : 参数类型必须相同

let fn1 = (a:number)=>{}
let fn2 = (a:number)=>{}
let fn3 = (a:number)=>{}

fn1 = fn2
fn2 = fn1
fn1 = fn3 // 报错

// 3.参数返回值 : 返回值类型必须相同

let fn1 = ():number=> 1
let fn2 = ():number=> 2
let fn3 = ():string=> '3'

fn1 = fn2
fn2 = fn1
fn1 = fn3 // 报错

// 4.双向协变
// a. 参数的双向协变 : 参数多的赋值给参数少的

let fn1 = (a:number)=>{}
let fn2 = (a:(number|string))=>{}

fn1 = fn2
fn2 = fn1 // 报错

// b. 返回值的双向协变 : 可以将返回值是具体类型的赋值给联合类型的函数

let fn1 = (x:boolean):(number|string)=> x?1:'1'
let fn2 = (x:boolean):number => 2

fn1 = fn2
fn2 = fn1 // 报错

// 5.函数重载 : 重载多的赋值给重载少的

function fn(x:number, y:number):number
function fn(x:string, y:string):string
function fn(x:any, y:any) {
    return x + y
}

function fn1(x:number, y:number):number
function fn1(x:any, y:any) {
    return x - y
}

let a = fn
a = fn1 // 报错

let b = fn1
b = fn

// 6.可选参数及剩余参数 : 当一个函数有剩余参数的时候，它被当做无限个可选参数

function fn(args:any[], callback:(...args:any[])=>void){
    
}

fn([1,2], (x,y,z)=> console.log(x+y+z))
fn([1,2], (x?,y?)=> console.log(x+y))
fn([1,2], (x,y?,z?)=> console.log(x+y+z))
```

##### 枚举类型兼容性

```ts
// 1. 数字枚举与数字兼容
enum Gender {
    Male,
    Female
}
let a:Gender;
a = 100

// 2. 数字枚举与数字枚举不兼容
enum Gender {
    Male,
    Female
}
enum Animal {
    Dog,
    Cat
}
let a:Gender;
a = Gender.Male
a = Animal.Dog // 报错

// 3. 字符串枚举与字符串不兼容
enum Gender {
    Male='男',
    Female='女'
}

let a:Gender;
a = Gender.Male
a = Gender.Female
a = 'xixi' // 报错
```

##### 类兼容性

```ts
public 可多不可少
private / protected 不能相互赋值
```

##### 泛型的兼容性

```ts
// TS是一个结构化的类型系统,类型参数只在作为成员类型的一部分被消耗是影响到结果类型

interface IP<T> {}
let x: IP<number>
let y: IP<string>
x = y
y = x

interface IP<T> {
    data: T
}
let x: IP<number>
let y: IP<string>
x = y // 报错
y = x // 报错
```

