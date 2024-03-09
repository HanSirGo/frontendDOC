```javascript
module.exports = {
  root: true,
  parserOptions: {
    parser: 'babel-eslint',
    sourceType: 'module'
  },
  env: {
    browser: true,
    node: true,
    es6: true,
  },
  extends: ['plugin:vue/recommended', 'eslint:recommended'],

  // add your custom rules here
  //it is base on https://github.com/vuejs/eslint-config-vue
  rules: {
    "vue/max-attributes-per-line": [2, {
      "singleline": 10,
      "multiline": {
        "max": 1,
        "allowFirstLine": false //多个特性的元素应该分多行撰写，每个特性一行
      }
    }],
    "vue/singleline-html-element-content-newline": "off",//双标签不能在一行
    "vue/multiline-html-element-content-newline":"off",
    "vue/name-property-casing": ["error", "PascalCase"],//首字母大写，然后直接连接起来，单词之间没有连接
    "vue/no-v-html": "off",//避免使用v-html指令
    'accessor-pairs': 2,//setter 的属性设置一个 getter
    'arrow-spacing': [2, {
      'before': true,
      'after': true
    }], //箭头函数前后空格一致
    'block-spacing': [2, 'always'],//always默认需要多一个空格，never不需要空格
    'brace-style': [2, '1tbs', {
      'allowSingleLine': true //（默认）强制执行一个真正的大括号,风格"allowSingleLine": true（默认false）允许一个块打开和关闭括号在同一行上
    }],
    'camelcase': [0, {
      'properties': 'always'//（默认）为属性名称强制执行,never可关闭
    }],
    'comma-dangle': [2, 'never'],//（默认）不允许尾随逗号
    'comma-spacing': [2, {
      'before': false,
      'after': true
    }], //逗号前不需要有空格，逗号后需要有一个空格
    'comma-style': [2, 'last'],//逗号在后面
    'constructor-super': 2,//派生类的构造函数必须调用super()
    'curly': [2, 'multi-line'],//一个块可以省略花括号
    'dot-location': [2, 'property'],//点与属性放在同一行
    'eol-last': 2,//文件末尾换行
    'eqeqeq': ["error", "always", {"null": "ignore"}],//永远用全等，null不用于此规则
    'generator-star-spacing': [2, {
      'before': true,
      'after': true
    }],//关键字和function和函数之间都需要一个空格
    'handle-callback-err': [2, '^(err|error)$'],//处理异步行为的时候处理error回调
    'indent': [2, 2, {
      'SwitchCase': 1   //switch的case语句用两个空格的缩进
    }],
    'jsx-quotes': [2, 'prefer-single'],//属性值使用单引号
    'key-spacing': [2, {
      'beforeColon': false,
      'afterColon': true
    }],
    'keyword-spacing': [2, {
      'before': true,
      'after': true
    }],//对象的键和:需要空格，键值和：需要空格
    'new-cap': [2, {
      'newIsCap': true,
      'capIsNew': false//要求new使用大写启动函数调用所有操作符。要求所有大写启动的函数不用与new操作符一起调用
    }],
    'new-parens': 2,//在使用new关键字调用不带参数的构造函数时需要括号
    'no-array-constructor': 2,//此规则不允许使用Array构造函数
    'no-caller': 2,//不可能使用arguments.caller并arguments.callee进行几次代码优化
    'no-console': 'off',//此规则禁止调用console对象的方法。
    'no-class-assign': 2,//class类创建变量，不能修改变量
    'no-cond-assign': 2,//在条件语句中，将比较运算符（如==）作为赋值运算符（例如=）是错误的
    'no-const-assign': 2,//不能修改使用const关键字声明的变量
    'no-control-regex': 0,//此规则不允许正则表达式中的控制字符
    'no-delete-var': 2,//此规则不允许在变量上使用delete操作符
    'no-dupe-args': 2,//此规则不允许在函数声明或表达式中使用重复的参数名称
    'no-dupe-class-members': 2,//此规则旨在标记class在级别成员中使用重复名称。
    'no-dupe-keys': 2,//此规则不允许在对象文字中使用重复键。
    'no-duplicate-case': 2,//不允许在switch语句的case子句中使用重复的测试表达式。
    'no-empty-character-class': 2,//不允许在正则表达式中使用空字符类。
    'no-empty-pattern': 2,//标记解构结构对象和数组中的任何空模式,用等号
    'no-eval': 2,//不用evel
    'no-ex-assign': 2,//不允许在catch子句中重新分配例外
    'no-extend-native': 2,//不允许扩展名
    'no-extra-bind': 2,//箭头函数不能用bind，不能滥用bind
    'no-extra-boolean-cast': 2,//禁止不必要的布尔转换
    'no-extra-parens': [2, 'functions'],//仅在函数表达式附近禁止不必要的括号
    'no-fallthrough': 2,//switch case的失败处理用法
    'no-floating-decimal': 2,//消除浮点小数点，并在数值有小数点但在其之前或之后缺少数字时发出警告
    'no-func-assign': 2,//但覆盖/重新分配写为 FunctionDeclaration 的函数
    'no-implied-eval': 2,//setTimeout()和setInterval()不能使用字符串参数
    'no-inner-declarations': [2, 'functions'],//不允许function嵌套块中的声明
    'no-invalid-regexp': 2,//不允许RegExp构造函数中的无效正则表达式字符串
    'no-irregular-whitespace': 2,
    'no-iterator': 2,//防止使用该__iterator__属性时可能出现的错误
    'no-label-var': 2,//禁止创建与范围内的变量共享名称的标签
    'no-labels': [2, {
      'allowLoop': false,
      'allowSwitch': false//break或continue用于switch语句
    }],
    'no-lone-blocks': 2,//块级绑定（let和const），类声明或函数声明（以严格模式）存在，则代码块可能会创建新范围
    'no-mixed-spaces-and-tabs': 2,//不允许使用混合空格和制表符进行缩进
    'no-multi-spaces': 2,//禁止在逻辑表达式，条件表达式，声明，数组元素，对象属性，序列和函数参数周围使用多个空格
    'no-multi-str': 2,//防止使用多行字符串
    'no-multiple-empty-lines': [2, {
      'max': 1                          //减少阅读代码时所需的滚动。它会在超过最大空行数量1时发出警告
    }],
    'no-native-reassign': 2,//不允许修改只读全局变量
    'no-negated-in-lhs': 2,//不允许否定in表达式中的左操作数
    'no-new-object': 2,//不允许使用Object构造函数
    'no-new-require': 2,//消除new require表达的使用
    'no-new-symbol': 2,//防止Symbol与new一起使用
    'no-new-wrappers': 2,//杜绝使用String，Number以及Boolean与new操作
    'no-obj-calls': 2,//不允许调用Math，JSON和Reflect对象作为功能
    'no-octal': 2,//不允许使用八进制文字
    'no-octal-escape': 2,//不允许字符串文字中的八进制转义序列,应该使用 Unicode 转义序列
    'no-path-concat': 2,//防止 Node.js 中的目录路径字符串连接
    'no-proto': 2,//当一个对象被__proto__创建时被设置为该对象的构造函数的原始原型属性。getPrototypeOf是获得“原型”的首选方法。
    'no-redeclare': 2,//消除在同一范围内具有多个声明的变量
    'no-regex-spaces': 2,//正则表达式文字中不允许有多个空格
    'no-return-assign': [2, 'except-parens'],//return中除非用圆括号括起来，否则不允许赋值
    'no-self-assign': 2,//消除自我分配
    'no-self-compare': 2,//较变量与自身通常是错误
    'no-sequences': 2,//则禁止使用逗号运算符,除了for循环和圆括号的表达式
    'no-shadow-restricted-names': 2,//NaN，Infinity，undefined,不要定义
    'no-spaced-func': 2,//不允许功能标识符与其应用程序之间的间距
    'no-sparse-arrays': 2,//不允许稀疏数组文字，它们在逗号前没有元素的地方有“孔”
    'no-this-before-super': 2,//在派生类的构造函数中，如果在调用之前使用this/ ，则会引发参考错误
    'no-throw-literal': 2,//不允许抛出不可能是Error对象的文字
    'no-trailing-spaces': 2,//不允许在行尾添加尾随空白
    'no-undef': 2,//任何对未声明的变量的引用都会导致警告,除非该变量在/*global ...*/注释中明确提到
    'no-undef-init': 2,//消除初始化为的变量声明undefined
    'no-unexpected-multiline': 2,//不允许混淆多行表达式
    'no-unmodified-loop-condition': 2,//循环中的变量经常在循环中修改。如果不是，那可能是一个错误
    'no-unneeded-ternary': [2, {
      'defaultAssignment': false   //允许条件表达式作为默认分配模式
    }],
    'no-unreachable': 2,//return，throw，break，和continue语句无条件退出的代码块，之后他们的任何语句可以不被执行
    'no-unsafe-finally': 2,//try和catch,和finally的使用
    'no-unused-vars': [2, {
      'vars': 'all',
      'args': 'none'  //消除未使用的变量，函数和函数的参数
    }],
    'no-useless-call': 2,//Function.prototype.call()并且Function.prototype.apply()可以用正常的函数调用来替代
    'no-useless-computed-key': 2,//禁止不必要地使用计算属性键
    'no-useless-constructor': 2,//没有必要将两个字符串连接在一起
    'no-useless-escape': 0,//转义字符串，模板文字和正则表达式中的非特殊字符不会产生任何影响
    'no-whitespace-before-property': 2,//如果对象的属性位于同一行上，则该规则不允许围绕点或在开头括号之前留出空白
    'no-with': 2,//不允许with声明
    'one-var': [2, {
      'initialized': 'never' //变量要先声明，再赋值
    }],
    'operator-linebreak': [2, 'after', {
      'overrides': {
        '?': 'before',
        ':': 'before'   //需要将换行符置于操作员之前，需要将换行符放在操作员面前
      }
    }],
    'padded-blocks': [2, 'never'],//块里面不要有空行
    'quotes': [2, 'single', {
      'avoidEscape': true,
      'allowTemplateLiterals': true  //允许使用反引号，双引号或单引号
    }],
    'semi': [2, 'never'],//末尾不需要分号
    'semi-spacing': [2, {
      'before': false,//强制分号间隔。此规则可防止在表达式中使用分号之前的空格
      'after': true
    }],
    'space-before-blocks': [2, 'always'],//将强化块之前的间距一致性,块的小括号和花括号有一个空格
    'space-before-function-paren': [2, 'never'],//never在(参数后面不允许任何空格
    'space-in-parens': [2, 'never'],//"never" （默认）在圆括号内强制使用零空格
    'space-infix-ops': 2, //确保中缀操作员周围有空间
    'space-unary-ops': [2, {
      'words': true,
      'nonwords': false //new有间隔,运算符没有间隔
    }],
    'spaced-comment': [2, 'always', {
      'markers': ['global', 'globals', 'eslint', 'eslint-disable', '*package', '!', ',']//此规则将强制间距的一致性//或/*
    }],
    'template-curly-spacing': [2, 'never'],//不允许大括号内的空格。
    'use-isnan': 2,//不允许比较'NaN'
    'valid-typeof': 2,//typeof表达式与有效的字符串文字进行比较
    'wrap-iife': [2, 'any'],//要求所有立即调用的函数表达式都包含在圆括号中
    'yield-star-spacing': [2, 'both'],//强制执行*周围 yield*表达式的间距,两个空格
    'yoda': [2, 'never'],//强制执行一种将变量与文字值进行比较的一致条件样式
    'prefer-const': 2,//初始分配后从未重新分配变量
    'no-debugger': process.env.NODE_ENV === 'production' ? 2 : 0,
    'object-curly-spacing': [2, 'always', {
      objectsInObjects: false          //需要大括号内的空格（{}除外）
    }],
    'array-bracket-spacing': [2, 'never']//数组括号内强制实现一致的间距
  }
}
```

