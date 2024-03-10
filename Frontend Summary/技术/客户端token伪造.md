# 客户端token伪造的最佳解决方案

`JWT`身份鉴权方案，token会作为主要的鉴权方式来作为前后端通信校验的凭证，当该token被篡改或者直接被第三方拿到，就可以伪造该用户做一系列业务操作，是一种非常严重的安全漏洞。

那，JWT是啥？如果你不知道，借助AI的力量，看看它的解释：

`JWT（JSON Web Tokens）`是一种开放标准（RFC 7519），用于在各方之间安全地传输信息作为JSON对象。由于其信息可以被验证和信任，JWT通常用于身份验证和信息交换。由于其较小的尺寸，它们特别适合于空间受限的环境，例如HTTP头部。

JWT主要由三个部分组成：

1. **Header**（头部）：Header通常由两部分组成，令牌的类型（即JWT）以及所使用的签名算法（如HMAC SHA256或RSA）。
2. **Payload**（负载）：其中包含所谓的Claims（声明），Claims是有关实体（通常是用户）和其他数据的声明。有三种类型的Claims：注册的声明、公共的声明和私有的声明。
3. **Signature**（签名）：为了获取这部分的签名，必须有编码过的header（头部）、编码过的payload（负载）、一个密钥，通过header中指定的算法进行签名。签名用于验证消息在传输途中没有被更改，并且，对于使用私有秘钥签署的token，还可以验证请求方是否为token的合法发布者。

`JWT`的使用流程一般如下：

1. 用户使用各种方法（如用户名和密码）向认证服务器进行身份验证。
2. 一旦服务器验证了用户的身份，它将生成一个JWT，并将其作为响应返回给用户。
3. 用户随后将此令牌用作每次向服务器请求资源时的凭据。通常，这个令牌放在HTTP请求的Authorization头部中，带上'Bearer'前缀。
4. 服务器会检验这个`JWT`的signature来确认其有效性，然后返回请求的数据。

`JWT`使用上述机制使得用户状态无需在服务器持久存储，从而更容易实现无状态、可扩展的应用，这在分布式微服务架构中特别有用。同时，JWT还提供了一种简单的方式，能够保证数据传输过程中的数据安全性和完整性。

![1709971876634](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971876634.png)image.png

介绍完`JWT`，那防止token伪造有没有什么好的方案？对于安全防御问题，核心策略就是`“提升破解难度”`，我们可以在客户端和服务端的通信中加入一个额外的约定签名，签名可以由当前时间戳信息、设备ID、日期、双方约定好的秘钥经过一些加密算法构造而成加密解密方式互相约定好，这样攻击者就算拿到了`Token`，还需要知道签名的生成规则和加密方式，才可以达成攻击，就像这样：

![1709971893093](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971893093.png)image.png

其实就相当于多了一层校验逻辑判断。

那接下来我们来实现一下吧。

我们先起个`Nest`项目，执行命令：

```
nest new jwt-project --strict
```

要做身份验证，那我们就准备好两个接口————`login`、`getUserInfo`

```
interface loginDTO {  access_token: string;  msg: string;}interface getUserInfoDTO {  name: string;  id: number;}@Controller()export class AppController {  constructor(    private readonly appService: AppService,  ) {}  @Post('login')  login(): loginDTO {    return this.appService.login();  }  @Post('getUserInfo')  getUserInfo(): getUserInfoDTO {    return this.appService.getUserInfo();  }}
```

启动项目：

```
npm run start
```

浏览器上访问一下`login`接口，项目已经跑起来了

![1709971909338](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971909338.png)image.png

接口准备好了，我们实现`login`的逻辑，数据库这里就不准备了，对于用户信息判断我们直接在`Nest`维护一个数组来存储，并且引入`Nest`的`JWT`服务。

![1709971925249](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971925249.png)image.png

我们提前准备一下，首先安装依赖包。

```
npm i @nestjs/jwt
```

在模块中引入：

![1709971942165](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971942165.png)image.png

这样`login`接口就会基于user的信息生成一个`JWT`字符串秘钥返回，调用一次接口试一下：

![1709971955783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971955783.png)image.png

有了`Token`就可以验完整的会话流程了，客户端通常我们会把`Token`存在本地存储，我们起一个客户端项目试一下，后端项目我们先开一下`cors`，支持跨域访问。

![1709971970481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971970481.png)image.png

我们起一个前端项目，用`umi.js`来作为主框架，执行命令：

```
npx create-umi@latest
```

找一个页面文件，写入调用`login`的代码：

![1709971984688](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971984688.png)image.png

为了方便测试，页面刷新我们就用调一次`login`，更新一次`Token`，并把它存在本地存储中，接下来我们准备一下业务接口。

![1709971998853](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709971998853.png)

对于`Token`做了为空和真实性两层校验，我们测试一下，在前端页面先把`Token`给删了，调一次：

![1709972014080](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972014080.png)image.png

![1709972046619](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972046619.png)image.png

请求头没有`authorization`字段，后端报了401，符合预期，我们再篡改一下`Token`试试：

