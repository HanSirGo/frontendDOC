## Gulp

##### Gulp

```
npm i gulp -g
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258602805.png" alt="1709258602805" style="zoom: 67%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258609378.png" alt="1709258609378" style="zoom:50%;" />

```
直接 在终端里输入（gulp 任务名）。注意不要用DOM和BOM的API
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258632457.png" alt="1709258632457" style="zoom:50%;" />

```
gulp方法：src() dest() watch()
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258671238.png" alt="1709258671238" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258692981.png" alt="1709258692981" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258698700.png" alt="1709258698700" style="zoom:50%;" />

```
watch() ,需要先在终端调用watch的方法，保持监听的状态
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258739298.png" alt="1709258739298" style="zoom:50%;" />

##### gulp插件

```
安装插件：https://gulpjs.com/plugins/
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258780126.png" alt="1709258780126" style="zoom:50%;" />

```
gulp-connect:本地生成服务器环境
注意：每个监听的事件后面都要pipe(connect.reload())
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709258858348.png" alt="1709258858348" style="zoom:50%;" />

```
gulp插件推荐：
	压缩css:csso 
	压缩html:gulp-html-minify
	压缩图片：gulp-imagemin
	合并文件：gulp-concat
```

