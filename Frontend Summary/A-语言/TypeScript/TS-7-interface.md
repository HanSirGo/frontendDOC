### interface 接口
#### 1. interface 接口
interface 是对象的模板，可以看作是一种类型约定，中文译为“接口”。使用了某个模板的对象，就拥有了指定的类型结构。
```javascript
interface Person {
  firstName: string;
  lastName: string;
  age: number;
}
```
> 上面示例中，定义了一个接口Person，它指定一个对象模板，拥有三个属性firstName、lastName和age。任何实现这个接口的对象，都必须部署这三个属性，并且必须符合规定的类型。

实现该接口很简单，只要指定它作为对象的类型即可。
```javascript
const p:Person = {
  firstName: 'John',
  lastName: 'Smith',
  age: 25
};
```
> 上面示例中，变量p的类型就是接口Person，所以必须符合Person指定的结构。

方括号运算符可以取出 interface 某个属性的类型。
```javascript
interface Foo {
  a: string;
}

type A = Foo['a']; // string
```
> 上面示例中，Foo['a']返回属性a的类型，所以类型A就是string。

**interface 可以表示对象的各种语法，它的成员有5种形式。**
		
 - 对象属性
 - 对象的属性索引 		
 - 对象方法 		
 - 函数 		
 - 构造函数
 - **（1）对象属性**
```javascript
interface Point {
  x: number;
  y: number;
}
```
> 上面示例中，x和y都是对象的属性，分别使用冒号指定每个属性的类型。

属性之间使用分号或逗号分隔，最后一个属性结尾的分号或逗号可以省略。

如果属性是可选的，就在属性名后面加一个问号。
```javascript
interface Foo {
  x?: string;
}
```
如果属性是只读的，需要加上readonly修饰符。
```javascript
interface A {
  readonly a: string;
}
```
 - **（2）对象的属性索引**
```javascript
interface A {
  [prop: string]: number;
}
```
> 上面示例中，[prop: string]就是属性的字符串索引，表示属性名只要是字符串，都符合类型要求。

属性索引共有string、number和symbol三种类型。

一个接口中，最多只能定义一个字符串索引。字符串索引会约束该类型中所有名字为字符串的属性。
```javascript
interface MyObj {
  [prop: string]: number;

  a: boolean;      // 编译错误
}
```
> 上面示例中，属性索引指定所有名称为字符串的属性，它们的属性值必须是数值（number）。属性a的值为布尔值就报错了。

属性的数值索引，其实是指定数组的类型。
```javascript
interface A {
  [prop: number]: string;
}

const obj:A = ['a', 'b', 'c'];
```
> 上面示例中，[prop: number]表示属性名的类型是数值，所以可以用数组对变量obj赋值。

同样的，一个接口中最多只能定义一个数值索引。数值索引会约束所有名称为数值的属性。

**如果一个 interface 同时定义了字符串索引和数值索引，那么数值索引必须服从于字符串索引。因为在 JavaScript 中，数值属性名最终是自动转换成字符串属性名。**
```javascript
interface A {
  [prop: string]: number;
  [prop: number]: string; // 报错
}

interface B {
  [prop: string]: number;
  [prop: number]: number; // 正确
}
```
> 上面示例中，数值索引的属性值类型与字符串索引不一致，就会报错。数值索引必须兼容字符串索引的类型声明。

 - **（3）对象的方法**

对象的方法共有三种写法。
```javascript
// 写法一
interface A {
  f(x: boolean): string;
}

// 写法二
interface B {
  f: (x: boolean) => string;
}

// 写法三
interface C {
  f: { (x: boolean): string };
}
```
属性名可以采用表达式，所以下面的写法也是可以的。
```javascript
const f = 'f';

interface A {
  [f](x: boolean): string;
}
```
类型方法可以重载。
```javascript
interface A {
  f(): number;
  f(x: boolean): boolean;
  f(x: string, y: string): string;
}
```
interface 里面的函数重载，不需要给出实现。但是，由于对象内部定义方法时，无法使用函数重载的语法，所以需要额外在对象外部给出函数方法的实现。
```javascript
interface A {
  f(): number;
  f(x: boolean): boolean;
  f(x: string, y: string): string;
}

function MyFunc(): number;
function MyFunc(x: boolean): boolean;
function MyFunc(x: string, y: string): string;
function MyFunc(
  x?:boolean|string, y?:string
):number|boolean|string {
  if (x === undefined && y === undefined) return 1;
  if (typeof x === 'boolean' && y === undefined) return true;
  if (typeof x === 'string' && typeof y === 'string') return 'hello';
  throw new Error('wrong parameters');  
}

const a:A = {
  f: MyFunc
}
```
> 上面示例中，接口A的方法f()有函数重载，需要额外定义一个函数MyFunc()实现这个重载，然后部署接口A的对象a的属性f等于函数MyFunc()就可以了。

 - **（4）函数**

