**[TypeScript 教程](https://wangdoc.com/typescript/)**
### 一、基本用法
#### 1. 类型声明
```javascript
let x:number;
console.log(x) // 报错
```
变量x没有赋值就被读取，导致报错。
#### 2. 类型推断

> **类型声明并不是必需的，如果没有，TypeScript 会自己推断类型。**
```javascript
let foo = 123;
foo = 'hello'; // 报错
```
变量foo的类型推断为number，后面赋值为字符串，TypeScript 就报错了。
#### 3. TypeScript 的编译
**JavaScript 的运行环境（浏览器和 Node.js）不认识 TypeScript 代码。** 所以，TypeScript 项目要想运行，必须先转为 JavaScript 代码，这个代码转换的过程就叫做“编译”（compile）。

TypeScript 官方没有做运行环境，只提供编译器。编译时，会将类型声明和类型相关的代码全部删除，只留下能运行的 JavaScript 代码，并且不会改变 JavaScript 的运行结果。

因此，TypeScript 的类型检查只是编译时的类型检查，而不是运行时的类型检查。一旦代码编译为 JavaScript，运行时就不再检查类型了。
#### 4. 值与类型
**“类型”是针对“值”的，可以视为是后者的一个元属性。每一个值在 TypeScript 里面都是有类型的。比如，3是一个值，它的类型是number。**

> 它们是可以分离的，TypeScript 的编译过程，实际上就是把“类型代码”全部拿掉，只保留“值代码”。
#### 5. TypeScript Playground
> 最简单的 TypeScript 使用方法，就是使用官网的在线编译页面，叫做 [TypeScript Playground](https://www.typescriptlang.org/play)。

#### 6. tsc 编译器

> TypeScript 官方提供的编译器叫做 tsc，可以将 TypeScript 脚本编译成 JavaScript 脚本。本机想要编译 TypeScript 代码，必须安装 tsc。
> 根据约定，TypeScript 脚本文件使用.ts后缀名，JavaScript 脚本文件使用.js后缀名。tsc 的作用就是把.ts脚本转变成.js脚本。
##### 6.1 安装
```javascript
npm install -g typescript

// 安装完成后，检查一下是否安装成功
tsc -v  // 或者 tsc --version
Version 5.1.6
```
##### 6.2 帮助信息
```javascript
// -h或--help参数输出帮助信息。
tsc -h
// 默认情况下，“--help”参数仅显示基本的可用选项。我们可以使用“--all”参数，查看完整的帮助信息。
tsc --all
```
##### 6.3 编译脚本
```javascript
// tsc命令后面，加上 TypeScript 脚本文件，就可以将其编译成 JavaScript 脚本。
tsc app.ts  // 在当前目录下，生成一个app.js脚本文件，这个脚本就完全是编译后生成的 JavaScript 代码。

// tsc命令也可以一次编译多个 TypeScript 脚本。
tsc file1.ts file2.ts file3.ts  // 在当前目录生成三个 JavaScript 脚本文件file1.js、file2.js、file3.js。

```
###### 1）--outFile
```javascript
// 如果想将多个 TypeScript 脚本编译成一个 JavaScript 文件，使用--outFile参数。
tsc file1.ts file2.ts --outFile app.js // 上面命令将file1.ts和file2.ts两个脚本编译成一个 JavaScript 文件app.js。
```
###### 2）--outDir
```javascript
// 编译结果默认都保存在当前目录，--outDir参数可以指定保存到其他目录。
tsc app.ts --outDir dist  // 会在dist子目录下生成app.js。
```
###### 3）--target
为了保证编译结果能在各种 JavaScript 引擎运行，**tsc 默认会将 TypeScript 代码编译成很低版本的 JavaScript，即3.0版本（以es3表示）**。这通常不是我们想要的结果。
```javascript
// 这时可以使用--target参数，指定编译后的 JavaScript 版本。建议使用es2015，或者更新版本。
tsc --target es2015 app.ts
```
##### 6.4 编译错误的处理
> **如果编译报错，tsc命令就会显示报错信息，但是这种情况下，依然会编译生成 JavaScript 脚本。**

**如果希望一旦报错就停止编译，不生成编译产物，可以使用--noEmitOnError参数。**
```javascript
tsc --noEmitOnError app.ts // 在报错后，就不会生成app.js
```
**tsc 还有一个--noEmit参数，只检查类型是否正确，不生成 JavaScript 文件。**
```javascript
tsc --noEmit app.ts // 上面命令只检查是否有编译错误，不会生成app.js。
```

> tsc 命令的更多参数，详见 **《tsc 编译器》** 一章。
##### 6.5 tsconfig.json
> **TypeScript 允许将tsc的编译参数，写在配置文件tsconfig.json**。只要当前目录有这个文件，tsc就会自动读取，所以运行时可以不写参数。
```javascript
tsc file1.ts file2.ts --outFile dist/app.js
```
**上面这个命令写成tsconfig.json，就是下面这样。**
```javascript
{
  "files": ["file1.ts", "file2.ts"],
  "compilerOptions": {
    "outFile": "dist/app.js"
  }
}
```
**有了这个配置文件，编译时直接调用tsc命令就可以了。**
```javascript
tsc
```
> tsconfig.json的详细介绍，参见 **《tsconfig.json 配置文件》** 一章。

##### 6.6 ts-node
>  [ts-node](https://github.com/TypeStrong/ts-node) 是一个非官方的 npm 模块，可以直接运行 TypeScript 代码。

使用时，可以先全局安装它。
```javascript
npm install -g ts-node
```
安装后，就可以直接运行 TypeScript 脚本。
```javascript
ts-node script.ts
```
> 上面命令运行了 TypeScript 脚本script.ts，给出运行结果。

**如果不安装 ts-node，也可以通过 npx 调用它来运行 TypeScript 脚本。**
```javascript
npx ts-node script.ts
```
上面命令中，npx会在线调用 ts-node，从而在不安装的情况下，运行script.ts。

**如果执行 ts-node 命令不带有任何参数，它会提供一个 TypeScript 的命令行 REPL 运行环境，你可以在这个环境中输入 TypeScript 代码，逐行执行。**
```javascript
ts-node
>
```
> 上面示例中，单独运行ts-node命令，会给出一个大于号，这就是 TypeScript 的 REPL 运行环境，可以逐行输入代码运行。

```javascript
$ ts-node

> const twice = (x:string) => x + x;
> twice('abc')
'abcabc'
> 
```
> 上面示例中，在 TypeScript 命令行 REPL 环境中，先输入一个函数twice，然后调用该函数，就会得到结果。

**要退出这个 REPL 环境，可以按下 Ctrl + d，或者输入.exit。**

> 如果只是想简单运行 TypeScript 代码看看结果，ts-node 不失为一个便捷的方法。