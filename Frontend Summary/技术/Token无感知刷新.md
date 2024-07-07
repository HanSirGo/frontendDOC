## Token无感知刷新的

### 什么是JWT

`JWT`是全称是`JSON WEB TOKEN`，是一个开放标准，用于将各方数据信息作为JSON格式进行对象传递，可以对数据进行可选的数字加密，可使用`RSA`或`ECDSA`进行公钥/私钥签名。

JWT，全称为JSON Web Token，是一种用于在网络上传递信息的开放标准（RFC 7519）。它是一种紧凑且自包含的方式，用于在各方之间安全地传递信息。JWT通常用于身份验证和信息传递，特别是在Web应用程序中。

JWT由三部分组成：

1. Header（头部）： 包含了两部分信息，算法和token类型。通常包括两部分，alg表示签名算法，typ表示token类型。
2. Payload（负载）： 实际存储的信息数据。Payload包含了一些称为声明的属性，这些声明有一些预定义的字段，也可以包含自定义的字段。例如，包括用户的身份信息、权限等。
3. Signature（签名）： 使用头部指定的签名算法对头部、负载和一个密钥进行签名。签名的作用是验证发送者的身份以及确保在传输过程中没有被篡改。

JWT的结构如下：

```js
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

这是一个JWT的例子，由三部分组成，通过点号（.）分隔。具体而言，第一部分是Base64编码的头部，第二部分是Base64编码的负载，第三部分是使用头部中指定的算法签名后的结果。整个JWT可以在网络上传输，接收方可以使用相同的密钥和算法验证签名并解析出其中的信息。

### 使用场景

`JWT`最常见的使用场景就是缓存当前用户登录信息，当用户登录成功之后，拿到`JWT`，之后用户的每一个请求在请求头携带上`Authorization`字段来辨别区分请求的用户信息。且不需要额外的资源开销。

JWT（JSON Web Token）主要用于在网络中安全地传递信息。以下是一些常见的使用场景：

1. **身份验证（Authentication）：** JWT被广泛用于用户身份验证。当用户登录成功后，服务器可以生成一个JWT作为身份令牌，并将其发送给客户端。客户端在后续请求中通过在请求头中附带JWT来证明其身份。服务器验证JWT的签名后，可以信任其中包含的用户身份信息。
2. **授权（Authorization）：** JWT可以包含有关用户的角色和权限信息。在进行授权决策时，服务器可以解析JWT中的信息，以确定用户是否有权执行特定操作。
3. **信息交换（Information Exchange）：** 由于JWT是一种轻量且自包含的数据格式，它在不同系统之间安全地传递信息变得很方便。这对于在微服务架构中的不同服务之间传递身份信息、权限信息等非常有用。
4. **单点登录（Single Sign-On，SSO）：** JWT被广泛用于实现单点登录系统，允许用户在一次身份验证后访问多个关联的系统而无需重新输入凭据。用户一旦登录成功，服务器颁发一个JWT，该JWT可以在多个服务之间传递并被验证，从而实现单点登录效果。
5. **重置密码和安全令牌（Reset Password and Security Tokens）：** JWT可以用于生成包含安全性信息的令牌，例如用于重置密码或执行其他敏感操作时的令牌。这些令牌可以包含有限期，以确保它们在一段时间后失效。
6. **移动应用开发（Mobile App Development）：** 在移动应用程序中，JWT可以用于安全地传递用户信息和授权令牌，以确保应用程序和后端之间的通信是安全的。

### 相比传统session的区别

比起传统的`session`认证方案，为了让服务器能识别是哪一个用户发过来的请求，都需要在服务器上保存一份用户的登录信息（通常保存在内存中），再与浏览器的`cookie`打交道。

- 安全方面 由于是使用`cookie`来识别用户信息的，如果`cookie`被拦截，用户会很容易受到跨站请求伪造的攻击。
- 负载均衡 当服务器A保存了用户A的数据之后，在下一次用户A服务器A时由于服务器A访问量较大，被转发到服务器B，此时服务器B没有用户A的数据，会导致`session`失效。
- 内存开销 随着时间推移，用户的增长，服务器需要保存的用户登录信息也就越来越多的，会导致服务器开销越来越大。

**区别和优势：**

1. **状态存储的位置：**

2. - **JWT：** 无状态，所有的信息都包含在JWT本身中，因此服务器不需要在后端存储用户的会话信息。每个请求都是独立的，服务器不需要在内存中维护用户的状态。
   - **传统Session：** 使用会话机制通常需要在服务器端存储用户的会话信息，这可能导致服务器维护会话状态的开销。

3. **扩展性：**

4. - **JWT：** 因为JWT是自包含的，可以轻松在不同的服务之间传递，适用于微服务架构。它支持跨域和分布式环境，因为不依赖服务器端存储。
   - **传统Session：** 在分布式环境下，需要考虑会话共享和同步的问题，可能需要使用一些额外的工具或技术。

5. **跨域通信：**

6. - **JWT：** 跨域通信更容易，因为JWT可以通过HTTP头部传递，无需考虑跨域问题。
   - **传统Session：** 在跨域情况下，可能需要使用一些额外的配置或技术来处理会话共享和身份验证问题。

7. **无需服务器端存储：**

8. - **JWT：** 无需在服务器端存储会话信息，减轻了服务器的负担，特别适用于大规模分布式系统。
   - **传统Session：** 服务器通常需要存储会话信息，这可能会导致服务器负担增加，特别是在用户规模庞大的情况下。

9. **前后端分离：**

10. - **JWT：** 支持前后端分离的架构，因为JWT可以由前端存储并在每个请求中发送。
    - **传统Session：** 通常与后端紧耦合，需要在前端和后端之间共享会话信息。

11. **性能：**

12. - **JWT：** 由于无需在服务器端存储会话信息，可以降低服务器负载，提高性能。
    - **传统Session：** 需要在服务器端存储和管理会话信息，可能导致性能开销。

选择使用JWT还是传统Session取决于具体的应用场景和需求。JWT更适合无状态和分布式环境，而传统Session则可能更适用于一些特定的应用场景。

### 为什么说JWT不需要额外的开销

`JWT`为三个部分组成，分别是`Header`，`Payload`，`Signature`，使用`.`符号分隔。

```js
// 像这样子
xxxxx.yyyyy.zzzzz
```

#### 标头 header

标头是一个`JSON`对象，由两个部分组成，分别是令牌是类型（`JWT`）和签名算法（`SHA256`，`RSA`）

```js
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### 负荷 payload