interface 也可以用来声明独立的函数。
```javascript
interface Add {
  (x:number, y:number): number;
}

const myAdd:Add = (x,y) => x + y;
```
> 上面示例中，接口Add声明了一个函数类型。

 - **（5）构造函数**

interface 内部可以使用new关键字，表示构造函数。
```javascript
interface ErrorConstructor {
  new (message?: string): Error;
}
```
> 上面示例中，接口ErrorConstructor内部有new命令，表示它是一个构造函数。

> TypeScript 里面，构造函数特指具有constructor属性的类，详见《Class》一章。
#### 2. interface 的继承
interface 可以继承其他类型，主要有下面几种情况。
##### 2.1 interface 继承 interface
**interface 可以使用extends关键字，继承其他 interface。**
```javascript
interface Shape {
  name: string;
}

interface Circle extends Shape {
  radius: number;
}
```
> 上面示例中，Circle继承了Shape，所以Circle其实有两个属性name和radius。这时，Circle是子接口，Shape是父接口。

extends关键字会从继承的接口里面拷贝属性类型，这样就不必书写重复的属性。

**interface 允许多重继承。**
```javascript
interface Style {
  color: string;
}

interface Shape {
  name: string;
}

interface Circle extends Style, Shape {
  radius: number;
}
```
> 上面示例中，Circle同时继承了Style和Shape，所以拥有三个属性color、name和radius。

多重接口继承，实际上相当于多个父接口的合并。

**如果子接口与父接口存在同名属性，那么子接口的属性会覆盖父接口的属性。注意，子接口与父接口的同名属性必须是类型兼容的，不能有冲突，否则会报错。**
```javascript
interface Foo {
  id: string;
}

interface Bar extends Foo {
  id: number; // 报错
}
```
> 上面示例中，Bar继承了Foo，但是两者的同名属性id的类型不兼容，导致报错。

**多重继承时，如果多个父接口存在同名属性，那么这些同名属性不能有类型冲突，否则会报错。**
```javascript
interface Foo {
  id: string;
}

interface Bar {
  id: number;
}

// 报错
interface Baz extends Foo, Bar {
  type: string;
}
```
> 上面示例中，Baz同时继承了Foo和Bar，但是后两者的同名属性id有类型冲突，导致报错。
##### 2.2 interface 继承 type
**interface 可以继承type命令定义的*对象类型*。**
```javascript
type Country = {
  name: string;
  capital: string;
}

interface CountryWithPop extends Country {
  population: number;
}
```
> 上面示例中，CountryWithPop继承了type命令定义的Country对象，并且新增了一个population属性。

**注意，如果type命令定义的类型不是对象，interface 就无法继承。**
##### 2.3 interface 继承 class
interface 还可以继承 class，即继承该类的所有成员。关于 class 的详细解释，参见下一章。
```javascript
class A {
  x:string = '';

  y():boolean {
    return true;
  }
}

interface B extends A {
  z: number
}
```
> 上面示例中，B继承了A，因此B就具有属性x、y()和z。

实现B接口的对象就需要实现这些属性。
```javascript
const b:B = {
  x: '',
  y: function(){ return true },
  z: 123
}
```
> 上面示例中，对象b就实现了接口B，而接口B又继承了类A。

