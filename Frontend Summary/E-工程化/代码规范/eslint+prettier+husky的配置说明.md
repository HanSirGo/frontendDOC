### eslint和prettier的区别

> eslint官网描述：目标是提供一个插件化的javascript代码检测工具
> 只能作用于js代码及文件，html、css、json、vue代码及文件的相关格式处理不了；

> prettier官网描述：
> 1、一个“有态度”的代码格式化工具
2、支持大量编程语言
全部代码都可以自定义格式化。

```js
// 1、eslint针对的是javascript，他是一个检测工具，包含js语法以及少部分格式问题，在eslint看来，语法对了就能保证代码正常允许，格式问题属于其次；

// 2、prettier属于格式化工具，它看不惯格式不统一，所以它就把eslint没干好的事接着干，另外，prettier支持包含js在内的**多种语言

// 总结：eslint和prettier这俩兄弟一个保证js代码质量，一个保证代码美观。
```



### eslint和prettier冲突原因及机制
#### 1、原因

```
由两者的区别可以看到，ESLint和Prettier都可以作用于js代码及文件，由于两个插件默认配置存在差异，所以在公共域必然也就存在冲突了。

1、ESLint默认语句结尾不加分号，Prettier默认语句结尾加分号；
2、ESLint默认强制使用单引号，Prettier默认使用双引号；
3、ESLint默认句末减少不必要的逗号，Prettier默认尽可能多使用逗号；
4、......
```

#### 2、机制
```
保存之后，**先执行了ESLint的配置，然后再执行Prettier的配置**。不管前面的ESLint怎样设置的规则，只要和Prettier的设置冲突了，就直接会被**Prettier覆盖**，所以最后呈现的代码效果都是Prettier格式化的结果。
```

#### 3、解决方案
> 根据 vuejs 官方的 ESLint 配置 https://eslint.vuejs.org/user-guide/#usage
> ，其中有讲到如何解决 ESLint 和 Prettier 配置冲突的问题

![1709791812013](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709791812013.png)
##### 安装 eslint-config-prettier 依赖
```javascript
npm i -D eslint-config-prettier
```
##### 配置 .eslintrc.js
```javascript
  extends: [
    // 参考vuejs官方的eslint配置：https://eslint.vuejs.org/user-guide/#usage
    "plugin:vue/vue3-recommended",
    // 覆盖 ESLint 配置，确保 prettier 放在最后
    "prettier",
  ]
```
> 相关链接：
>  - prettier官方文档：https://prettier.io/docs/en/integrating-with-linters.html#docsNav
>   - eslint-config-prettier：https://github.com/prettier/eslint-config-prettier
>    - eslint-plugin-prettier：https://github.com/prettier/eslint-plugin-prettier
>    - typescript-eslint：https://github.com/typescript-eslint/typescript-eslint/blob/master/docs/getting-started/linting/README.md#usage-with-prettier
### 一，eslint
#### 1. 安装selint

```javascript
npm install eslint --save-dev
```
##### 下载 ESLint 相关依赖：

```javascript
npm install eslint eslint-config-prettier eslint-plugin-prettier eslint-plugin-vue @typescript-eslint/eslint-plugin @typescript-eslint/parser -D
```
![1709791898402](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709791898402.png)

#### 2. 生成.eslintrc.js文件
```javascript
npx eslint --init
```
生成.eslintrc.js文件：