负荷部分也是一个`JSON`对象，用于存放需要传递的数据，例如用户的信息

```js
{
  "username": "_island",
  "age": 18
}
```

此外，JWT规定了7个可选官方字段（建议）

| 属性 |    说明     |
| :--: | :---------: |
| iss  |  JWT签发人  |
| exp  | JWT过期时间 |
| sub  | JWT面向用户 |
| aud  |  JWT接收方  |
| nbf  | JWT生效时间 |
| iat  | JWT签发时间 |
| jti  |   JWT编号   |

#### 签章 signature

这一部分，是由前面两个部分的签名，防止数据被篡改。在服务器中指定一个密钥，使用标头中指定的签名算法，按照下面的公式生成这签名数据

```js
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

在拿到签名数据之后，把这三个部分的数据拼接起来，每个部分中间使用`.`来分隔。这样子我们就生成出一个了`JWT`数据了，接下来返回给客户端储存起来。而且客户端在发起请求时，携带这个`JWT`在请求头中的`Authorization`字段，服务器通过解密的方式即可识别出对应的用户信息。

### JWT优势和弊端

#### 优势

- 数据体积小，传输速度快
- 无需额外资源开销来存放数据
- 支持跨域验证使用

#### 弊端

- 生成出来的`Token`无法撤销，即使重置账号密码之前的`Token`也是可以使用的（需等待JWT过期）
- 无法确认用户已经签发了多少个`JWT`
- 不支持`refreshToken`

### 关于refreshToken

`refreshToken`是`Oauth2`认证中的一个概念，和`accessToken`一起生成出来的。

当用户携带的这个`accessToken`过期时，用户就需要在重新获取新的`accessToken`，而`refreshToken`就用来重新获取新的`accessToken`的凭证。

**如何工作：**

1. **获取访问令牌：** 用户首次通过身份验证流程获取访问令牌。这个访问令牌是用于访问受保护资源的令牌。
2. **获取刷新令牌：** 在用户登录成功后，除了访问令牌之外，通常还会颁发一个刷新令牌。刷新令牌通常具有更长的有效期。
3. **使用访问令牌：** 客户端使用访问令牌来访问受保护资源。访问令牌在每次请求中被附加到请求头中。
4. **刷新访问令牌：** 当访问令牌即将过期或已过期时，客户端可以使用刷新令牌来请求一个新的访问令牌，而无需用户再次提供用户名和密码。

**优势：**

1. **减少用户重新登录次数：** 通过使用刷新令牌，可以减少用户需要重新进行身份验证的频率，提高用户体验。
2. **更安全的令牌管理：** 因为刷新令牌通常具有较长的有效期，而访问令牌的有效期相对较短，即使访问令牌被泄露，攻击者也只能使用它的短时间内。
3. **简化令牌的使用：** 客户端在使用刷新令牌时，可以通过后台进行令牌的刷新，而不需要用户的直接参与，使得整个流程更加简化。

**安全注意事项：**

1. **刷新令牌的保护：** 刷新令牌应该像访问令牌一样受到保护，以防止泄漏。
2. **刷新令牌的有效期：** 刷新令牌的有效期应该足够长，以确保用户不会频繁被要求重新登录，但又不至于过长以致于安全性受到威胁。
3. **撤销令牌的机制：** 考虑实现撤销机制，以便在用户注销或令牌被泄露时及时撤销刷新令牌。
4. **令牌刷新的安全性：** 令牌刷新请求和响应的通信应该是安全的，最好采用HTTPS协议。

### 为什么要有refreshToken

当你第一次接触的时候，你有没有一个这样子的疑惑，为什么需要`refreshToken`这个东西，而不是服务器端给一个期限较长甚至永久性的`accessToken`呢？

抱着这个疑惑我在网上搜寻了一番，

其实这个`accessToken`的使用期限有点像我们生活中的入住酒店，当我们在入住酒店时，会出示我们的身份证明来登记获取房卡，此时房卡相当于`accessToken`，可以访问对应的房间，当你的房卡过期之后就无法再开启房门了，此时就需要再到前台更新一下房卡，才能正常进入，这个过程也就相当于`refreshToken`。

`accessToken`使用率相比`refreshToken`频繁很多，如果按上面所说如果`accessToken`给定一个较长的有效时间，就会出现不可控的权限泄露风险。

### 使用refreshToken可以提高安全性

- 用户在访问网站时，`accessToken`被盗取了，此时攻击者就可以拿这个`accessToke`访问权限以内的功能了。如果`accessToken`设置一个短暂的有效期2小时，攻击者能使用被盗取的`accessToken`的时间最多也就2个小时，除非再通过`refreshToken`刷新`accessToken`才能正常访问。
- 设置`accessToken`有效期是永久的，用户在更改密码之后，之前的`accessToken`也是有效的

总体来说有了`refreshToken`可以降低`accessToken`被盗的风险

### 关于JWT无感刷新TOKEN方案（结合axios）

### 业务需求

在用户登录应用后，服务器会返回一组数据，其中就包含了`accessToken`和`refreshToken`，每个`accessToken`都有一个固定的有效期，如果携带一个过期的`token`向服务器请求时，服务器会返回401的状态码来告诉用户此`token`过期了，此时就需要用到登录时返回的`refreshToken`调用刷新`Token`的接口（`Refresh`）来更新下新的`token`再发送请求即可。

### 工具

`axios`作为最热门的`http`请求库之一，我们本篇文章就借助它的错误响应拦截器来实现`token`无感刷新功能。

### 具体实现

利用`interceptors.response`，在业务代码获取到接口数据之前进行状态码`401`判断当前携带的`accessToken`是否失效。下面是关于`interceptors.response`中异常阶段处理内容。当响应码为401时，响应拦截器会走中第二个回调函数`onRejected`

```js
// 最大重发次数
const MAX_ERROR_COUNT = 5;
// 当前重发次数
let currentCount = 0;
// 缓存请求队列
const queue: ((t: string) => any)[] = [];
// 当前是否刷新状态
let isRefresh = false;

