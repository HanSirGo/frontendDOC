### vue项目中Object.fromEntries方法报错

浏览器版本问题导致Object.fromEntries is not a function， fromEntries是es10提出来的方法polyfill和babel都不转换这个方法

解决方法：polyfill-object.fromentries

1、npm i polyfill-object.fromentries

2、main.js中import 'polyfill-object.fromentries';

![1715510289267](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1715510289267.png)