```javascript
module.exports = {
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": [
        "eslint:recommended",//继承Eslint中推荐的（打钩的）规则项http://eslint.cn/docs/rules/
        "plugin:vue/essential"// 此项是用来配置vue.js风格
    ],
    "parserOptions": {
        "ecmaVersion": 13,
        "sourceType": "module"
    },
    "plugins": [
        "vue"
    ],
    "rules": {
	    /*
		 * "off" 或 0    ==>  关闭规则
		 * "warn" 或 1   ==>  打开的规则作为警告（不影响代码执行）
		 * "error" 或 2  ==>  规则作为一个错误（代码不能执行，界面报错）
		 */
		 // eslint (http://eslint.cn/docs/rules)
		"no-var": "error", // 要求使用 let 或 const 而不是 var
		"no-multiple-empty-lines": ["error", { max: 1 }], // 不允许多个空行
		"no-use-before-define": "off", // 禁止在 函数/类/变量 定义之前使用它们
		"prefer-const": "off", // 此规则旨在标记使用 let 关键字声明但在初始分配后从未重新分配的变量，要求使用 const
		"no-irregular-whitespace": "off", // 禁止不规则的空白

		// typeScript (https://typescript-eslint.io/rules)
		"@typescript-eslint/no-unused-vars": "error", // 禁止定义未使用的变量
		"@typescript-eslint/prefer-ts-expect-error": "error", // 禁止使用 @ts-ignore
		"@typescript-eslint/no-inferrable-types": "off", // 可以轻松推断的显式类型可能会增加不必要的冗长
		"@typescript-eslint/no-namespace": "off", // 禁止使用自定义 TypeScript 模块和命名空间。
		"@typescript-eslint/no-explicit-any": "off", // 禁止使用 any 类型
		"@typescript-eslint/ban-types": "off", // 禁止使用特定类型
		"@typescript-eslint/explicit-function-return-type": "off", // 不允许对初始化为数字、字符串或布尔值的变量或参数进行显式类型声明
		"@typescript-eslint/no-var-requires": "off", // 不允许在 import 语句中使用 require 语句
		"@typescript-eslint/no-empty-function": "off", // 禁止空函数
		"@typescript-eslint/no-use-before-define": "off", // 禁止在变量定义之前使用它们
		"@typescript-eslint/ban-ts-comment": "off", // 禁止 @ts-<directive> 使用注释或要求在指令后进行描述
		"@typescript-eslint/no-non-null-assertion": "off", // 不允许使用后缀运算符的非空断言(!)
		"@typescript-eslint/explicit-module-boundary-types": "off", // 要求导出函数和类的公共类方法的显式返回和参数类型

		// vue (https://eslint.vuejs.org/rules)
		"vue/no-v-html": "off", // 禁止使用 v-html
		"vue/script-setup-uses-vars": "error", // 防止<script setup>使用的变量<template>被标记为未使用，此规则仅在启用该no-unused-vars规则时有效。
		"vue/v-slot-style": "error", // 强制执行 v-slot 指令样式
		"vue/no-mutating-props": "off", // 不允许组件 prop的改变
		"vue/custom-event-name-casing": "off", // 为自定义事件名称强制使用特定大小写
		"vue/attributes-order": "off", // vue api使用顺序，强制执行属性顺序
		"vue/one-component-per-file": "off", // 强制每个组件都应该在自己的文件中
		"vue/html-closing-bracket-newline": "off", // 在标签的右括号之前要求或禁止换行
		"vue/max-attributes-per-line": "off", // 强制每行的最大属性数
		"vue/multiline-html-element-content-newline": "off", // 在多行元素的内容之前和之后需要换行符
		"vue/singleline-html-element-content-newline": "off", // 在单行元素的内容之前和之后需要换行符
		"vue/attribute-hyphenation": "off", // 对模板中的自定义组件强制执行属性命名样式
		"vue/require-default-prop": "off", // 此规则要求为每个 prop 为必填时，必须提供默认值
		"vue/multi-word-component-names": "off", // 要求组件名称始终为 “-” 链接的单词
    }
};

```
#### 3. 查看这个推荐的默认规则
> 这里的规则，写在后面的会覆盖前面的，并且rules中的规则最后会覆盖这里的，所以我们自定义规则就是写在rules中，才能覆盖之前的规则，从而生效。