某些类拥有私有成员和保护成员，interface 可以继承这样的类，但是意义不大。
```javascript
class A {
  private x: string = '';
  protected y: string = '';
}

interface B extends A {
  z: number
}

// 报错
const b:B = { /* ... */ }

// 报错
class C implements B {
  // ...
}
```
> 上面示例中，A有私有成员和保护成员，B继承了A，但无法用于对象，因为对象不能实现这些成员。这导致B只能用于其他 class，而这时其他 class 与A之间不构成父类和子类的关系，使得x与y无法部署。
#### 3. 接口合并
**多个同名接口会合并成一个接口。**
```javascript
interface Box {
  height: number;
  width: number;
}

interface Box {
  length: number;
}
```
> 上面示例中，两个Box接口会合并成一个接口，同时有height、width和length三个属性。

这样的设计主要是为了兼容 JavaScript 的行为。JavaScript 开发者常常对全局对象或者外部库，添加自己的属性和方法。那么，只要使用 interface 给出这些自定义属性和方法的类型，就能自动跟原始的 interface 合并，使得扩展外部类型非常方便。

举例来说，Web 网页开发经常会对window对象和document对象添加自定义属性，但是 TypeScript 会报错，因为原始定义没有这些属性。解决方法就是把自定义属性写成 interface，合并进原始定义。
```javascript
interface Document {
  foo: string;
}

document.foo = 'hello';
```
> 上面示例中，接口Document增加了一个自定义属性foo，从而就可以在document对象上使用自定义属性。

**同名接口合并时，同一个属性如果有多个类型声明，彼此不能有类型冲突。**
```javascript
interface A {
  a: number;
}

interface A {
  a: string; // 报错
}
```
> 上面示例中，接口A的属性a有两个类型声明，彼此是冲突的，导致报错。

**同名接口合并时，如果同名方法有不同的类型声明，那么会发生函数重载。而且，后面的定义比前面的定义具有更高的优先级。**
```javascript
interface Cloner {
  clone(animal: Animal): Animal;
}

interface Cloner {
  clone(animal: Sheep): Sheep;
}

interface Cloner {
  clone(animal: Dog): Dog;
  clone(animal: Cat): Cat;
}

// 等同于
interface Cloner {
  clone(animal: Dog): Dog;
  clone(animal: Cat): Cat;
  clone(animal: Sheep): Sheep;
  clone(animal: Animal): Animal;
}
```
> 上面示例中，clone()方法有不同的类型声明，会发生函数重载。这时，越靠后的定义，优先级越高，排在函数重载的越前面。比如，clone(animal: Animal)是最先出现的类型声明，就排在函数重载的最后，属于clone()函数最后匹配的类型。

**这个规则有一个例外。同名方法之中，如果有一个参数是字面量类型，字面量类型有更高的优先级。**
```javascript
interface A {
  f(x:'foo'): boolean;
}

interface A {
  f(x:any): void;
}

// 等同于
interface A {
  f(x:'foo'): boolean;
  f(x:any): void;
}
```
> 上面示例中，f()方法有一个类型声明的参数x是字面量类型，这个类型声明的优先级最高，会排在函数重载的最前面。

一个实际的例子是 Document 对象的createElement()方法，它会根据参数的不同，而生成不同的 HTML 节点对象。
```javascript
interface Document {
  createElement(tagName: any): Element;
}
interface Document {
  createElement(tagName: "div"): HTMLDivElement;
  createElement(tagName: "span"): HTMLSpanElement;
}
interface Document {
  createElement(tagName: string): HTMLElement;
  createElement(tagName: "canvas"): HTMLCanvasElement;
}

// 等同于
interface Document {
  createElement(tagName: "canvas"): HTMLCanvasElement;
  createElement(tagName: "div"): HTMLDivElement;
  createElement(tagName: "span"): HTMLSpanElement;
  createElement(tagName: string): HTMLElement;
  createElement(tagName: any): Element;
}
```
> 上面示例中，createElement()方法的函数重载，参数为字面量的类型声明会排到最前面，返回具体的 HTML 节点对象。类型越不具体的参数，排在越后面，返回通用的 HTML 节点对象。

**如果两个 interface 组成的联合类型存在同名属性，那么该属性的类型也是联合类型。**
```javascript
interface Circle {
  area: bigint;
}

interface Rectangle {
  area: number;
}

declare const s: Circle | Rectangle;

s.area;   // bigint | number
```
> 上面示例中，接口Circle和Rectangle组成一个联合类型Circle | Rectangle。因此，这个联合类型的同名属性area，也是一个联合类型。本例中的declare命令表示变量s的具体定义，由其他脚本文件给出，详见《declare 命令》一章。
#### 4. interface 与 type 的异同
**interface命令与type命令作用类似，都可以表示对象类型。**

