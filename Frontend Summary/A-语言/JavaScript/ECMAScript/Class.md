# 在创建对象的时候， new class 和 new function 可有什么区别？

在 JavaScript 中，使用 `new` 关键字创建对象时，不论是通过类（`class`）还是通过构造函数（`function`），都会有一些相似之处和一些不同之处。以下是它们的区别和相似点：

### 相似点

1. **原型继承**:

2. - 不论是 `new class` 还是 `new function`，最终创建的对象都会继承自它们的原型（`prototype`）。
   - 这意味着创建的对象可以访问定义在原型链上的方法和属性。

3. **this 绑定**:

4. - 在构造函数（`function`）和类（`class`）中，`this` 会自动绑定到新创建的对象上。
   - 因此，在构造函数或类的构造器（`constructor`）中，`this` 指向新创建的对象。

5. **返回值**:

6. - 如果构造函数或类的构造器没有显式地返回一个对象，那么 `new` 调用会自动返回 `this`，即新创建的对象。
   - 如果显式返回了一个对象，那么这个对象会作为 `new` 调用的返回值，而非新创建的对象。

### 区别

1. **语法**:

   ```
   // Class 语法
   class MyClass {
     constructor(name) {
       this.name = name;
     }
   
     greet() {
       console.log(`Hello, ${this.name}`);
     }
   }
   
   // Function 构造函数语法
   function MyFunction(name) {
     this.name = name;
   }
   
   MyFunction.prototype.greet = function() {
     console.log(`Hello, ${this.name}`);
   }
   ```

2. - `class` 是在 ECMAScript 2015 (ES6) 中引入的一个语法糖，提供了更结构化的方式来定义构造函数和继承。
   - `function` 是 JavaScript 中更早期的构造函数定义方式。

3. **静态方法和属性**:

   ```
   // Class 语法
   class MyClass {
     constructor(name) {
       this.name = name;
     }
   
     greet() {
       console.log(`Hello, ${this.name}`);
     }
   }
   
   // Function 构造函数语法
   function MyFunction(name) {
     this.name = name;
   }
   
   MyFunction.prototype.greet = function() {
     console.log(`Hello, ${this.name}`);
   }
   ```

4. - 在类中，你可以轻松定义静态方法和属性，它们不会出现在实例中，而是直接在类上。
   - 使用函数构造函数时，静态方法和属性需要手动添加。

5. **继承**:

   ```
   // Class 继承
   class ParentClass {
     constructor(name) {
       this.name = name;
     }
   }
   
   class ChildClass extends ParentClass {
     constructor(name, age) {
       super(name); // 调用父类构造函数
       this.age = age;
     }
   }
   
   // Function 继承
   function ParentFunction(name) {
     this.name = name;
   }
   
   function ChildFunction(name, age) {
     ParentFunction.call(this, name); // 调用父构造函数
     this.age = age;
   }
   
   ChildFunction.prototype = Object.create(ParentFunction.prototype);
   ChildFunction.prototype.constructor = ChildFunction;
   ```

6. - `class` 提供了更加简洁的继承语法，通过 `extends` 关键字可以轻松地实现类继承。
   - 使用 `function` 构造函数时，需要使用 `Object.create` 或直接操作原型链来实现继承。

7. **类的性质**:

   ```
   // Class 示例
   const instance = new MyClass(); // 必须使用 new
   
   // Function 构造函数示例
   const instance2 = MyFunction(); // 不使用 new 可能会出问题
   ```

8. - `class` 声明在执行前不会提升，而 `function` 声明会被提升到作用域顶部，但 `class` 声明的内容是暂时性死区内的，必须在声明后才能使用。
   - 类的构造器在实例化时必须使用 `new`，否则会抛出错误，而函数构造函数在没有使用 `new` 的情况下调用时不会抛出错误，但可能会导致 `this` 指向错误（比如指向全局对象或 `undefined`）。

### 总结

- **类 (class)** 提供了一种更加结构化、清晰的面向对象编程方式，尤其是在涉及继承、静态方法等方面。而且它的语法更容易理解和使用，特别是对那些来自面向对象编程语言（如 Java、C++）的开发者来说。
- **函数构造函数 (function)** 是一种较老的、原生的 JavaScript 对象创建方式，虽然功能上几乎可以实现类的所有功能，但语法上不如类直观和简洁。