```javascript
"extends": [
    "eslint:recommended",//继承Eslint中推荐的（打钩的）规则项http://eslint.cn/docs/rules/
    "plugin:vue/essential"// 此项是用来配置vue.js风格https://eslint.vuejs.org/rules
],
```
[quotes - Rules - ESLint中文](http://eslint.cn/docs/rules/quotes)

[Available rules | eslint-plugin-vue (vuejs.org)](https://eslint.vuejs.org/rules/)
这个默认规则是怎么生效的呢？举个例子，我们点进去链接，可以看到这个no-debugger是打上了对勾（也就是"eslint:recommended"）中生效的规则，然后我们再在项目中写个debugger，然后命令行运行检查eslint的命令，就会报错了。

项目根目录执行：

```javascript
npx eslint --ext .vue src/
等价于：./node_modules/.bin/eslint --ext .vue src/
--ext:可以指定在指定目录/文件
.vue：vue文件
src/：在src目录下匹配

```
![1709791963755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709791963755.png)
#### 4，自定义规则
Eslint附带了大量的校验规则，这些规则的值分别有如下规律：

off | 0 :表示关闭规则。
warn | 1 :表示将该规则转换为警告。
error | 2 :表示将该规则转换为错误。
常见的rules规则，可以看官网：[List of available rules - ESLint中文](http://eslint.cn/docs/rules/)

```javascript
// "semi": [2, "always"],//语句强制分号结尾
// "quotes": [2, "double"],//引号类型 ""
//"no-alert": 0,//禁止使用alert
//"no-console": 2,//禁止使用console
//"no-const-assign": 2,//禁止修改const声明的变量
//"no-debugger": 2,//禁止使用debugger
//"no-duplicate-case": 2,//switch中的case标签不能重复
//"no-extra-semi": 2,//禁止多余的冒号
//"no-multi-spaces": 1,//不能用多余的空格

```
当我们这样自定义配置结尾必须分号之后，再运行检查，项目中就会报对应的错：

![1709792010374](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792010374.png)

#### 5，package.json中配置检查命令行
上文中，我们是手动输入命令行来检查代码是否符合eslint规范的，这样每次检查都要输入一遍，有的人要是记不住这命令怎么办？于是可以在package.json的script中写死这个脚本：
```javascript
"lint": "eslint --ext .js --ext .jsx --ext .vue src/",
```

于是，项目根目录运行npm run lint，实际上就是运行：
```javascript
npx eslint --ext .js --ext .jsx --ext .vue src/
```
从而实现代码的检查：
![1709792043479](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792043479.png)

#### 6，eslint插件自动检查规范
难道每次写完一句代码，都要npm run lint来检查下是否符合规范嘛？那样太麻烦了。于是有eslint插件，这里我用的是vscode，打开插件市场，搜索到eslint，安装即可：
![1709792084666](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792084666.png)
安装了之后什么效果呢？就是它会自动检查你的代码是否符号规范，并且会在编辑器中标识出来哪里不符合规范，底下终端处还会罗列出问题：
![1709792124564](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792124564.png)

#### 7，package.json中配置自动修复命令行
比如说上文咱们写了好多这种bug，一个一个手动修复，太麻烦了。eslint提供了一个–fix的命令行，可以实现自动修复不符合规范的代码，但是这种修复不是万能的，官网中说，有这个（扳手）🔧的图标对应的规则才是可以修复的。
![1709792184211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792184211.png)
可以看到，debugger这个规则应该是不能修复的，而分号这个是可以自动修复的。

具体的命令行，同样设置到package.json中：
```javascript
"lint-fix": "eslint --fix --ext .js --ext .jsx --ext .vue src/"
```
在根目录执行这个npm run lint-fix
![1709792204262](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792204262.png)
可以看到分号的错误被修复了，而debugger无法自动修复，只能手动修复。

#### 8，在vscode中配置setting,让每次保存代码时自动修复
先不考虑debugger这种只能手动修复的，在实际写代码中，更多时候遇到的是分号啦，单双引号啦这些可以被自动修复的错误，那么，我们还需要每次去手动敲npm run lint-fix嘛？那同样效率很低。现在利用vscode的eslint插件我们已经能一写完代码，就知道是否符合规范了。然而我们还想实现的是每次写完代码，按下ctrl+s保存时，它能够自动修复。

幸运的是，vscode的setting.json能够帮助我们实现这一点：
> 在vscode中按快捷键ctrl+shift+p，输入setting,打开setting.json
![1709792231328](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792231328.png)
增加这段配置即可（如果未生效重启vscode）：
```javascript
 //配置保存时按照eslint文件的规则来处理一下代码
"editor.codeActionsOnSave": {
    "source.fixAll": true,
    "eslint.autoFixOnSave" : true,
  },
```
一按保存，就自动修复了分号的问题。
#### 9，配置eslintignore
有些文件，我们不希望它受eslint管辖，可以在根目录新建.eslintignore文件，把想要自由的文件或目录丢进去即可：

```javascript
.eslintrc.js
.prettierrc.js
babel.config.js
vetur.config.js
/node_modules/

```
![1709792249989](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792249989.png)
#### 10，总结
反思几个问题：
##### 我们为什么需要eslint?

> 	因为每个人的代码习惯不一样，每个开发人员都需要和他人协作或者项目需要交接给下一代开发者。保持统一的代码规范，可以极大地提高效率，降低沟通和理解代码的成本。
##### 既然已经有vscode的eslint插件帮助我们自动检查和修复代码了，为什么还需要在pakage.json中配置eslint命令行?
> 	因为vscode是我们自己电脑上的编辑器，我们不能保证另一个人拿到我们的代码时他也是用vscode的（也许他用记事本？）。就算他用vscode,我们也不能保证他安装了eslint插件。
	 ，他就可以使用pakage.json的命令行来检查和修复代码。因为项目中肯定需要安装eslint依赖，也肯定有我们配置的.eslintrc.js规则文件，这样依旧能保证代码的统一规范。

### 二、prettie
#### 1，prettier的作用
上文我们配置了eslint,它规范的是代码偏向语法层面上的风格。比如说不能使用console啦，箭头函数参数必须包裹在括号中啦这些。

而项目中有时候，我们写代码删删改改，或者是个人的代码习惯，就有可能出现这样的代码：

```javascript
<script>
	import HelloWorld from './components/HelloWorld.vue';
        export default {
                name: 'App',
        components: {
    HelloWorld
  }
};
</script>

```
排版很乱，或者写html时，太长了换不换行之类的代码排版问题，就需要prettier来规范。

也就是说，prettier规范的是代码偏向于排版层面上的风格。
#### 2，安装prettier

```javascript
npm install --save-dev --save-exact prettier

```
#### 3，使用命令行修复格式化
在 package.json 中配置一条脚本命令，运行 npm run prettier 就会格式化所有文件。（注意是否要用）
```javascript
{
	"scripts": {
		"prettier": "prettier --write ."
	}
}
```
格式化src下文件（要不要用根据实际情况）
```javascript
npx prettier --write src/
```
![1709792294045](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792294045.png)
可以看到，我原先故意写乱的代码，变得整齐了。

#### 4，配置自定义的prettier规则
需要在根目录新建一个文件.prettierrc.js（则需要module.export）,如果是.prettierrc(则只需要json格式)：

```javascript
//此处的规则供参考，其中多半其实都是默认值，可以根据个人习惯改写
module.exports = {
    printWidth: 80, //单行长度
    tabWidth: 2, //缩进长度
    useTabs: false, //使用空格代替tab缩进
    semi: false, //句末使用分号
    singleQuote: false, //使用单引号
    arrowParens: "avoid",//单参数箭头函数参数周围使用圆括号-eg: (x) => xavoid：省略括号
    bracketSpacing: true,//在对象前后添加空格-eg: { foo: bar }
    insertPragma: false,//在已被preitter格式化的文件顶部加上标注
    jsxBracketSameLine: false,//多属性html标签的‘>’折行放置
    rangeStart: 0,
    requirePragma: false,//无需顶部注释即可格式化
    trailingComma: "none",//多行时尽可能打印尾随逗号
    useTabs: false//使用空格代替tab缩进
  };

```

```javascript
// @see: https://www.prettier.cn

module.exports = {
	// 超过最大值换行
	printWidth: 130,
	// 缩进字节数
	tabWidth: 2,
	// 使用制表符而不是空格缩进行
	useTabs: true,
	// 结尾不用分号(true有，false没有)
	semi: false,
	// 使用单引号(true单引号，false双引号)
	singleQuote: true,
	// 更改引用对象属性的时间 可选值"<as-needed|consistent|preserve>"
	quoteProps: "as-needed",
	// 在对象，数组括号与文字之间加空格 "{ foo: bar }"
	bracketSpacing: true,
	// 多行时尽可能打印尾随逗号。（例如，单行数组永远不会出现逗号结尾。） 可选值"<none|es5|all>"，默认none
	trailingComma: "none",
	// 在JSX中使用单引号而不是双引号
	jsxSingleQuote: true,
	//  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号 ,always：不省略括号
	arrowParens: "avoid",
	// 如果文件顶部已经有一个 doclock，这个选项将新建一行注释，并打上@format标记。
	insertPragma: false,
	// 指定要使用的解析器，不需要写文件开头的 @prettier
	requirePragma: false,
	// 默认值。因为使用了一些折行敏感型的渲染器（如GitHub comment）而按照markdown文本样式进行折行
	proseWrap: "preserve",
	// 在html中空格是否是敏感的 "css" - 遵守CSS显示属性的默认值， "strict" - 空格被认为是敏感的 ，"ignore" - 空格被认为是不敏感的
	htmlWhitespaceSensitivity: "css",
	// 换行符使用 lf 结尾是 可选值"<auto|lf|crlf|cr>"
	endOfLine: "auto",
	// 这两个选项可用于格式化以给定字符偏移量（分别包括和不包括）开始和结束的代码
	rangeStart: 0,
	rangeEnd: Infinity,
	// Vue文件脚本和样式标签缩进
	vueIndentScriptAndStyle: false,
};
```

为了测试是否生效，我故意把句末是否使用分号写成了false(上文中eslint是需要分号的)。执行npx prettier --write src/，可以看到，分号被取消了，但是不满足eslint规则，报错了：
![1709792312307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792312307.png)
**冲突的问题稍后再说，这里只是说明我们配置的.prettierrc.js是生效的。**

#### 5，prettier插件保存代码自动修复
同样的，我们不可能每写一行代码，就运行npx prettier --write src/来美化一次代码，我们希望保存代码时，就能够自动格式化代码。于是又需要安装prettier插件。、
![1709792327662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792327662.png)
然后再ctrl+shift+p打开vscode的setting.json文件，添加如下配置：

```javascript
 //prettier可以格式化很多种格式，所以需要在这里对应配置下
 "[html]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[less]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescriptreact]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[jsonc]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  //这个设置在ctrl+s的时候，会启用默认的格式化，这里是prettier
  "editor.formatOnSave": true

```
这时候按保存代码，会发现在闪动，依旧是报eslint的错：
这是因为eslint的规则和prettier冲突了，当我们保存的时候。先执行eslint的自动修复，于是分号会加上。然后又执行prettier的修复，分号又去除，这就导致闪动，到头来还是报eslint的错。这里主要是体现prettier在保存代码时能够自动修复，冲突的问题依旧先放到后面说。

#### 6，让编译器能报prettier的错
到目前为止，对于prettier,我们还无法查看哪里不符合规则，而只是通过自动修复来规范代码风格。现在我们想像eslint一样，代码一写，如果不符合，就出现红色的波浪线提示哪里有问题。

这个要怎么实现呢？可以利用eslint的报错，把prettier当成eslint的插件注入eslint中，让eslint来报这个错（实际上还是vscode的eslint实现的）
**安装依赖：**
```javascript
npm i -D eslint-plugin-prettier

```
**然后再在.eslintrc.js 配置文件中添加这个配置，意思就是使用 eslint 报prettier的错误：**

```javascript
// .eslintrc.js
{
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}

```
这样设置后，我故意把代码排版变乱：
![1709792351182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792351182.png)
可以注意到，编辑器已经可以同时报eslint和prettier的错误了。

这时保存下代码，除了冲突的部分，都会被自动修复。

接下来可以着手解决这两家伙冲突的问题了。
#### 7，解决eslint和prettier的冲突问题
先说一个事情，其实上文中我反复说的冲突。并不是真正意义上的eslint和prettier的冲突。因为上文的冲突，eslint的规则设置在.eslintrc.js的rules中，prettier的设置在prettierrc.js中，这两者都是我们开发者自己设置的！这分明是前端程序员自己傻逼，给编辑器下绊子！（这么说大家别打我，我还年轻还没娶媳妇……）

我解释一下冲突的缘由，就能够理解我为啥这么说了。

首先，我们是安装了eslint。

那这个eslint要能校验代码，它肯定是有一套默认的代码规范的。

上文中第一章节第三点说过

```javascript
"extends": [
    "eslint:recommended",//继承Eslint中推荐的（打钩的）规则项http://eslint.cn/docs/rules/
    "plugin:vue/essential"// 此项是用来配置vue.js风格
],

```
这里的eslint:recommended就是这套默认规则，当然有时候我们不用这套规则。会用其他成熟的规则方案例如Airbnb 规范。比如这里我就额外引入了plugin:vue/essential，作为vue文件的规范。

这里需要再强调一点，这个extends数组中的规则，后面的会覆盖前面的，也就是vue/essential会覆盖掉recommended中的重复部分。

并且这里的规则是由安装依赖引入的，存放在node_modules文件夹中，也就是为了保证其他开发人员代码一致，这里面的文件是不允许改动的。

**所以说eslint和prettier的冲突问题，其实说的是这些依赖引入的规则和prettier的冲突！
而不是你自己配置的eslint中的rules和你自己配置的prettier冲突。**

为了证明我的说法，我去node_modules/eslint/conf/eslint-recommended.js增加一个eslint规则：（这个文件中设置的规则上文说过是官网中打了勾的部分，quotes不在默认规则里，为了好理解，我手动添加一个）
![1709792374584](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792374584.png)
现在eslint中的配置是这样，我把自定义的配置关闭掉，prettier中是这样。即eslint默认配置需要双引号，prettier设置需要单引号，两者是冲突的：
![1709792402726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792402726.png)
这时候我再按ctrl+s保存代码，就可以看到两者冲突了。（修改后未生效，需要重启vscode）:
网上说的解决冲突的依赖安装一下：

```javascript
npm i -D eslint-config-prettier

```
把prettier设置的规则添加到extends数组中：

```javascript
  extends: [
    'eslint:recommended', //继承Eslint中推荐的（打钩的）规则项http://eslint.cn/docs/rules/
    'plugin:vue/essential', // 此项是用来配置vue.js风格
    'prettier',//把prettier中设置的规则添加进来，让它覆盖上面设置的规则。这样就不会和上面的规则冲突了
  ],

```
这样一来，就让在它之前的所有可能会与 prettier 规则存在冲突的 eslint规则失效，并使用 prettier 的规则进行代码检查。

接着，在项目中ctrl+s报错代码，可以看到报错消失，变成了prettier设置的单引号了：
![1709792432641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709792432641.png)
这才是真正意义上的解决冲突。

接下来，再来说rules中的冲突怎么办？它的优先级要高于extends中的规则，而extends中冲突的规则已经被prettier搞失效了，你又在rules中再定义，于是又会和prettier中产生冲突。

也就是说，**上文解决冲突只会解决extends中的冲突。不会解决rules中的冲突！**

那我们这种冲突应该怎么解决？

> 第一种方案：把自己想要的规则配置成npm包发布，然后引入到这个extends数组中。
> 
> 第二种方案：relus中的配置和prettier中的保持一致即可。
#### 8，让没有安装prettier的项目也能代码格式化
有的时候，我们的项目并没有配置prettier,而我们希望在vscode中可以在保存代码时美化代码。就可以在ctrl+shift+p打开setting.json配置prettier规则：

```javascript
 /*  prettier的配置 */
  "prettier.printWidth": 80, // 超过最大值换行
  "prettier.tabWidth": 2, // 缩进字节数
  "prettier.useTabs": false, // 句尾添加分号
  "prettier.singleQuote": false, // 使用单引号代替双引号
  "prettier.proseWrap": "preserve", //  (x) => {} 箭头函数参数只有一个时是否要有小括号。avoid：省略括号
  "prettier.bracketSpacing": true, // 在对象，数组括号与文字之间加空格 "{ foo: bar }"
  "prettier.endOfLine": "auto", // 结尾是 \n \r \n\r auto
  "prettier.eslintIntegration": false, //不让prettier使用eslint的代码格式进行校验
  "prettier.htmlWhitespaceSensitivity": "ignore",
  "prettier.ignorePath": ".prettierignore", // 不使用prettier格式化的文件填写在项目的.prettierignore文件中
  "prettier.jsxBracketSameLine": false, // 在jsx中把'>' 是否单独放一行
  "prettier.jsxSingleQuote": false, // 在jsx中使用单引号代替双引号
  "prettier.parser": "babylon", // 格式化的解析器，默认是babylon
  "prettier.requireConfig": false, // Require a 'prettierconfig' to format prettier
  "prettier.stylelintIntegration": false, //不让prettier使用stylelint的代码格式进行校验
  "prettier.trailingComma": "none", // 在对象或数组最后一个元素后面是否加逗号（在ES5中加尾逗号）
  "prettier.tslintIntegration": false,
  "prettier.arrowParens": "avoid"

```
#### 9，总结
反思问题：

既然vscode中设置了pettier(上文第八点)，已经能够格式化代码了，还要安装prettier依赖，去配置格式。

> 原因和eslint中一样，vscode中配置的，其他人也许不用vscode，也许安装了vscode,但是没有在setting.json中设置规范。而项目读取prettier规范时有一个优先级的，如果在根目录找到了.prettier文件，则不会去查找setting.json中的规则了。只有找不到.prettier文件时，我们配置在setting.json中的规则才会生效。

### 三、提交代码时eslint校验
#### 1，husky
为了保证每次提交的 git 代码是正确的，为此我们可以使用 eslint 配合 git hook， 在进行git commit 的时候验证eslint规范

如果 eslint 验证不通过，则不能提交。

我们需要安装一个 git 的 hook 工具 – husky （我刚开始安装的是最新版本，发现没有生效，回退后才生效）

```javascript
npm install husky@4.3.8 --save-dev

```
然后在package.json中增加配置：

```javascript
"husky": {
    "hooks": {
      "pre-commit": "echo 'husky' && npm run lint"
    }
  }

```
意思是在进行 git commit 的时候 先去执行 pre-commit 里面的命令 ： 我们在这里输出 husky 并且执行 npm run lint (我们在上文第一章，第5点加上的验证eslint的命令)

如果eslint验证通过了，则会进行commit 操作，否则会报eslint的错误提示。

生效的标志是项目的.git/hooks目录下，会生成一堆文件，原本是只有pre-commit.sample这类文件，等husky安装完成，会多出pre-commit等文件。

再运行git commit命令，就会检查代码了。
#### 2，lint-staged
> **如果一个项目中期才引入husky，那么我们只想对git add到暂存区的文件进行git hook的脚本触发，那么可以借助lint-staged来实现。**

如果这是一个新项目以上的就已经满足要求了，但是如果拿到的项目是一个老项目呢，别人开发了很久，这个时候加入再加入 eslint 规则，全局去检查，会发现一堆报错信息。这个就慌了。修改可能带来其他问题。

为了解决这种问题，我们就需要引入 lint-staged

lint-staged 的作用是只对 git add 缓存区的代码进行 eslint 代码规范校验。这样就避免了全局校验的问题。你修改了上面代码，你就提交了什么代码，其他代码不做 eslint 校验。

```javascript
npm install --save-dev lint-staged
```
在 package.json 中添加：

```javascript
"lint-staged": {
    "src/**/*.{css，scss,less}": [
      "npm run lint",
      "git commit"
    ],
    "src/**/*.{js,vue}": [
      "npm run lint",
      "git commit"
    ]
  }
```