export default async (error: AxiosError<ResponseDataType>) => {
  const statusCode = error.response?.status;
  const clearAuth = () => {
    console.log('身份过期，请重新登录');
    window.location.replace('/login');
    // 清空数据
    sessionStorage.clear();
    return Promise.reject(error);
  };
  // 为了节省多余的代码，这里仅展示处理状态码为401的情况
  if (statusCode === 401) {
    // accessToken失效
    // 判断本地是否有缓存有refreshToken
    const refreshToken = sessionStorage.get('refresh') ?? null;
    if (!refreshToken) {
      clearAuth();
    }
    // 提取请求的配置
    const { config } = error;
    // 判断是否refresh失败且状态码401，再次进入错误拦截器
    if (config.url?.includes('refresh')) {
    clearAuth();
    }
    // 判断当前是否为刷新状态中（防止多个请求导致多次调refresh接口）
    if (!isRefresh) {
      // 设置当前状态为刷新中
      isRefresh = true;
      // 如果重发次数超过，直接退出登录
      if (currentCount > MAX_ERROR_COUNT) {
        clearAuth();
      }
      // 增加重试次数
      currentCount += 1;

      try {
        const {
          data: { access },
        } = await UserAuthApi.refreshToken(refreshToken);
        // 请求成功，缓存新的accessToken
        sessionStorage.set('token', access);
        // 重置重发次数
        currentCount = 0;
        // 遍历队列，重新发起请求
        queue.forEach((cb) => cb(access));
        // 返回请求数据
        return ApiInstance.request(error.config);
      } catch {
        // 刷新token失败，直接退出登录
        console.log('请重新登录');
        sessionStorage.clear();
        window.location.replace('/login');
        return Promise.reject(error);
      } finally {
        // 重置状态
        isRefresh = false;
      }
    } else {
      // 当前正在尝试刷新token，先返回一个promise阻塞请求并推进请求列表中
      return new Promise((resolve) => {
        // 缓存网络请求，等token刷新后直接执行
        queue.push((newToken: string) => {
          Reflect.set(config.headers!, 'authorization', newToken);
          // @ts-ignore
          resolve(ApiInstance.request<ResponseDataType<any>>(config));
        });
      });
    }
  }

  return Promise.reject(error);
};
```

### 抽离代码

把上面关于调用刷新`token`的代码抽离成一个`refreshToken`函数，单独处理这一情况，这样子做有利于提高代码的可读性和维护性，且让看上去代码不是很臃肿

```js
// refreshToken.ts
export default async function refreshToken(error: AxiosError<ResponseDataType>) {
    /* 
    将上面 if (statusCode === 401) 中的代码贴进来即可，这里就不重复啦
    代码仓库地址: https://github.com/QC2168/axios-bz/blob/main/Interceptors/hooks/refreshToken.ts
    */
}
```

经过上面的逻辑抽离，现在看下拦截器中的代码就很简洁了，后续如果要调整相关逻辑直接在`refreshToken.ts`文件中调整即可。

```js
import refreshToken from './refreshToken.ts'
export default async (error: AxiosError<ResponseDataType>) => {
  const statusCode = error.response?.status;

  // 为了节省多余的代码，这里仅展示处理状态码为401的情况
  if (statusCode === 401) {
    refreshToken()
  }

  return Promise.reject(error);
};
```

## 实现无感刷新 token-2

### 环境

1. 请求采用的 Axios V1.3.2。
2. 平台的采用的 `JWT(JSON Web Tokens)` 进行用户登录鉴权。
   **「（拓展：JWT 是一种认证机制，让后台知道该请求是来自于受信的客户端；更详细的可以自行查询相关资料）」**

### 问题现象

线上用户在使用的时候，偶尔会出现突然跳转到登录页面，需要重新登录的现象。

### 原因

1. 突然跳转到登录页面，是由于当前的 token 过期，导致请求失败；在 `axios` 的响应拦截`axiosInstance.interceptors.response.use`中处理失败请求返回的状态码 401，此时得知`token`失效，因此跳转到登录页面，让用户重新进行登录。
2. 平台目前的逻辑是在 `token` 未过期内，用户登录平台可直接进入首页，无需进行登录操作；因此就存在该现象：用户打开平台，由于此时 `token` 未过期，用户直接进入到了首页，进行其他操作。但是在用户操作的过程中，`token` 突然失效了，此时就会出现突然跳转到登录页面，严重影响用户的体验感！
   （**「注：目前线上项目中存在数据大屏，一些实时数据的显示；因此存在用户长时间停留在大屏页面，不进行操作，查看实时数据的情况」**）

### 切入点

1. 怎样及时的、在用户感知不到的情况下更新`token`？
2. 当 `token` 失效的情况下，出错的请求可能不仅只有一个；当失效的 `token` 更新后，怎样将多个失败的请求，重新发送？

### 操作流程

好了！经过了一番分析后，我们找到了问题的所在，并且确定了切入点；那么接下来让我们实操，将问题解决掉。
**「前要：」**
1、我们仅从前端的角度去处理。
2、后端提供了两个重要的参数：`accessToken`（用于请求头中，进行鉴权，存在有效期）；`refreshToken`（刷新令牌，用于更新过期的 accessToken，相对于 accessToken 而言，它的有效期更长）。

#### 1、处理 axios 响应拦截

**「注：在我实际的项目中，accessToken 过期后端返回的 statusCode 值为 401，需要在axiosInstance.interceptors.response.use 的 error回调中进行逻辑处理。」**

```js
// 响应拦截
axiosInstance.interceptors.response.use(
  (response) => {
    return response;
  },
  (error) => {
    let {
      data, config
    } = error.response;
    return new Promise((resolve, reject) => {
      /**
       * 判断当前请求失败
       * 是否由 toekn 失效导致的
       */
      if (data.statusCode === 401) {
         /**
         * refreshToken 为封装的有关更新 token 的相关操作 
         */
        refreshToken(() => {
          resolve(axiosInstance(config));
        });
      } else {
        reject(error.response);
      }
    })
  }
)
```

1. 我们通过判断`statusCode`来确定，是否当前请求失败是由`token`过期导致的；
2. 使用 Promise 处理将失败的请求，将由于 `token` 过期导致的失败请求存储起来（存储的是请求回调函数，resolve 状态）。**「理由：后续我们更新了 token 后，可以将存储的失败请求重新发起，以此来达到用户无感的体验」**

##### **「补充：」**

**「现象」**：在我过了几天登录平台的时候发现，`refreshToken`过期了，但是没有跳转到登录界面**「原因」**：
1、当refreshToken过期失效后，后端返回的状态码也是 `401`
2、发起的更新`token`的请求采用的也是处理后的`axios`，因此**「响应失败的拦截，对更新请求同样适用」**。
**「问题：」**
这样会造成，当refreshToken过期后，会出现停留在首页，无法跳转到登录页面。
**「解决方法」**
针对这种现象，我们需要完善一下`axios`中响应拦截的逻辑。

```js
axiosInstance.interceptors.response.use(
  (response) => {
    return response;
  },
  (error) => {
    let {
      data, config
    } = error.response;
    return new Promise((resolve, reject) => {
      /**
       * 判断当前请求失败
       * 是否由 toekn 失效导致的
       */
      if (
        data.statusCode === 401 &&
        config.url !== '/api/token/refreshToken'
      ) {
        refreshToken(() => {
          resolve(axiosInstance(config));
        });
      } else if (
        data.statusCode === 401 &&
        config.url === '/api/token/refreshToken'
      ) {
        /**
         * 后端 更新 refreshToken 失效后
         * 返回的状态码， 401
         */
        window.location.href = `${HOME_PAGE}/login`;
      } else {
        reject(error.response);
      }
    })
  }
)
```

#### 2、封装 refreshToken 逻辑

**「要点：」**

1. 存储由于`token`过期导致的失败的请求。
2. 更新本地以及axios中头部的`token`。
3. 当 `refreshToken` 刷新令牌也过期后，让用户重新登录。

```js
// 存储由于 token 过期导致 失败的请求
let expiredRequestArr: any[] = [];

