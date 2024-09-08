# console.('各种打印输出')

## **前言**

`Console` 对象用于 JavaScript 调试，提供了一个命令行接口，用来与网页代码互动，显示网页代码运行时的信息。

JavaScript 原生中默认是没有 Console 对象，这是宿主对象（也就是浏览器）提供的内置对象。用于访问调试控制台, 在不同的浏览器里效果可能不同。

**但是关于console了解多少呢？**

除了我们开发中最常用的`console.log`,console还有很多用法，以下源码都可以一键复制查看。

下面一一介绍：

- 输出文本（最常用的）
- 输出表格
- 输出带样式的文本
- 输出分组打印
- 输出代码运行时间
- 输出ascii文本

## **输出文本（最常用的）**

这个最常用了，没啥可说的了

```
console.log('hello')
console.info("打印info")
console.error("打印err")
```

![1725781173105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781173105.png)

## **输出表格**

我们可以通过`table`方法在控制台输出表格，这样更易于查看数据格式。

```
let table = [
    {name:'雪天',job:'前端',age:26},
    {name:'张三',job:'后端',age:30},
    {name:'李四',job:'设计',age:28},
 ]
console.table(table)
```

![1725781200551](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781200551.png)

![1725781332330](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781332330.png)

## 输出带样式的文本

可以通过`%c`和css给打印的文本添加样式

![1725781213216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781213216.png)

```
console.log(`%c我是`, `color: #00aa7f; font-weight: bold`);
console.log(`%c有样式的`, `color: #ffffff; font-weight: bold;background-color:#00aaff;padding:5px;border-radius:5px;`);
console.log(`%c打印`, `color: #ff007f; font-weight: bold`);
```

![1725781231289](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781231289.png)

## **输出分组打印**

如果控制台打印的东西很多，会让人看起来比较乱，我们可以将打印的东西分类分组打印。

```
console.group('分组标题一');
    console.log('信息1')
    console.log('信息1')
    console.log('信息1')
console.groupEnd();
   
console.group('分组标题二');
    console.log('信息2')
    console.log('信息2')
    console.log('信息2')
console.groupEnd();
```

![1725781247171](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781247171.png)

## **输出代码运行时间**

`console.time()`和`console.timeEnd()` 可以显示代码的运行时间,下面代码输出这个for循环执行的时长

```
console.time('time')
  let a = 0
  for (let i = 0; i < 1000; i++) {
      a += i
  }
console.timeEnd('time')
```

![1725781383783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781383783.png)

## **输出ascii文本**

可以借助`https://lunicode.com/bigtext`这个网站生成ascii文本然后输出在控制台

```
var res = `
  _    _ _______ __  __ _      
 | |  | |__   __|  \/  | |     
 | |__| |  | |  | \  / | |     
 |  __  |  | |  | |\/| | |     
 | |  | |  | |  | |  | | |____ 
 |_|  |_|  |_|  |_|  |_|______|
                                   
    `      
   
console.log(res)
```

![1725781399273](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781399273.png)

输出图片ascii，例如输出vue的logo

```
var res = `
.....................................................
'!'!'!'!'!!!!!!!!!!!'...........'!!!!!!!!!!!!'!'!'!'!
!"ttttttttj$########$!.........!#########$Itttttttt"!
.!"ttttttttI$########$!.......!#########%Itttttttt"'.
..!"ttttttttI$#########!....."#########%Itttttttt"'..
...'"ttttttttI%#########"..-z#########3ttttttttt(....
.....+ttttttttt3#########u!%########$uttttttttt!.....
......!tttttttttJ$#################$jttttttttt!......
.......!tttttttttj$###############$Itttttttt"!.......
........!"ttttttttI$#############$Itttttttt"!........
.........!"ttttttttI$###########%Itttttttt"'.........
..........'"ttttttttI%#########3ttttttttt*-..........
...........-+ttttttttt3######$Jttttttttt!............
.............(ttttttttt3####$jttttttttt!.............
..............!tttttttttu$#$Itttttttt"!..............
...............!"ttttttttI%Itttttttt"!...............
................!"ttttttttttttttttt"'................
.................'"ttttttttttttttt"'.................
..................'"ttttttttttttt!...................
....................(ttttttttttt!....................
.....................!tttttttt"!.....................
......................!"ttttt"!......................
.......................!"ttt"!.......................
........................!"t*-........................
.........................'!..........................
.....................................................
`      
      
console.log(res)
```

![1725781420197](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781420197.png)

## **佛曰：永无bug**

以后写项目这就是我的第一行代码...哈哈

```
console.log(
   '                   _oo0oo_                     \n' +
           '                  o8888888o          \n' +
           '                  88" . "88          \n' +
           '                  (| -_- |)          \n' +
           '                   O\\ = /O          \n' +
           "               ____/`---'\\____         \n" +
           "             .   ' \\\\| |// `.         \n" +
           '              / \\\\||| : |||// \\        \n' +
           '           / _||||| -卍- |||||- \\        \n' +
           '              | | \\\\\\ - /// | |        \n' +
           "            | \\_| ''\\---/'' | |        \n" +
           '             \\ .-\\__ `-` ___/-. /       \n' +
           "          ___`. .' /--.--\\ `. . __       \n" +
           '       ."" "< `.___\\_<|>_/___. ` >" "".        \n' +
           '      | | : `- \\`.;`\\ _ /`;.`/ - ` : | |       \n' +
           '        \\ \\ `-. \\_ __\\ /__ _/ .-` / /          \n' +
           "======`-.____`-.___\\_____/___.-`____.-'======  \n" +
           "                   `=---='                     \n" +
           '.............................................  \n\t\t' +
           '佛祖镇楼                  BUG辟易                          \n\t' +
           '佛曰:\n\t\t' +
           '写字楼里写字间，写字间里程序员；\n\t\t' +
           '程序人员写程序，又拿程序换酒钱。\n\t\t' +
           '酒醒只在网上坐，酒醉还来网下眠；\n\t\t' +
           '酒醉酒醒日复日，网上网下年复年。\n\t\t' +
           '但愿老死电脑间，不愿鞠躬老板前；\n\t\t' +
           '奔驰宝马贵者趣，公交自行程序员。\n\t\t' +
           '别人笑我忒疯癫，我笑自己命太贱；\n\t\t' +
           '不见满街漂亮妹，哪个归得程序员？'
       )
```

![1725781446877](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725781446877.png)