![1709972065419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972065419.png)image.png

调一次getUserInfo，抛了`Nest`的`JWT`校验失败的异常：

![1709972078600](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972078600.png)image.png

OK，至此`JWT`的登录身份验证部分已经实现了，但现在有一个问题，我们的`Token`是暴露在浏览器的，如果我复制了这个`Token`，去postman调一下，是不是也可以调通？

测试一下。

![1709972091720](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972091720.png)image.png

毫无意外，翻车了。这也回到了文章标题，那如何解决这种情况呢？开头也提到了，加一层认证即可，在传`Token`的鉴权接口我们再多加一个`sign`字段，也放在`header`，这里的加解密我们用`crypto-js`，我们先实现下前端代码，首先安装依赖包。

```
npm i crypto-js --save-dev
```

然后在测试页面中引入，我们采用客户端加密`sign`、服务端解密`sign`的方案，因此我们实现个加密方法：

![1709972105597](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972105597.png)image.png

加密需要秘钥和秘钥偏移量，这两个参数前端和后端保持一致，这点非常重要。然后我们在`getUserInfo`接口中的请求头给它带上去。

![1709972126353](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972126353.png)image.png

加密的基准字符串采用调用时间、用户名、`Token`前6位组成，中间用冒号隔开，就像这样：

```
"1707449642869:jack:eyJhbG"
```

然后我们实现一下后端代码，同样也是先把依赖包安装，这是通用`JS`包，所以安装方式一样，然后在`Nest`中实现下解密验证的逻辑即可：

![1709972141945](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972141945.png)image.png

在后端我们加上解密的方法，同样基于密钥、密钥偏移量，与客户端一致，我们在业务接口中对于`Token`验证的基础上，再增加一层对于`sign`的验证，加强了接口安全，我们测试一下。

首先只传`Token`现在已经不行了：

![1709972156663](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972156663.png)image.png

后端默认使用第一个user来返回的，那前端我们传一个不一样的`username`作为`sign`，加上一层`sign`定制判断逻辑：

```
 if (userName !== 'aaa') {    throw new UnauthorizedException('用户信息错误');  }
```

再调一下，`header`都传上：

![1709972192419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972192419.png)image.png

命中了我们加上的判断逻辑，看一眼打印出来解密的`sign`日志：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972210012.png)image.png

打印出来的用户名是jack，不是aaa，当然校验失败了，这下我们的伪造解决方案实现了，我们可以把这一层逻辑全部集成在一个中间件上。

封装一下代码，新增**middlewares/auth.middleware.ts**

```
import {  Injectable,  NestMiddleware,  UnauthorizedException,} from '@nestjs/common';import { Request, Response, NextFunction } from 'express';import { JwtService } from '@nestjs/jwt';import * as CryptoJS from 'crypto-js';@Injectable()export class AuthMiddleware implements NestMiddleware {  constructor(private readonly jwtService: JwtService) {}  private readonly SECRET_KEY = CryptoJS.enc.Utf8.parse('3333e6e143439161'); //十六位十六进制数作为密钥  private readonly SECRET_IV = CryptoJS.enc.Utf8.parse('e3bbe7e3ba84431a'); //十六位十六进制数作为密钥偏移量  private decrypt(data: string): string {    //解密    const encryptedHexStr = CryptoJS.enc.Hex.parse(data);    const str = CryptoJS.enc.Base64.stringify(encryptedHexStr);    const decrypt = CryptoJS.AES.decrypt(str, this.SECRET_KEY, {      iv: this.SECRET_IV,      mode: CryptoJS.mode.CBC,      padding: CryptoJS.pad.Pkcs7,    });    const decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);    return decryptedStr.toString();  }  use(req: Request, res: Response, next: NextFunction) {    try {      const token = req.headers['authorization'];      const sign = req.headers['sign'];      if (!token) {        throw new UnauthorizedException('登录态失效，请重新登录');      }      if (!sign) {        throw new UnauthorizedException('身份验证失败');      }      //   token校验      this.jwtService.verify(token);      //   sign校验      const signRes = this.decrypt(sign as string);      const [time, userName, tokenStr] = signRes.split(':');      console.log('time:', time);      console.log('userName:', userName);      console.log('tokenStr:', tokenStr);      if (userName === '' || time === '' || tokenStr === '') {        throw new UnauthorizedException('身份验证失败');      }      if (userName !== 'aaa') {        throw new UnauthorizedException('用户信息错误');      }      return next();    } catch (error) {      throw new UnauthorizedException(        error.message || 'Unauthorized: Invalid token',      );    }  }}
```

然后在`Nest`中开启这个中间件，我们目前`login`接口不需要校验，所以只开`getUserInfo`接口。

![1709972232357](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709972232357.png)image.png

再跑一遍，没问题，结束。

我们简单总结一下。

`JWT`是目前主流的会话鉴权方案，但是不做安全方案的情况下，直接把`Token`暴露在请求体中会引发伪造身份请求的问题。

我们通过在请求中多一层加解密的约定式校验在通信中增加了网络请求的安全性。