/**
 * 存储当前因为 token 失效导致发送失败的请求
 */
const saveErrorRequest = (expiredRequest: () => any) => {
  expiredRequestArr.push(expiredRequest);
}

// 避免频繁发送更新 
let firstRequre = true;
/**
 * 利用 refreshToken 更新当前使用的 token
 */
const updateTokenByRefreshToken = () => {
  firstRequre = false;
  axiosInstance.post(
    '更新 token 的请求',
  ).then(res => {
    let {
      refreshToken, accessToken
    } = res.data;
    // 更新本地的token
    localStorage.setItem('accessToken', accessToken);
    // 更新请求头中的 token
    setAxiosHeader(accessToken);
    localStorage.setItem('refreshToken', refreshToken);

    /**
     * 当获取了最新的 refreshToken, accessToken 后
     * 重新发起之前失败的请求
     */
    expiredRequestArr.forEach(request => {
      request();
    })
    expiredRequestArr = [];
  }).catch(err => {
    console.log('刷新 token 失败err', err);
    /**
     * 此时 refreshToken 也已经失效了
     * 返回登录页，让用户重新进行登录操作
     */
    window.location.href = `${HOME_PAGE}/login`;
  })
}

/**
 * 更新当前已过期的 token
 * @param expiredRequest 回调函数，返回由token过期导致失败的请求
 */
