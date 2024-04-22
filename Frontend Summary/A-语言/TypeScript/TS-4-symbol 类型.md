### symbol 类型
#### 1. symbol 类型
Symbol 是 ES2015 新引入的一种原始类型的值。它类似于字符串，但是每一个 Symbol 值都是独一无二的，与其他任何值都不相等。

**Symbol 值通过Symbol()函数生成。在 TypeScript 里面，Symbol 的类型使用symbol表示。**
```javascript
let x:symbol = Symbol();
let y:symbol = Symbol();

x === y // false
```
> 上面示例中，变量x和y的类型都是symbol，且都用Symbol()生成，但是它们是不相等的。
#### 2. unique symbol
symbol类型包含所有的 Symbol 值，但是无法表示某一个具体的 Symbol 值。

比如，5是一个具体的数值，就用5这个字面量来表示，这也是它的值类型。但是，**Symbol 值不存在字面量，必须通过变量来引用，所以写不出只包含单个 Symbol 值的那种值类型。**

为了解决这个问题，TypeScript 设计了symbol的一个子类型unique symbol，它表示单个的、某个具体的 Symbol 值。

因为**unique symbol表示单个值，所以这个类型的变量是不能修改值的，只能用const命令声明，不能用let声明。**
```javascript
// 正确
const x:unique symbol = Symbol();

// 报错
let y:unique symbol = Symbol();
```
> 上面示例中，let命令声明的变量，不能是unique symbol类型，会报错。

**const命令为变量赋值 Symbol 值时，变量类型默认就是unique symbol，所以类型可以省略不写。**
```javascript
const x:unique symbol = Symbol();
// 等同于
const x = Symbol();
```
每个声明为unique symbol类型的变量，它们的值都是不一样的，其实属于两个值类型。
```javascript
const a:unique symbol = Symbol();
const b:unique symbol = Symbol();

a === b // 报错
```
> 上面示例中，变量a和变量b的类型虽然都是unique symbol，但其实是两个值类型。不同类型的值肯定是不相等的，所以最后一行就报错了。

由于 Symbol 类似于字符串，可以参考下面的例子来理解。
```javascript
const a:'hello' = 'hello';
const b:'world' = 'world';

a === b // 报错
```
> 上面示例中，变量a和b都是字符串，但是属于不同的值类型，不能使用严格相等运算符进行比较。而且，由于变量a和b是两个类型，就不能把一个赋值给另一个。
```javascript
const a:unique symbol = Symbol();
const b:unique symbol = a; // 报错
```
> 上面示例中，变量a和变量b的类型都是unique symbol，但是其实类型不同，所以把a赋值给b会报错。

上例变量b的类型，如果要写成与变量a同一个unique symbol值类型，只能写成类型为typeof a。
```javascript
const a:unique symbol = Symbol();
const b:typeof a = a; // 正确
```
不过我们知道，相同参数的Symbol.for()方法会返回相同的 Symbol 值。TypeScript 目前无法识别这种情况，所以可能出现多个 unique symbol 类型的变量，等于同一个 Symbol 值的情况。
```javascript
const a:unique symbol = Symbol.for('foo');
const b:unique symbol = Symbol.for('foo');
```
> 上面示例中，变量a和b是两个不同的值类型，但是它们的值其实是相等的。

**unique symbol 类型是 symbol 类型的子类型，所以可以将前者赋值给后者，但是反过来就不行。**
```javascript
const a:unique symbol = Symbol();

const b:symbol = a; // 正确

const c:unique symbol = b; // 报错
```
> 上面示例中，unique symbol 类型（变量a）赋值给 symbol 类型（变量b）是可以的，但是 symbol 类型（变量b）赋值给 unique symbol 类型（变量c）会报错。

unique symbol 类型的一个作用，就是用作属性名，这可以保证不会跟其他属性名冲突。如果要把某一个特定的 Symbol 值当作属性名，那么它的类型只能是 unique symbol，不能是 symbol。
```javascript
const x:unique symbol = Symbol();
const y:symbol = Symbol();

interface Foo {
  [x]: string; // 正确
  [y]: string; // 报错
}
```
> 上面示例中，变量y当作属性名，但是y的类型是 symbol，不是固定不变的值，导致报错。

unique symbol类型也可以用作类（class）的属性值，但只能赋值给类的readonly static属性。
```javascript
class C {
  static readonly foo:unique symbol = Symbol();
}
```
> 上面示例中，静态只读属性foo的类型就是unique symbol。注意，这时static和readonly两个限定符缺一不可，这是为了保证这个属性是固定不变的。
#### 3. 类型推断
如果变量声明时没有给出类型，TypeScript 会推断某个 Symbol 值变量的类型。

**let命令声明的变量，推断类型为 symbol。**
```javascript
// 类型为 symbol
let x = Symbol();
```
**const命令声明的变量，推断类型为 unique symbol。**
```javascript
// 类型为 unique symbol
const x = Symbol();
```
**但是，const命令声明的变量，如果赋值为另一个 symbol 类型的变量，则推断类型为 symbol。**
```javascript
let x = Symbol();

// 类型为 symbol
const y = x;
```
**let命令声明的变量，如果赋值为另一个 unique symbol 类型的变量，则推断类型还是 symbol。**
```javascript
const x = Symbol();

// 类型为 symbol
let y = x;
```