> 在前端项目，文件目录中存在.vscode文件夹，文件夹下一般存在两个文件extensions.json和setting.json。作用是保持所有开发者安装了相同的插件和相同的配置，**保持开发环境一致性**。
> ![1713771662033](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713771662033.png)

### 在初始化项目中添加独有vscode的配置
#### settings.json
> 想在项目中添加vscode的配置，例如：自动保存，保存时格式化，缩进的方式是 tab 缩进2个空格等等...


> 1. 使用vscode打开项目
> 2. 在目录区域，右键，打开文件夹设置，选择 工作区
> 3. 修改一些设置，这样就可以在项目的目录中生成 .vscode 文件夹，修改的设置在 settings.json中

![1713771676100](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713771676100.png)
> 注意：项目中的setting.json会覆盖vscode中的全局配置。
#### extensions.json
**安装推荐插件**
在当前项目中，需要安装哪些插件。
```javascript
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "Vue.volar"
  ]
}
```
查看当前项目（工作区）推荐插件，步骤：
![1713771689936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713771689936.png)
![1713771704283](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1713771704283.png)
### 总结
extensions.json和setting.json对于团队协作开发起到很重要的作用