export const refreshToken = (expiredRequest: () => any) => {
  saveErrorRequest(expiredRequest);
  if (firstRequre) {
    updateTokenByRefreshToken();
  }
}
```

##### **「补充：」**

**「问题：」**
1、怎么能保证当更新token后，在处理存储的过期请求时，此时没有过期请求还在存呢？；万一此时还在`expiredRequestArr`推失败的请求呢？
**「解决方法」**我们需要调整一下更新 token的逻辑，确保当前由于过期失败的请求都接收到了，再更新token然后重新发起请求。

### **「最终结果：」**

```js
// refreshToken.ts

/**
 * 功能：
 *  用于实现无感刷新 token
 */
import { axiosInstance, setAxiosHeader } from "@/axios"
import { CLIENT_ID, HOME_PAGE } from "@/systemInfo"

// 存储由于 token 过期导致 失败的请求
let expiredRequestArr: any[] = [];

/**
 * 存储当前因为 token 失效导致发送失败的请求
 */
const saveErrorRequest = (expiredRequest: () => any) => {
  expiredRequestArr.push(expiredRequest);
}

/**
 * 执行当前存储的由于过期导致失败的请求
 */
const againRequest = () => {
  expiredRequestArr.forEach(request => {
    request();
  })
  clearExpiredRequest();
}

