# 从 never 切入，摸透 TypeScript 的学习思路

![1724510309923](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510309923.png)

我本来只是想跟大家分享一些 never 这个知识点：一个虽然用得很少，但是报错信息里经常会出现的类型。

但是在群里讨论的时候，隐约发现不少道友对于 TS 的类型系统并没有一个比较系统的认知，所以经常在面对一些情况不知所措。这篇文章就从 never 类型切入，带大家把类型系统简单总结一下。

------

### **类型系统**

类型其实表达的是一种**集合**。集合是数学上的概念，通俗来说，表达的就是一个范围。我们在学习之前，一定要用集合的概念去理解所有的类型。也就是说，实际上我们学 TS，就是一个探讨**集合范围大小**的问题。

例如：**any 表示最大集合。** 他可以是任意类型，你怎么用他都不会出错。如下所示

![1724510322571](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510322571.png)

any 类型由于范围太广，所以约束力度几乎没有，它可以让整个 TS 形同虚设。

当我们知道类型是一种集合之后，那么，我们就可以很自然的衍生出一些非常常见的概念，例如：**全集、子集、空集、交集、并集、补集**等。

全集 any，表示范围最大的集合。

空集 never，表示范围最小的集合。**此时集合中没有任何值。**

子集表示范围大小的包含关系。这个概念是学好 TS 类型的关键中的关键。在我的付费专栏《JavaScript》中有专门提高，**子集的理解与类型兼容性有非常大的关系。** 在赋值关系中，**我们只能把子集赋值给范围更大的父集**

```
type A = number
type B = number | string
```

例如这个案例中，B 为范围更广的集合，而 A 则为 B 的子集。因此，这里我们可以使用子集去赋值给父集。

> ✓
>
> 或者说，子类型，赋值给父类型。这里和设计模式中的**里氏替换原则**是高度相似的概念：任何使用父类型的地方，都可以用子类型进行替换

因此，下面这段代码的报错，我们就可以非常轻松的理解了

```
let b: number | string = 20
let c: string = b
```

![1724510372865](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510372865.png)

当然，除此之外，对象、泛型、函数的父子集合关系，我们还要额外学习，本文主要提供学习方法，大家可以购买《JavaScript 核心进阶》进一步理解。这里主要理解起来最困难的，是函数和泛型中的逆变与协变。

联合类型表示并集。我们可以使用并集扩大集合范围。

```
let b: number | string = 20
```

交叉类型表示交集。我们可以使用交集缩小集合范围。

```
let b: number & string
```

当交叉类型为空时，在 ts 中，就使用 never 来表示

![1724510389529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510389529.png)

在 TS 中，其他的集合操作常常会被作为面试题来考察。例如我们可以使用 `Exclude` 来定义一个获取补集的类型

```
type Exclude<T, U> = T extends U ? never : T
```

有了这个之后，我们就可以有如下运用

```
type a = 'name' | 'age' | 'gender' | 'class'
type b = Exclude<a, 'name'>
```

![1724510409133](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510409133.png)

我们可以分析一下具体的实现原理。

在 extends 的语法规则中，当传入的泛型为联合类型时，会先分配再传入

因此，此时传入的联合类型 `a` 会被拆分传入。

也就是说，`T exnteds U` 的比较会变成

```
// never
'name' extends 'name' ? never : 'name'
// age
'age' extends 'name' ? never : 'age'
// gender
'gender' extends 'name' ? never : 'gender'
// class
'class' extends 'name' ? never : 'class'
```

所以通过这种方式，我们可以做到从联合类型中排除指定的类型，从而实现补集。

除此之外，实现 Pick 挑选，Omit 取反等都是常见的面试考点。完整的学习可以参考这篇文章：[TypeScript 中的 extends 怎么这么骚呀](http://mp.weixin.qq.com/s?__biz=MzI4NjE3MzQzNg==&mid=2649867845&idx=1&sn=396992841fd2e7682e17d8686db4b4ab&chksm=f3e597d6c4921ec056a203833f56feed7236d39a75f324a590b4a8ecf9aac2fac56eed899c8e&scene=21#wechat_redirect)

------

### **理解 never 的存在**

当我们有了集合的概念，对于理解下面这些情况的报错，就会变得非常自然。

第一个案例

```
const c: number & string = 20
```

![1724510432311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510432311.png)

第二个案例

```
type A = {
  name: string,
  age: number
}

type B = {
  name: number,
  age: number
}

type C = A & B // {name: never, age: number}  

let person: C = {
  name: 'jake',
  age: 20
}
```

![1724510455290](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510455290.png)

第三个案例

```
interface O {
  a: number,
  b: string
}

function foo(obj: O, key: keyof O) {
  obj[key] = 100
}
```

![1724510474371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510474371.png)

第三个案例需要稍微解释一下。我们可以很容易看出，`obj[key]` 要么为 number，要么为 string。但是 TS 为了确保类型安全，传入的类型既要满足 number，又要满足 string，因此，他实际上是取的一个交集

```
number & string
```

推导的结果自然就是 never。

------

### **验证 never 类型**

在之前讲解 [extends](http://mp.weixin.qq.com/s?__biz=MzI4NjE3MzQzNg==&mid=2649867845&idx=1&sn=396992841fd2e7682e17d8686db4b4ab&chksm=f3e597d6c4921ec056a203833f56feed7236d39a75f324a590b4a8ecf9aac2fac56eed899c8e&scene=21#wechat_redirect) 的文章中，我详细分析了如何验证一个类型是否为 never 类型

在 TS 中，never 被看成是一个空联合类型，结合在 extends 中学到的知识，我们可以这样做

```
type IsNever<T> = [T] extends [never] ? true : false;

type R1 = IsNever<never> // 'true' 
type R2 = IsNever<number> // 'false' 
```

------

### **使用 never**

我们学习理解 never 的目的，主要是用来理解某些情况下出现的报错。但是偶尔我们也可以使用 never 来解决一些逻辑缺失

例如，我们在定义 Exclude 时，就借助了 never 类型来完成判断

```
type Exclude<T, U> = T extends U ? never : T
```

我们也可以使用同样的逻辑来完成对象中属性的过滤

```
type Filter<O extends Object, ValueType> = {
  [Key in keyof O as ValueType extends O[Key] ? Key : never]: O[Key]
}


interface P {
  a: string,
  b: number,
  c: boolean,
  d: boolean,
  e: number
}

type T1 = Filter<P, number>
```

![1724510515887](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510515887.png)

成功过滤出全是 number 的属性

```
type T2 = Filter<P, boolean>
```

![1724510529022](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724510529022.png)

成功过滤出全是 boolean 的属性

> !
>
> > 大多数情况下，never 都运用于在类型体操中，结合 extends 条件判断一起使用。

------

### **总结**

TS 类型系统是一个集合系统。类型系统的运用，就是集合的运用。any 表示范围最大的集合，never 表示范围最小的集合，空集合。因此，我们可以结合以前在数学上学到的集合知识，来快速掌握 TS 的学习。

never 作为空集合，大多数情况下出现在报错里。我们需要集合对 never 的学习来理解这些报错出现的原因。除此之外，我们也经常将 never 与条件判断结合起来使用，其运用方式经常会作为面试考点。