```javascript
"no-alert": 0,//禁止使用alert confirm prompt
"no-catch-shadow": 2,//禁止catch子句参数与外部作用域变量同名？
"no-class-assign": 2,//禁止给类赋值？
"no-const-assign": 2,//禁止修改const声明的变量
"no-constant-condition": 2,//禁止在条件中使用常量表达式 if(true) if(1)
"no-continue": 0,//禁止使用continue
"no-control-regex": 2,//禁止在正则表达式中使用控制字符？
"no-debugger": 2,//禁止使用debugger
"no-delete-var": 2,//不能对var声明的变量使用delete操作符？
"no-div-regex": 1,//不能使用看起来像除法的正则表达式/=foo/
"no-dupe-keys": 2,//在创建对象字面量时不允许键重复 {a:1,a:1}
"no-dupe-args": 2,//同个函数参数不能重复
"no-duplicate-case": 2,//switch中的case标签不能重复
"no-else-return": 2,//如果if语句里面有return,后面不能跟else语句
"no-empty": 2,//块语句中的内容不能为空
"no-empty-character-class": 2,//正则表达式中的[]内容不能为空
"no-eq-null": 2,//禁止对null使用==或!=运算符
"no-eval": 1,//禁止使用eval
"no-ex-assign": 2,//禁止给catch语句中的异常参数赋值
"no-extend-native": 2,//禁止扩展native对象
"no-extra-bind": 2,//禁止不必要的函数绑定
"no-extra-boolean-cast": 2,//禁止不必要的bool转换
"no-extra-parens": 2,//禁止非必要的括号
"no-extra-semi": 2,//禁止多余的冒号
"no-fallthrough": 1,//禁止switch穿透
"no-floating-decimal": 2,//禁止省略浮点数中的0 .5 3.
"no-func-assign": 2,//禁止重复的函数声明
"no-implicit-coercion": 1,//禁止隐式转换
"no-implied-eval": 2,//禁止使用隐式eval
"no-inline-comments": 0,//禁止行内备注
"no-inner-declarations": [2, "functions"],//禁止在块语句中使用声明（变量或函数）
"no-invalid-regexp": 2,//禁止无效的正则表达式
"no-invalid-this": 2,//禁止无效的this，只能用在构造器，类，对象字面量
"no-irregular-whitespace": 2,//不能有不规则的空格
"no-iterator": 2,//禁止使用__iterator__ 属性
"no-label-var": 2,//label名不能与var声明的变量名相同
"no-labels": 2,//禁止标签声明
"no-lone-blocks": 2,//禁止不必要的嵌套块
"no-lonely-if": 2,//禁止else语句内只有if语句
"no-loop-func": 1,//禁止在循环中使用函数（如果没有引用外部变量不形成闭包就可以）
"no-mixed-requires": [0, false],//声明时不能混用声明类型
"no-mixed-spaces-and-tabs": [2, false],//禁止混用tab和空格
"linebreak-style": [0, "windows"],//换行风格
"no-multi-spaces": 1,//不能用多余的空格
"no-multi-str": 2,//字符串不能用\换行
"no-multiple-empty-lines": [1, {"max": 2}],//空行最多不能超过2行
"no-native-reassign": 2,//不能重写native对象
"no-negated-in-lhs": 2,//in 操作符的左边不能有!
"no-nested-ternary": 0,//禁止使用嵌套的三目运算
"no-new": 1,//禁止在使用new构造一个实例后不赋值
"no-new-func": 1,//禁止使用new Function
"no-new-object": 2,//禁止使用new Object()
"no-new-require": 2,//禁止使用new require
"no-new-wrappers": 2,//禁止使用new创建包装实例，new String new Boolean new Number
"no-obj-calls": 2,//不能调用内置的全局对象，比如Math() JSON()
"no-octal": 2,//禁止使用八进制数字
"no-octal-escape": 2,//禁止使用八进制转义序列
"no-param-reassign": 2,//禁止给参数重新赋值
"no-path-concat": 0,//node中不能使用__dirname或__filename做路径拼接
"no-plusplus": 0,//禁止使用++，--
"no-process-env": 0,//禁止使用process.env
"no-process-exit": 0,//禁止使用process.exit()
"no-proto": 2,//禁止使用__proto__属性
"no-redeclare": 2,//禁止重复声明变量
"no-regex-spaces": 2,//禁止在正则表达式字面量中使用多个空格 /foo bar/
"no-restricted-modules": 0,//如果禁用了指定模块，使用就会报错
"no-return-assign": 1,//return 语句中不能有赋值表达式
"no-script-url": 0,//禁止使用javascript:void(0)
"no-self-compare": 2,//不能比较自身
"no-sequences": 0,//禁止使用逗号运算符
"no-shadow": 2,//外部作用域中的变量不能与它所包含的作用域中的变量或参数同名
"no-shadow-restricted-names": 2,//严格模式中规定的限制标识符不能作为声明时的变量名使用
"no-spaced-func": 2,//函数调用时 函数名与()之间不能有空格
"no-sparse-arrays": 2,//禁止稀疏数组， [1,,2]
"no-sync": 0,//nodejs 禁止同步方法
"no-ternary": 0,//禁止使用三目运算符
"no-trailing-spaces": 1,//一行结束后面不要有空格
"no-this-before-super": 0,//在调用super()之前不能使用this或super
"no-throw-literal": 2,//禁止抛出字面量错误 throw "error";
"no-undef": 1,//不能有未定义的变量
"no-undef-init": 2,//变量初始化时不能直接给它赋值为undefined
"no-undefined": 2,//不能使用undefined
"no-unexpected-multiline": 2,//避免多行表达式
"no-underscore-dangle": 1,//标识符不能以_开头或结尾
"no-unneeded-ternary": 2,//禁止不必要的嵌套 var isYes = answer === 1 ? true : false;
"no-unreachable": 2,//不能有无法执行的代码
"no-unused-expressions": 2,//禁止无用的表达式
"no-unused-vars": [2, {"vars": "all", "args": "after-used"}],//不能有声明后未被使用的变量或参数
"no-use-before-define": 2,//未定义前不能使用
"no-useless-call": 2,//禁止不必要的call和apply
"no-void": 2,//禁用void操作符
"no-var": 0,//禁用var，用let和const代替
"no-warning-comments": [1, { "terms": ["todo", "fixme", "xxx"], "location": "start" }],//不能有警告备注
"no-with": 2,//禁用with

"array-bracket-spacing": [2, "never"],//是否允许非空数组里面有多余的空格
"arrow-parens": 0,//箭头函数用小括号括起来
"arrow-spacing": 0,//=>的前/后括号
"accessor-pairs": 0,//在对象中使用getter/setter
"block-scoped-var": 0,//块语句中使用var
"brace-style": [1, "1tbs"],//大括号风格
"callback-return": 1,//避免多次调用回调什么的
"camelcase": 2,//强制驼峰法命名
"comma-dangle": [2, "never"],//对象字面量项尾不能有逗号
"comma-spacing": 0,//逗号前后的空格
"comma-style": [2, "last"],//逗号风格，换行时在行首还是行尾
"complexity": [0, 11],//循环复杂度
"computed-property-spacing": [0, "never"],//是否允许计算后的键名什么的
"consistent-return": 0,//return 后面是否允许省略
"consistent-this": [2, "that"],//this别名
"constructor-super": 0,//非派生类不能调用super，派生类必须调用super
"curly": [2, "all"],//必须使用 if(){} 中的{}
"default-case": 2,//switch语句最后必须有default
"dot-location": 0,//对象访问符的位置，换行的时候在行首还是行尾
"dot-notation": [0, { "allowKeywords": true }],//避免不必要的方括号
"eol-last": 0,//文件以单一的换行符结束
"eqeqeq": 2,//必须使用全等
"func-names": 0,//函数表达式必须有名字
"func-style": [0, "declaration"],//函数风格，规定只能使用函数声明/函数表达式
"generator-star-spacing": 0,//生成器函数*的前后空格
"guard-for-in": 0,//for in循环要用if语句过滤
"handle-callback-err": 0,//nodejs 处理错误
"id-length": 0,//变量名长度
"indent": [2, 4],//缩进风格
"init-declarations": 0,//声明时必须赋初值
"key-spacing": [0, { "beforeColon": false, "afterColon": true }],//对象字面量中冒号的前后空格
"lines-around-comment": 0,//行前/行后备注
"max-depth": [0, 4],//嵌套块深度
"max-len": [0, 80, 4],//字符串最大长度
"max-nested-callbacks": [0, 2],//回调嵌套深度
"max-params": [0, 3],//函数最多只能有3个参数
"max-statements": [0, 10],//函数内最多有几个声明
"new-cap": 2,//函数名首行大写必须使用new方式调用，首行小写必须用不带new方式调用
"new-parens": 2,//new时必须加小括号
"newline-after-var": 2,//变量声明后是否需要空一行
"object-curly-spacing": [0, "never"],//大括号内是否允许不必要的空格
"object-shorthand": 0,//强制对象字面量缩写语法
"one-var": 1,//连续声明
"operator-assignment": [0, "always"],//赋值运算符 += -=什么的
"operator-linebreak": [2, "after"],//换行时运算符在行尾还是行首
"padded-blocks": 0,//块语句内行首行尾是否要空行
"prefer-const": 0,//首选const
"prefer-spread": 0,//首选展开运算
"prefer-reflect": 0,//首选Reflect的方法
"quotes": [1, "single"],//引号类型 `` "" ''
"quote-props":[2, "always"],//对象字面量中的属性名是否强制双引号
"radix": 2,//parseInt必须指定第二个参数
"id-match": 0,//命名检测
"require-yield": 0,//生成器函数必须有yield
"semi": [2, "always"],//语句强制分号结尾
"semi-spacing": [0, {"before": false, "after": true}],//分号前后空格
"sort-vars": 0,//变量声明时排序
"space-after-keywords": [0, "always"],//关键字后面是否要空一格
"space-before-blocks": [0, "always"],//不以新行开始的块{前面要不要有空格
"space-before-function-paren": [0, "always"],//函数定义时括号前面要不要有空格
"space-in-parens": [0, "never"],//小括号里面要不要有空格
"space-infix-ops": 0,//中缀操作符周围要不要有空格
"space-return-throw-case": 2,//return throw case后面要不要加空格
"space-unary-ops": [0, { "words": true, "nonwords": false }],//一元运算符的前/后要不要加空格
"spaced-comment": 0,//注释风格要不要有空格什么的
"strict": 2,//使用严格模式
"use-isnan": 2,//禁止比较时使用NaN，只能用isNaN()
"valid-jsdoc": 0,//jsdoc规则
"valid-typeof": 2,//必须使用合法的typeof的值
"vars-on-top": 2,//var必须放在作用域顶部
"wrap-iife": [2, "inside"],//立即执行函数表达式的小括号风格
"wrap-regex": 0,//正则表达式字面量用小括号包起来
"yoda": [2, "never"]//禁止尤达条件
```
