## node

#### 安装node

```js
https://nodejs.org/zh-cn/download/
https://blog.csdn.net/qq_40712862/article/details/120231621

选择适合的版本安装
```

![1709257385297](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709257385297.png)

#### path 模块

```
(1)改变路径node:path模块
path 模块：用于处理文件和目录的路径的实用工具，使用前需先引入模块；
先引入：
const path = require('path');

path.join()：用于链接路径，并且会自动转换当前系统路径的分隔符"/"或"\"；

path.resolve()：也是用于链接路径，但却和path.join()方法却很多不同，
而且path.resolve()方法本身就自带一个to的绝对路径参数，也会自动转换分隔符，
在某些场景用起来也方便很多；

__dirname：是node的一个全局变量，获得当前文件所在目录的完整目录名，搭配path一起使用；

话不多说，下面直接开始演示，演示完毕后有总结他们的特性。
```

##### path.resolve() 使用

```js
语法：path.resolve([from ...], to)
```

###### 一：路径为空或__dirname 

```js
演示当"path"路径为空时，得到结果是当前文件所在的绝对路径，类似 __dirname ；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve(''));
console.log(path.resolve(__dirname);
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path
```

###### 二：./开头或者没有字符时，加不加 ./ 都不影响路径的拼接

```js
演示字符以 ./ 开头或者没有字符时，得到的结果是加不加 ./ 都不影响路径的拼接；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('a'));
console.log(path.resolve('a','b'));
console.log(path.resolve('c','b','a'));
console.log(path.resolve('./a'));
console.log(path.resolve('a','./b'));
console.log(path.resolve('./c','b','./a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\Berlin-Case\path\c\b\a
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\Berlin-Case\path\c\b\a
```

###### 三： / 开头，跳转到该盘的根路径，(在哪个盘运行就是哪个盘)

```js
演示字符以 / 开头，path.resolve()的特点之一就是碰到"/"斜杆则会直接跳转到E盘的根路径
(在哪个盘运行就是哪个盘)，这里跟在终端输出cd /是一样的原理，也会跳转到E盘的根路径；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('/a'));
console.log(path.resolve('/a','b','c'));
console.log(path.resolve('a','/b','c'));
console.log(path.resolve('c','b','/a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\a
// 输出：E:\a\b\c
// 输出：E:\b\c
// 输出：E:\a
```

###### 四：../开头，先回上一层

```js
演示字符以../开头，也就是上一层的意思，path.resolve()会把下个要拼接的"path"路径给覆盖掉；

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve());
console.log(path.resolve('../'));
console.log(path.resolve('../a'));
console.log(path.resolve('../a','b'));
console.log(path.resolve('a','../b'));
console.log(path.resolve('c','b','../a'));
console.log(path.resolve('../c','b','a'));
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\
// 输出：E:\Berlin-Case\a
// 输出：E:\Berlin-Case\a\b
// 输出：E:\Berlin-Case\path\b
// 输出：E:\Berlin-Case\path\c\a
// 输出：E:\Berlin-Case\c\b\a
```

###### 五：__dername

```js
演示path.resolve()搭配__dername变量

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.resolve(__dirname,'a'));
console.log(path.resolve('a','b',__dirname));
console.log(path.resolve(__dirname,'./a','b'));
console.log(path.resolve(__dirname,'/a','b'));
console.log(path.resolve(__dirname,'../a','b'));
console.log(path.resolve(__dirname,'a','../b'));
console.log(path.resolve('a','/b',__dirname));
 
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path
// 输出：E:\Berlin-Case\path\a\b
// 输出：E:\a\b
// 输出：E:\Berlin-Case\a\b
// 输出：E:\Berlin-Case\path\b
// 输出：E:\Berlin-Case\path
 从案例五例子可以看出，__dirname变量需放在第一个，否则会覆盖在它之前的'path'路径，包括斜杠' / '，
还有个要注意的点，__dirname之后也不能出现' / '，不然也覆盖之前的路径；
```

###### 总结

```js
path.resolve("./path")的特性有以下几点：

从右往左读取"path"路径,并开始拼接,本身就自带绝对路径参数 "to" ；
当"path"路径为空时，则会直接获取当前文件所在的绝对路径；
当遇到字符以 ./ 开头或者没有字符，则正常拼接，所以可省略不加字符；
当遇到字符以 / 开头，则不会拼接到前面的路径并以自身所在盘为根路径加以拼接；
当遇到字符以 ../ 开头，则会将下一个要拼接"path"路径给覆盖，然后继续往左拼接；
搭配__dirname时，需将放置第一位，且与 ' / ' 有互相覆盖的冲突；
```

