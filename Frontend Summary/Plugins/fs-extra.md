### fs-extra
**fs-extra是fs的一个扩展，提供了非常多的便利API，并且继承了fs所有方法和为fs方法添加了promise的支持。**

**它应该是 fs 的替代品。**
#### 安装
```javascript
npm install fs-extra -S
```
#### 用法
**fs-extra代替fs使用，所有fs方法都附在fs-extra，fs如果未传递回调，则所有方法都将返回promise用法**

```javascript

const fs = require('fs-extra')
 
// 异步方法，返回promise
fs.copy('/tmp/myfile', '/tmp/mynewfile')
  .then(() => console.log('success!'))
  .catch(err => console.error(err))
 
// 异步方法，回调函数
fs.copy('/tmp/myfile', '/tmp/mynewfile', err => {
  if (err) return console.error(err)
  console.log('success!')
})
 
// 同步方法，注意必须使用try catch包裹着才能捕获错误
try {
  fs.copySync('/tmp/myfile', '/tmp/mynewfile')
  console.log('success!')
} catch (err) {
  console.error(err)
}
 
// Async/Await:
async function copyFiles () {
  try {
    await fs.copy('/tmp/myfile', '/tmp/mynewfile')
    console.log('success!')
  } catch (err) {
    console.error(err)
  }
}
 
copyFiles()

```
#### API Methods
下面的所有方法都是fs-extra扩展方法
##### async
>  - copy 
>  - emptyDir 
>  - ensureFile 
>  - ensureDir 
>  - ensureLink 
>  - ensureSymlink 
>  - mkdirp
>  - mkdirs 
>  - move 
>  - outputFile 
>  - outputJson 
>  - pathExists 
>  - readJson 
>  - remove
>  - writeJson

##### sync
>  - copySync
>  - emptyDirSync 
>  - ensureFileSync 
>  - ensureDirSync 
>  - ensureLinkSync
>  - ensureSymlinkSync 
>  - mkdirpSync 
>  - mkdirsSync 
>  - moveSync 
>  - outputFileSync
>  - outputJsonSync 
>  - pathExistsSync 
>  - readJsonSync 
>  - removeSync 
>  - writeJsonSync