/**
 * 清空当前存储的过期请求
 */
export const clearExpiredRequest = () => {
  expiredRequestArr = [];
}

/**
 * 利用 refreshToken 更新当前使用的 token
 */
const updateTokenByRefreshToken = () => {
  axiosInstance.post(
    '更新请求url',
    {
      clientId: CLIENT_ID,
      userName: localStorage.getItem('userName')
    },
    {
      headers: {
        'Content-Type': 'application/json;charset=utf-8',
        'Authorization': 'bearer ' + localStorage.getItem("refreshToken")
      }
    }
  ).then(res => {
    let {
      refreshToken, accessToken
    } = res.data;
    // 更新本地的token
    localStorage.setItem('accessToken', accessToken);
    localStorage.setItem('refreshToken', refreshToken);
    setAxiosHeader(accessToken);
    /**
     * 当获取了最新的 refreshToken, accessToken 后
     * 重新发起之前失败的请求
     */
    againRequest();
  }).catch(err => {
    /**
     * 此时 refreshToken 也已经失效了
     * 返回登录页，让用户重新进行登录操作
     */
    window.location.href = `${HOME_PAGE}/login`;
  })
}

let timer: any = null;
/**
 * 更新当前已过期的 token
 * @param expiredRequest 回调函数，返回过期的请求
 */
export const refreshToken = (expiredRequest: () => any) => {
  saveErrorRequest(expiredRequest);
  // 保证再发起更新时，已经没有了过期请求要进行存储
  if (timer) clearTimeout(timer);
  timer = setTimeout(() => {
    updateTokenByRefreshToken();
  }, 500);
}
```

```js
// 响应拦截 区分登录前
axiosInstance.interceptors.response.use(
  (response) => {
    return response;
  },
  (error) => {
    let {
      data, config
    } = error.response;
    return new Promise((resolve, reject) => {
      /**
       * 判断当前请求失败
       * 是否由 toekn 失效导致的
       */
      if (
        data.statusCode === 401 &&
        config.url !== '/api/token/refreshToken'
      ) {
        refreshToken(() => {
          resolve(axiosInstance(config));
        });
      } else if (
        data.statusCode === 401 &&
        config.url === '/api/token/refreshToken'
      ) {
        /**
         * 后端 更新 refreshToken 失效后
         * 返回的状态码， 401
         */
        clearExpiredRequest();
        window.location.href = `${HOME_PAGE}/login`;
      } else {
        reject(error.response);
      }
    })
  }
)
```