**很多对象类型既可以用 interface 表示，也可以用 type 表示。而且，两者往往可以换用，几乎所有的 interface 命令都可以改写为 type 命令。**

它们的相似之处，首先表现在都能为对象类型起名。
```javascript
type Country = {
  name: string;
  capital: string;
}

interface Country {
  name: string;
  capital: string;
}
```
> 上面示例是type命令和interface命令，分别定义同一个类型。

**class命令也有类似作用，通过定义一个类，同时定义一个对象类型。但是，它会创造一个值，编译后依然存在。如果只是单纯想要一个类型，应该使用type或interface。**

##### **interface 与 type 的区别有下面几点。**

 - **（1）type能够表示非对象类型，而interface只能表示对象类型（包括数组、函数等）。**

 - **（2）interface可以继承其他类型，type不支持继承。**

继承的主要作用是添加属性，type定义的对象类型如果想要添加属性，只能使用&运算符，重新定义一个类型。
```javascript
type Animal = {
  name: string
}

type Bear = Animal & {
  honey: boolean
}
```
> 上面示例中，类型Bear在Animal的基础上添加了一个属性honey。

上例的&运算符，表示同时具备两个类型的特征，因此可以起到两个对象类型合并的作用。

作为比较，interface添加属性，采用的是继承的写法。
```javascript
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}
```
继承时，type 和 interface 是可以换用的。interface 可以继承 type。
```javascript
type Foo = { x: number; };

interface Bar extends Foo {
  y: number;
}
type 也可以继承 interface。

interface Foo {
  x: number;
}

type Bar = Foo & { y: number; };
```
 - **（3）同名interface会自动合并，同名type则会报错。也就是说，TypeScript 不允许使用type多次定义同一个类型。**
```javascript
type A = { foo:number }; // 报错
type A = { bar:number }; // 报错
```
> 上面示例中，type两次定义了类型A，导致两行都会报错。

作为比较，interface则会自动合并。
```javascript
interface A { foo:number };
interface A { bar:number };

const obj:A = {
  foo: 1,
  bar: 1
};
```
> 上面示例中，interface把类型A的两个定义合并在一起。

这表明，interface 是开放的，可以添加属性，type 是封闭的，不能添加属性，只能定义新的 type。

 - **（4）interface不能包含属性映射（mapping），type可以，详见《映射》一章。**
```javascript
interface Point {
  x: number;
  y: number;
}

// 正确
type PointCopy1 = {
  [Key in keyof Point]: Point[Key];
};

// 报错
interface PointCopy2 {
  [Key in keyof Point]: Point[Key];
};
```
 - **（5）this关键字只能用于interface。**
```javascript
// 正确
interface Foo {
  add(num:number): this;
};

// 报错
type Foo = {
  add(num:number): this;
};
```
> 上面示例中，type 命令声明的方法add()，返回this就报错了。interface 命令没有这个问题。

下面是返回this的实际对象的例子。
```javascript
class Calculator implements Foo {
  result = 0;
  add(num:number) {
    this.result += num;
    return this;
  }
}
```
 - **（6）type 可以扩展原始数据类型，interface 不行。**
```javascript
// 正确
type MyStr = string & {
  type: 'new'
};

// 报错
interface MyStr extends string {
  type: 'new'
}
```
> 上面示例中，type 可以扩展原始数据类型 string，interface 就不行。

 - **（7）interface无法表达某些复杂类型（比如交叉类型和联合类型），但是type可以。**
```javascript
type A = { /* ... */ };
type B = { /* ... */ };

type AorB = A | B;
type AorBwithName = AorB & {
  name: string
};
```
> 上面示例中，类型AorB是一个联合类型，AorBwithName则是为AorB添加一个属性。这两种运算，interface都没法表达。

综上所述，如果有复杂的类型运算，那么没有其他选择只能使用type；一般情况下，interface灵活性比较高，便于扩充类型或自动合并，建议优先使用。