## keyof

> keyof 操作符，是将一个类型 映射为它所有成员名称的联合类型。

```ts
interface Person {
    name: string
    age: number
    gender: string
}
type p = keyof Person // "name"|"age"|"gender"
// keyof 将Person映射成一个联合类型
```

## Partial--属性变为可选属性

> Partial 的作用是将传入得属性变成可选性，原理就是使用keyof拿到所有的属性名，然后再使用in【遍历】，T[P]拿到相应的值。

```ts
type Partial<T> = { 
    [P in keyof T] ?: T[P]
}
// 作用：生成一个新类型，该类型与T拥有相同的属性，但所有属性皆为可选项

// eg:
interface Person {
    name:string
    age:number
}
type People = Partial<Person>
// 相当于
type People = {
    name?:string
    age?:number
}
```

## Required--可选属性变成必选

> Required 的作用是将传入得属性变为必选项，原理是使用 **-?** 将可选性的 **?** 去掉。与之对应的还有个 **+?**。

```ts
type Required<T> = {
    [P in keyof T]-?: T[P]
}

// eg:
interface Person {
    name:string
    age?:number
}
type People = Required<Person>
// 相当于
type People = {
    name:string
    age:number
}
```

## Readonly--都成为只读属性

> Readonly 作用是将传入的属性变为只读选项。

```ts
type Readonly<T> = {
    readonly [P in keyof T]: T[P]
}

// eg:
interface Person {
    name:string
    age:number
}
type People = Required<Person>
// 相当于
type People = {
    readonly name:string
    readonly age:number
}
```

## Mutable--去掉readonly

> Mutable 作用是将传入属性的readonly移除。

```ts
type Mutable<T> = {
    -readonly [P in keyof T]: T[P]
}

// eg:
interface Person {
    readonly name:string
    readonly age:number
    readonly height:number
}
type People = Mutable<Person>

function newPerson (key:string) : Person {
    const person: Mutable<Person> = {name:'z',age:18}
    if(shouldHaveHeight(key)) {
        person.x = 180
    }
    retrun person
}
```

## Record--将类型B赋值给类型A的每一项

> Record作用是将K中所有属性的值转化成T类型。
>
> Record<K, T>构造具有给定类型T的一组属性K的类型。在将一个类型的属性映射到另一个类型的属性时，Recors非常方便。它会将一个类型的所有属性值都映射到另一个类型上并创造一个新的类型。

```ts
type Record<K extends keyof any, T> = {
    [P in K]: T
}

// eg:
type petsGroup = 'dog'|'cat'|'fish'
interface IPetInfo {
    name: string
    age: number
}
type IPets = Record<petsGroup, IPetInfo>

const animalsInfo:IPets = {
    dog: {
        name:'dogname',
        age:2
    },
    cat: {
        name:'catname',
        age:3
    },
    fish: {
        name:'fishname',
        age:1
    }
}
```

## Pick--将类型A中的一些属性拿出来组成新的类型

> Pick 的作用是从T中取出一系列K的属性

```ts
type Pick<T, K extends keyof T> = {
    [P in K]: T[P]
}

// eg:
interface Person {
    name: string
    age: number
    id: number
    sex: 0 | 1
}

type Woman = Pick<Person, "name"|"id">
// 相当于
interface Woman {
    name:string
    id:number
}
```

## Exclude--将类型A中的属性且类型B中没有的属性拿出来

> Exclude 的作用是从T中找出U中没有的元素。

```ts
type Exclude<T, U> = T extends U ? never : T

type A = Exclude<'key1'|'key2', 'key2'>
// 'key1'
// 这个定义就利用了条件类型中的分配原则，来尝试将这个实例拆开看看发生了什么：
type A = `Exclude<'key1'|'key2',' key2'>`

// 等价与
type A = `Exclude<'key1',' key2'>`|`Exclude<'key2',' key2'>`

// =>
type A = ('key1' extends 'key2' ? never : 'key1')|('key2' extends 'key2' ? never : 'key2')

// =>
// never 是所有类型的子类型
type A = 'key1'|never = 'key1'
```

## Extract--将类型A和B中共有的属性拿出来

> 高级函数 Extract 和上面的Exclude 刚好相反，它是将第二个参数的联合项从第一个参数的联合项中提取出来，当然，第二个参数可以含有第一个参数没有的项。

```ts
type Extract<T, U> = T extends U ? T : never

type A = Exclude<'key1'|'key2', 'key1'>
// 'key1'
```

## Omit--去掉类型A的一些属性

> Omit 的作用是忽略对象的某些属性功能。它的作用主要是：以一个类型为基础支持 剔除某些属性，然后返回一个新类型。

```ts
type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof  T, K>>

// eg:
type Person = {
    name: string
    age: number
    location: string
}

type PersonWithoutLocation = Omit<Person, 'location'>

// PersonWithoutLocation equal to QuantumPerson
type QuantumPerson {
	name:string
	age: number
}
```

## OmitThisParameter--从类型A中剔除this参数类型

```ts
function f0(this:object, x:number) {}

type T = OmitThisParameter<typeof f0>
// (x: number) => void
```



## 关键字NonNullable--去掉联合类型A中的null和undefined

```ts
type res = NonNullable<string|number|boolean|null|undefined>
// res string|number|boolean
```

## 关键字ReturnType--获取函数返回值类型

```ts
type res = ReturnType<()=>string>
```

## 关键字ConstructorParameters--获取一个类的构造函数组成的元组类型

```ts
class Person {
	constructor(name:string,age:number) {}
}

type res = ConstructorParameters<typeof Person>

// res [name:string,age:number]
```

## 关键字Parameters--获取函数的参数类型组成的元组类型

```ts
function show(name:string,age:number) {}

type res = Parameters<typeof show>
                      
// res [name:string,age:number]
```

