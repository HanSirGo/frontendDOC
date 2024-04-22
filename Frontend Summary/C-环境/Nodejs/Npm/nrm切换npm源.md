## nrm

> nrm 是一个主要用于 Node.js 生态系统的命令行工具，它的全称为 "NPM Registry Manager"，即 NPM 注册源管理器。
>
> 试想一下：如果哪天你又跑去国外了，淘宝源肯定是用不了的，又要切换回官网源，或者哪天你们公司有自己的私有npm源了，又需要切换成公司的源，这样岂不很麻烦？
>
> 于是有了nrm。

### nrm作用

> 这个工具主要用来便捷地管理和切换不同的 NPM (Node Package Manager) 包注册源，这对于提高依赖包的下载速度非常有用，特别是对于那些在中国或其他国际网络访问受限地区的开发者。

### nrm源起

> 由于 NPM 官方默认的包注册源位于国外，直接从该源下载包可能速度较慢，许多开发者会选择使用如淘宝 NPM 镜像这样的国内镜像源。nrm 就是用来帮助用户在不同镜像源之间快速切换的工具。

### 使用 nrm

```js
#  安装 nrm：在命令行中运行以下命令来全局安装 nrm
$  npm install -g nrm

#  列出可用的 NPM 源：安装完成后，可以通过以下命令查看已知的 NPM 源列表：
$  nrm ls

#  使用 nrm 切换源：若要切换至某个特定的 NPM 源，例如切换至淘宝 NPM 镜像：
$  nrm use taobao

#  添加自定义源：如果你需要添加或移除自定义的 NPM 源，可以使用相应的 add 和 del 命令。
$  nrm del <registry>
$  nrm add <源名称> <源地址> // nrm add myregistry http://myregistry.com/

#  测试源的速度：可以通过以下命令测试各个源的下载速度：
$  nrm test

#  设置默认源：若要设置某个源为默认源，可以先切换至该源，nrm 会自动将其设为默认：
$  nrm default <registry-name>
```

[^registry]: 为源名