##### path.join() 使用

```js
语法：path.join([path1][, path2][, ...])
```

###### 一：无或空时，得到的结果是" . "

```js
演示当"path"路径为无或空时，得到的结果是" . ",只有传入__dirname的时候，才能得到绝对路径

// 引入path模块
const path = require('path'); 
// 此处直接打印
console.log(path.join());
console.log(path.join(''));
console.log(path.join(__dirname));
// 输出：.
// 输出：.
// 输出：E:\Berlin-Case\path
```

###### 二：./ 开头或者 / 和没有字符

```js
演示字符以 ./ 开头或者 / 和没有字符，得到的结果是加不加都不影响路径的拼接，
此时你应该发现跟path.resolve()的有所不同了，因为resolve()只会单纯的去拼接你写入的"path"路径，
而不会像path.resolve()那样用cd去运作；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join());
console.log(path.join('a'));
console.log(path.join('a','b'));
console.log(path.join('c','b','a'));
console.log(path.join('./a'));
console.log(path.join('a','./b'));
console.log(path.join('./c','b','./a'));
console.log(path.join('/a'));
console.log(path.join('a','/b'));
console.log(path.join('/c','b','/a'));
// 输出：.
// 输出：a
// 输出：a\b
// 输出：c\b\a
// 输出：a
// 输出：a\b
// 输出：c\b\a
// 输出：\a
// 输出：a\b
// 输出：\c\b\a
```

###### 三：../ 开头的字符

```js
演示以 ../ 开头的字符，此时你会发现join()不仅是单纯的去拼接路径，而且也是从右到左去拼接的，
../之后还有"path"路径的话，也是会被覆盖掉；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join());
console.log(path.join('../'));
console.log(path.join('../a'));
console.log(path.join('../a','b'));
console.log(path.join('a','../b'));
console.log(path.join('c','b','../a'));
console.log(path.join('../c','b','a'));
 
// 输出：.
// 输出：..\
// 输出：..\a
// 输出：..\a\b
// 输出：b
// 输出：c\a
// 输出：..\c\b\a
```

###### 四：__dername

```js
演示path.join()搭配__dername变量，为什么一定要把它放在第一位？

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join(__dirname,'a'));
console.log(path.join('a',__dirname));
 
// 输出：E:\Berlin-Case\path\a
// 输出：a\E:\Berlin-Case\path
看出区别了吗？对的，没错，join会不管对错，直接把你写入的路径都拼接到一块，
这也是为什么要放在第一位的原因，当然resolve()就没这种问题，接下来继续演示，与字符的搭配；

// 引入path模块
const path = require('path');
 
// 此处直接打印
console.log(path.join(__dirname,'/a'));
console.log(path.join(__dirname,'./a'));
console.log(path.join(__dirname,'../'));
console.log(path.join(__dirname,'../a'));
console.log(path.join(__dirname,'../a','b'));
 
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\path\a
// 输出：E:\Berlin-Case\
// 输出：E:\Berlin-Case\a
// 输出：E:\Berlin-Case\a\b

以上示例可以看出，'/'  './' 这两个字符在path.join()的方法中是不起作用的，不加也是一样的效果，
只有 '../ ' 才有返回上级目录的作用，所以使用path.join()时，加个__dirname，
拼上你要的"path"路径即可；（不加会很麻烦哦）
```

###### 总结

```js
在path.join()方法中，'/' 与 './' 一般情况下可以不用（特殊情况的拼接除外哈）；
在path.join()方法中，最好与__dirname变量搭配使用；
path.join()方法也是从右到左依次被解析排列组成路径的；
path.resolve与path.join的区别
结合上面两个方法的演示后的总结，它们之间的区别如下：

path.resolve()自带to参数，也就是当前输出文件的路径，而path.join()没有；
path.resolve()遇到 ' / ' 则会跳转到根目录(E:\),而path.join()则没效果；
path.resove()搭配__dirname变量使用时，就算__dirname在最右边，
resolve()会把左边的"path"路径给覆盖
```

#### http模块

```js
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

#### fs模块