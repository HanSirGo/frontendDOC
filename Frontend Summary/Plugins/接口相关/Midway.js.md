# Midway.js打通WebSocket前后端监听通道

# 前言

`WebSocket`协议允许客户端和服务端持久化连接，这种可以持续连接的特性使得`WebScoket`特别适用于游戏或者聊天室的场景，同样也适用于订单提交完以后监听状态变化进行页面刷新的场景。

在`Midway`中提供了对于`ws`模块的支持和封装，能够快速创建`WebSocket`服务。

# 服务端实现

我们采用`Midway.js`来实现服务端能力，首先创建一个项目：

```
npm init midway@latest -y
```

然后在项目中安装`WebSocket`的依赖包：

```
npm i @midwayjs/ws@3 --save
npm i @types/ws --save-dev
```

在`/src/configuration.ts`中开启`WebSocket`组件：

```
// src/configuration.ts
import { Configuration } from '@midwayjs/core';
import * as ws from '@midwayjs/ws';

@Configuration({
  imports: [ws],
  // ...
})
export class MainConfiguration {
  async onReady() {
        // ...
  }
}
```

然后我们开始写接口，首先在项目目录创建`socket`目录：

```
├── package.json
├── src
│   ├── configuration.ts          ## 入口配置文件
│   ├── interface.ts
│   └── socket                    ## ws 服务的文件
│       └── hello.controller.ts
├── test
├── bootstrap.js                  ## 服务启动入口
└── tsconfig.json
```

通过 `@WSController` 装饰器定义 WebSocket 服务：

```
import { WSController } from '@midwayjs/core';

@WSController()
export class HelloSocketController {
  // ...
}
```

当有客户端连接时，会触发 `connection` 事件，我们在代码中可以使用 `@OnWSConnection()` 装饰器来修饰一个方法，当每个客户端第一次连接服务时，将自动调用该方法。

```
import { WSController, OnWSConnection, Inject } from '@midwayjs/core';
import { Context } from '@midwayjs/ws';
import * as http from 'http';

@WSController()
export class HelloSocketController {

  @Inject()
  ctx: Context;

  @OnWSConnection()
  async onConnectionMethod(socket: Context, request: http.IncomingMessage) {
    console.log(`namespace / got a connection ${this.ctx.readyState}`);
  }
}
```

WebSocket 是通过事件的监听方式来获取数据。Midway 提供了 `@OnWSMessage()` 装饰器来格式化接收到的事件，每次客户端发送事件，被修饰的方法都将被执行。

```
import { WSController, OnWSMessage, Inject } from '@midwayjs/core';
import { Context } from '@midwayjs/ws';

@WSController()
export class HelloSocketController {

  @Inject()
  ctx: Context;

  @OnWSMessage('message')
  async gotMessage(data) {
    return { name: 'harry', result: parseInt(data) + 5 };
  }
}
```

我们可以通过 `@WSBroadCast` 装饰器将消息发送到所有连接的客户端上。

```
import { WSController, OnWSConnection, Inject } from '@midwayjs/core';
import { Context } from '@midwayjs/ws';

@WSController()
export class HelloSocketController {

  @Inject()
  ctx: Context;

  @OnWSMessage('message')
  @WSBroadCast()
  async gotMyMessage(data) {
    return { name: 'harry', result: parseInt(data) + 5 };
  }

  @OnWSDisConnection()
  async disconnect(id: number) {
    console.log('disconnect ' + id);
  }
}
```

至此，基础的`WebSocket`接口就开发好了。

- 当客户端连接时，我们会打印`namespace / got a connection`；
- 当客户端发送消息时，我们会返回给客户端基于入参的编号附加5的结果；
- 当客户端断开连接时，我们会打印`disconnect`；

最后我们在服务端开启一个`WebSocket`服务：

```
// src/config/config.default
export default {
  // ...
  webSocket: {
    port: 3000,
  },
}
```

接下来我们前端来调用测试下。

# 前端实现

前端使用`react`，我们建一个新页面，在`useEffect`中开启`ws`服务：

```
useEffect(async () => {
    const ws = new WebSocket(`ws://localhost:9999`);

    ws.onopen = () => {
      console.log('连接成功');
      ws.send(1);
    };
    ws.onmessage = (e) => {
      console.log('服务端响应：', e);
    };
    ws.onclose = (e) => {
      console.log('关闭连接，服务端响应：', e)
    }
  }, []);
```

这样我们在前端就可以打印出服务端返回的结果，同时服务端获取到了日志信息，前后端的`WebSocket`链路打通了。

前端：

![1710057399325](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710057399325.png)

开启了`webSocket`服务：

![1710057422154](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710057422154.png)

后端：

![1710057435179](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710057435179.png)

# 订单刷新

接下来我们模拟一个场景：当前端提交完订单后到了订单详情页，需要实时获取订单状态，如果订单申请通过，则展示成功信息，这里就不连接数据库了，直接在服务端维护一个对象来模拟：

```
import {
  WSController,
  Inject,
  OnWSConnection,
  OnWSMessage,
  OnWSDisConnection,
} from '@midwayjs/core';
import { Context } from '@midwayjs/ws';
import * as http from 'http';

const orderInfo = {
  status: 'pending',
  id: 1,
  // ...orderInfo
};

@WSController()
export class HelloSocketController {
  @Inject()
  ctx: Context;

  // 客户端第一次连接
  @OnWSConnection()
  async onConnectionMethod(socket: Context, request: http.IncomingMessage) {
    this.ctx.logger.info(`namespace / got a connection ${this.ctx.readyState}`);
    setTimeout(() => {
      orderInfo['status'] = 'success';
    }, 3000);
  }

  // 收到客户端消息
  @OnWSMessage('message')
  async gotMessage() {
    if (orderInfo.status === 'success') {
      return { result: true };
    }
    return { result: false };
  }

  // 客户端断开连接
  @OnWSDisConnection()
  async disconnect(id: number) {
    console.log('disconnect ' + id);
  }
}
```

- 在后端mock一个`orderInfo`；
- 当`WebSocket`连接后，延迟三秒改变`orderInfo`的状态，模拟一个算法审核的时间；
- 三秒后响应给前端新状态；

前端配合改造：

```
useEffect(async () => {
    const ws = new WebSocket(`ws://localhost:9999`);

    ws.onopen = () => {
      console.log('连接成功');
      timer.current = setInterval(() => {
        ws.send(1);
      }, 1000);
    };
    ws.onmessage = (e) => {
      console.log('服务端响应：', e);
      if (JSON.parse(e.data).result) {
        setOrderPass(true);
      }
    };
    ws.onclose = (e) => {
      console.log('关闭连接，服务端响应：', e);
    };
  }, []);

  return <>订单状态：{orderPass ? '通过' : '申请中'}</>;
```

改造后，前端页面刷新，三秒后订单状态异步更改为通过，达到了页面监听实时刷新的效果，这样的效果是不是比传统`Http定时轮询`性能更优呢？

![1710057502329](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710057502329.png)