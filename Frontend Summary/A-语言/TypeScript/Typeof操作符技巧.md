## Typeof操作符技巧

### **获取对象的类型**

![1713004686569](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004686569.png)

 `man` 对象是一个普通的JavaScript对象，在TypeScript中你可以使用type或interface来定义对象的类型。有了这个对象类型，你就可以使用TypeScript内置的工具类型，比如Partial、Required、Pick或Readonly来处理对象类型，以满足不同的需求。

对于简单的对象，这可能不是什么大问题。但是对于具有更深嵌套层次的大型复杂对象，手动定义它们的类型可能会让人感到头脑麻木。要解决这个问题，可以使用typeof操作符。

```
type Person = typeof man;
```

```
type Address = Person["address"];
```

与之前手动定义类型相比，使用typeof操作符要容易得多。 `Person["address"]` 是一个索引访问类型，用于查找另一个类型(Person类型)上的特定属性(address)。

### **获取将所有枚举键表示为字符串的类型**

在TypeScript中，枚举类型是被编译成常规JavaScript对象的特殊类型:

![1713004703284](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004703284.png)

因此，也可以对枚举类型使用 `typeof` 操作符。但这通常没有太多实际用途，当处理枚举类型时，它通常与 `keyof` 操作符结合使用:

![1713004730136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004730136.png)

### **获取函数对象的类型**

还有另一种更常见的场景，在工作中使用typeof操作符。在获得相应的函数类型之后，你可以继续使用TypeScript内置的ReturnType和Parameters实用工具类型来分别获得函数的返回值类型和参数类型。

### ![1713004765703](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004765703.png)**获取类对象的类型**

既然 `typeof` 操作符可以处理函数对象，那么它是不是也可以处理类对象呢。答案是肯定的。

![1713004785456](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004785456.png)

在上面的代码中， `createPoint` 是一个工厂函数，它创建Point类的一个实例。通过typeof运算符，可以获得Point类相应的构造签名，从而实现相应的类型验证。在定义Constructor的形参类型时，如果未使用typeof操作符，将出现以下错误消息:

![1713004798085](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004798085.png)

### **获得更精确的类型**

当使用 `typeof` 操作符时，如果你想获得更精确的类型，那么你可以将它与TypeScript 3.4版中引入的const断言结合使用。它的用法如下。

![1713004816500](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713004816500.png)

从上图可以看出，在使用const断言后，再使用typeof操作符，我们可以获得更精确的类型。