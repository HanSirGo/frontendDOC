# AST抽象语法树

## 什么是抽象语法树(Abstract Syntax Tree)？

它是源代码的抽象语法结构的树状表现形式，它以树状的形式描述了代码结构，树上的每个节点都表示源代码中的一种结构。

构建抽象语法树的过程称为`解析（parsing）`。解析通常需要词法分析器和语法分析器。词法分析器用于将文本拆分为有意义的`标记（token）`，语法分析器根据标记构建抽象语法树。

抽象语法树本质上是一个**JS对象**。

### 为什么需要抽象语法树？

`模板语法`变成`正常的HTML语法`难度比较大，所以需要抽象语法树来辅助编译器将模板编译成正常HTML语法。

所以需要通过抽象语法树周转，通过`模板语法`变成`抽象语法树AST`在变成`正常的HTML语法`

而通过抽象语法树变成真实的DOM的过程是可以进行优化的，那么就可以通过`虚拟节点+diff算法`进行`优化`。

### 抽象语法树和虚拟节点的关系

```html
<div id="app">  
 <h1>{{ message }}</h1>  
 <button @click="increment">Increment</button>  
</div>
```

编译器会将模板生成一个JS对象（AST抽象语法树），类似：

```js
 {  
  "tag": "div",  
  "attrs": [  
    { "name": "id", "value": "app" }  
  ], 
  "children": [  
    {  
      "tag": "h1",  
      "children": [  
        {  
          "type": "text",   
          "content": "{{ message }}"  
        }  
      ]  
    },  
    {  
      "tag": "button",  
      "events": {  
        "click": "increment"  
      },  
      "children": [  
        {  
          "type": "text",  
          "content": "Increment"  
        }  
      ]  
    }  
  ]  
}
```

Vue.js的编译器会遍历这个AST，并根据AST的节点生成对应的渲染函数，渲染函数包含一系列的h函数的调用，生成虚拟节点(h函数生成虚拟节点并进行diff比较后上树下一篇进行介绍)。

```js
render(h) {  
  return h('div', { attrs: { id: 'app' } }, [  
    h('h1', null, this.message), // 假设有逻辑处理将插值转换为数据  
    h('button', { on: { click: this.increment } }, 'Increment')  
  ]);  
}
```

> 抽象语法树不会直接生成虚拟节点，而是通过h函数生成。

> AST的生成和处理是为了最终生成渲染函数，而渲染函数中的h函数调用则是AST的终点。

### 相关算法储备：

编写”智能重复“smartRepeat函数

- 将`3[abc]`变为`abcabcabc`
- 将`3[2[a]2[b]]`变为`aabbaabbaabb`
- 将`2[3[aa]4[2[b]2[c]]]`变为`aaaaaabbccbbccbbccbbccaaaaaabbccbbccbbccbbcc`

实现思路：

- 遍历每一个字符。
- 如果这个字符是`数字+[`，那么就把`数字`和一个`空字符串`，放到一个对象中，并把这个对象进行`入栈`。
- 如果这个字符是`字母+]`，就把`栈顶对象`中的str属性`追加`上字母。
- 如果这个字符串是`]`，`出栈`得到对象，然后把str属性值重复num次，追加到栈顶对象中str属性值。
- 循环结束后，还要需要出栈一次得到对象，然后把str属性值重复num次，返回结果。

 

```js
 let str = '2[3[aa]4[2[b]2[c]]]';
  function smartRepeat (templateStr) {
    let stack = [],
      result = '',
      i = 0;
    let rest = templateStr;
    /* 匹配数字和[，并以数字开头 */
    const regExp1 = /^(\d+)\[/;
    /* 匹配字母和]，并以字母开头 */
    const regExp2 = /^(\w+)\]/;
    while (i < templateStr.length - 1) {
      rest = templateStr.substring(i);
      // console.log(rest);
      if (regExp1.test(rest)) {
        const num = +rest.match(regExp1)[1];
        // /* 跳过一些遍历。加1是[长度的值 */
        i += num.toString().length + 1;
        stack.push({
          num,
          str: ''
        });
      } else if (regExp2.test(rest)) {
        const str1 = rest.match(regExp2)[1];
        stack[stack.length - 1].str += str1;
        /* 跳过一些遍历 */
        i += str1.length;
      } else if (rest[0] === ']') {
        i++;
        const { str, num } = stack.pop();
        stack[stack.length - 1].str += str.repeat(num);
      }
    }
    console.log(stack);
    const { str, num } = stack.pop();
    return str.repeat(num);
  }
  console.log(smartRepeat(str));
```

### 解析字符串

有了上面的算法，就可以实现模板字符串的解析了。

思路上也差不多：

- 遍历每一个字符
- 解析出 开头`<` 结束`>`中的`标签`和`属性`，如`h3`，`class="box cc dd" id="mybox"`。创建出AST节点并`入栈`。
- 解析出 `{{` `}}`中的`变量`，如`{{name}}`，或者`常量`，如`你好`。把结果放入到栈顶元素的`children`属性中，记得标记类型为`text`。
- 解析出 类似 `</div>`结束标签，需要`出栈`，并添加数据到它的父级的`children`属性中。

代码就不贴了，具体看仓库中的`parse函数`即可。



### 生成render函数(敬请期待)

我一定会去学习补充进来的这一部分的知识点。

下面有必要贴一张图：

![1719145201576](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719145201576.png)

模板字符串-AST-render-vnode-patch

这张图描述了模板字符串如何通过AST生成render函数，然后通过render函数生成虚拟节点，最后通过diff算法进行patch上树的过程。

如何生成vnode和patch，看文章虚拟DOM和diff算法

仓库地址：https://gitee.com/ctzlwzg/ast-study

