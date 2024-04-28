

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712993570995.png" alt="1712993570995" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711176751527.png" alt="1711176751527" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711177582855.png" alt="1711177582855" style="zoom: 50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711177582855.png" alt="1711177582855" style="zoom: 50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710675884447.png" alt="1710675884447" style="zoom:50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711187276776.png" alt="1711187276776" style="zoom: 50%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714197460600.png" alt="1714197460600" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711195137146.png" alt="1711195137146" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714286311960.png" alt="1714286311960" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1711271696330.png" alt="1711271696330" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713591765137.png" alt="1713591765137" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714193803400.png" alt="1714193803400" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712482918405.png" alt="1712482918405" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714193920309.png" alt="1714193920309" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713591432457.png" alt="1713591432457" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714282011321.png" alt="1714282011321" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714284931996.png" alt="1714284931996" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713685573992.png" alt="1713685573992" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714197574455.png" alt="1714197574455" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713691998511.png" alt="1713691998511" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714283083891.png" alt="1714283083891" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714197976117.png" alt="1714197976117" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714217133080.png" alt="1714217133080" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714286699107.png" alt="1714286699107" style="zoom:50%;" />

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1714287948413.png" alt="1714287948413" style="zoom:50%;" />

------



- 《如何搭建Monorepo环境》
- 《组件库的环境配置》
- 《如何开发一个组件》
- 《集成ESLint+Prettier+Stylelint+Husky规范代码》
- 《如何使用Vite打包组件库》
- 《前端流程化控制工具gulp的使用》
- 《引入单元测试框架Vitest》
- 《使用 glup 打包组件库并实现按需加载》
- 《自动化发布、管理版本号》
- 《VitePress搭建组件库文档站点》
- 《搭建一个完整前端脚手架》



**工具约束代码规范**

除了约定俗称的规范，我们也需要借助一些工具和插件来协助我们更好的完成规范这件事情。

**代码规范**

1. **vscode****[1]**：统一前端编辑器。
2. **editorconfig****[2]**: 统一团队**vscode**编辑器默认配置。
3. **prettier****[3]**: 保存文件自动格式化代码。
4. **eslint****[4]**: 检测代码语法规范和错误。
5. **stylelint****[5]**: 检测和格式化样式文件语法

可以看我这篇文章：**【前端工程化】配置React+ts企业级代码规范及样式格式和git提交规范****[6]**

**git提交规范**

1. **husky****[7]**:可以监听**githooks****[8]**执行，在对应**hook**执行阶段做一些处理的操作。
2. **lint-staged****[9]**: 只检测暂存区文件代码，优化**eslint**检测速度。
3. **pre-commit****[10]**：**githooks**之一， 在**commit**提交前使用**tsc**和**eslint**对语法进行检测。
4. **commit-msg****[11]**：**githooks**之一，在**commit**提交前对**commit**备注信息进行检测。
5. **commitlint****[12]**：在**githooks**的**pre-commit**阶段对**commit**备注信息进行检测。
6. **commitizen****[13]**：**git**的规范化提交工具，辅助填写**commit**信息。

可以看我这篇文章：**【前端工程化】配置React+ts企业级代码规范及样式格式和git提交规范****[14]**



##  搭建npm私服

公司前端项目不推荐使用太多第三方包，可以自己搭建公司**npm**私服，来托管公司自己封装的状态管理库，请求库，组件库，以及脚手架**cli**，**sdk**等**npm**包，方便复用和管理。

可以看我这两篇文章，都可以搭建**npm**私服：

**【前端工程化】巧用阿里云oss服务打造前端npm私有仓库****[20]**

**【前端工程化】使用verdaccio搭建公司npm私有库完整流程和踩坑记录****[21]**



**【前端工程化】从入门到精通，100行代码构建你的前端CLI脚手架之路****[22]**



 **GitHub Actions**[17] - 自动部署



**CSS time (codepen.io)**

