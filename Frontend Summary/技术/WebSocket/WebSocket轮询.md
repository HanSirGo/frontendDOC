# 封装WebSocket消息推送，干翻Ajax轮询方式

使用AJAX和WebSocket都可以实现消息推送，但它们在实现方式和适用场景上有所不同。下面是使用这两种技术实现消息推送的简要说明。

# **AJax实现或WebSocket实现对比**

## **AJAX 实现消息推送**

AJAX（Asynchronous JavaScript and XML）允许你在不重新加载整个页面的情况下，与服务器进行数据交换。但是，传统的AJAX并不直接支持实时消息推送，因为它基于请求-响应模式。为了模拟消息推送，你可以使用轮询（polling）或长轮询（long-polling）技术。

### **轮询（Polling）**

轮询是定期向服务器发送请求，以检查是否有新的消息。这种方法简单但效率较低，因为即使在没有新消息的情况下，也会频繁地发送请求。

```
function pollForMessages() {
    $.ajax({
        url: '/messages', // 假设这是获取消息的API端点
        method: 'GET',
        success: function(data) {
            // 处理接收到的消息
            console.log(data);
            
            // 等待一段时间后再次轮询
            setTimeout(pollForMessages, 5000); // 每5秒轮询一次
        },
        error: function() {
            // 处理请求失败的情况
            setTimeout(pollForMessages, 10000); // 等待更长时间后重试
        }
    });
}

// 开始轮询
pollForMessages();
```

### **长轮询（Long-Polling）**

长轮询是轮询的一种改进方式。客户端发起一个请求到服务器，服务器会保持这个连接打开直到有新消息到达或超时，然后返回新消息或超时响应。这种方式比简单轮询减少了无效的请求，但仍然存在一定的延迟和资源浪费。

使用长轮询时，通常需要在服务器端有特殊的支持来保持连接直到有数据可以发送。

## **WebSocket 实现消息推送**

WebSocket 提供了一个全双工的通信通道，允许服务器主动向客户端推送消息。一旦建立了WebSocket连接，服务器和客户端就可以随时向对方发送消息，而不需要像AJAX那样频繁地发起请求。

### **WebSocket 客户端实现**

```
var socket = new WebSocket('ws://your-server-url');

socket.onopen = function(event) {
    // 连接打开后，你可以向服务器发送消息
    socket.send('Hello Server!');
};

socket.onmessage = function(event) {
    // 当收到服务器发来的消息时，触发此事件
    console.log('Received:', event.data);
};

socket.onerror = function(error) {
    // 处理错误
    console.error('WebSocket Error:', error);
};

socket.onclose = function(event) {
    // 连接关闭时触发
    console.log('WebSocket is closed now.');
};
```

### **WebSocket 服务器端实现**

服务器端实现WebSocket通常依赖于特定的服务器软件或框架，如Node.js的`ws`库、Java的Spring WebSocket等。这些库或框架提供了处理WebSocket连接的API，你可以在这些连接上发送和接收消息。

在WebSocket服务器端，你可以保存与每个客户端的连接，并在需要时向它们发送消息

下面开始做封装WebSocket的介绍

# **想象**

想象一下，你是一位超级快递员，负责把客户的包裹准确无误地送到指定的地址。这些包裹里装的是WebSocket消息，而你的任务是根据每个包裹上的`userid`和`url`信息，找到正确的收件人并将包裹送达。

首先，你需要准备一辆超级快递车（也就是WebSocket连接）。这辆车非常智能，它可以记住多个收件人的地址（`url`），并且同时为他们运送包裹。但是，每个收件人（`userid`）只能对应一个地址，这样才不会送错。

当有客户找你寄送包裹时，他们会告诉你收件人的`userid`和地址`url`。你会把这些信息记在小本本上，然后告诉超级快递车：“嘿，车车，我们要去这个地方送这个包裹给这个人！”

快递车非常听话，它会立即启动并前往指定的地址。一旦到达，它就会静静地等待，直到有包裹需要送出。

当你需要发送消息时，就像把包裹放进快递车里一样简单。你只需告诉快递车：“给这个`userid`的人送这个包裹！”快递车就会准确无误地将包裹送达给指定的收件人。

如果收件人回复了消息，快递车就像个贴心小助手一样，会第一时间把回信拿给你。你可以轻松地查看并处理这些回信。

这样一来，你就不再需要亲自跑腿送包裹了，超级快递车会帮你搞定一切。你只需要告诉它去哪里、送给谁，然后坐等好消息就行啦！

1. **WebSocketMessenger（快递服务公司）** ：

2. - 负责建立和维护WebSocket连接。
   - 采用单例模式，确保同一时间只有一个实例在运行。
   - 存储收件人（`recipient`）和地址（`address`）信息。
   - 提供发送消息（`send_message`）的方法。

3. **快递员（WebSocket连接实例）** ：

4. - 由`WebSocketMessenger`创建和管理。
   - 负责实际的消息传递工作。
   - 知道如何与指定的收件人通信（通过地址）。

5. **客户（发送消息的人）** ：

6. - 使用`WebSocketMessenger`的服务来发送消息。
   - 提供收件人信息和消息内容。

7. **收件人（接收消息的人）** ：

8. - 在WebSocket连接的另一端，接收来自`WebSocketMessenger`传递的消息。

这些角色通过WebSocket连接进行交互，实现了消息的发送和接收。`WebSocketMessenger`作为服务提供者，管理着快递员（WebSocket连接实例），而客户和收件人则是服务的使用者。

# **代码层面**

服务node代码可以看上篇文章：**仅仅只会Ajax,那就out了！WebSocket实战解锁实时通信新境界！**[1]

```
// WebSocketMessenger（快递服务公司）
class WebSocketManager {
  constructor(url = null, userId = null, receiveMessageCallback = null) {
    this.socket = null // WebSocket 对象
    this.sendTimeObj = null // 发送信息给服务端的重复调用的时间定时器
    this.reconnectTimeObj = null // 尝试链接的宏观定时器
    this.reconnectTimeDistance = 5000 // 重连间隔，单位：毫秒
    this.maxReconnectAttempts = 10 // 最大重连尝试次数
    this.reconnectAttempts = 0 // 当前重连尝试次数
    this.id = userId //用户ID（业务逻辑，根据自己业务需求调整）
    this.url = url // WebSocket 连接地址
    this.receiveMessageCallback = receiveMessageCallback // 接收消息回调函数
  }

  /**
   * 开启WebSocket
   */
  async start() {
    if (this.url && this.id) {
      // 连接WebSocket
      this.connectWebSocket()
    } else {
      console.error('WebSocket erros: 请传入连接地址和用户id')
    }
  }

  /**
   * 创建WebSocket连接, 超级快递车
   */
  connectWebSocket() {
    // 通过id生成唯一值（服务端要求，具体根据自己业务去调整）
    let id = `${this.id}-${Math.random()}`
    // 创建 WebSocket 对象
    this.socket = new WebSocket(this.url, id) // 快递员（WebSocket连接实例

    // 处理连接打开事件
    this.socket.onopen = (event) => {
      // 给服务端发送第一条反馈信息
      this.startSendServe()
    }

    // 处理接收到消息事件
    this.socket.onmessage = (event) => {
      this.receiveMessage(event)
    }

    // 处理连接关闭事件
    this.socket.onclose = (event) => {
      // 清除定时器
      clearTimeout(this.sendTimeObj)
      clearTimeout(this.reconnectTimeObj)
      // 尝试重连
      if (this.reconnectAttempts < this.maxReconnectAttempts) {
        this.reconnectAttempts++
        console.log('重试链接次数：'+ this.reconnectAttempts)
        this.reconnectTimeObj = setTimeout(() => {
          this.connectWebSocket()
        }, this.reconnectTimeDistance)
      } else {
        // 重置重连次数
        this.reconnectAttempts = 0
        console.error(
          'WebSocketManager erros: Max reconnect attempts reached. Unable to reconnect.'
        )
      }
    }

    // 处理 WebSocket 错误事件
    this.socket.onerror = (event) => {
      console.error('WebSocketManager error:', event)
    }
  }

  /**
   * 发送给node的第一条信息
   */
  startSendServe() {

    this.sendMessage('hi I come from client')
  }

  /**
   * 发送消息
   * @param {String} message 消息内容
   */
  sendMessage(message) {
    if (this.socket.readyState === WebSocket.OPEN) {
      this.socket.send(message)
    } else {
      console.error(
        'WebSocketManager error: WebSocket connection is not open. Unable to send message.'
      )
    }
  }

  /**
   * 接收到消息
   */
  receiveMessage(event) {
    // 根据业务自行处理
    console.log('receiveMessage:', event.data)
    this.receiveMessageCallback && this.receiveMessageCallback(event.data)
  }

  /**
   * 关闭连接
   */
  closeWebSocket() {
    this.socket.close()
    // 清除定时器 重置重连次数
    clearTimeout(this.sendTimeObj)
    clearTimeout(this.reconnectTimeObj)
    this.reconnectAttempts = 0
  }
}
```

# **代码解读**

该类用于管理和控制WebSocket连接，包括连接建立、消息接收、重连机制等。下面是对代码的详细解读：

## **构造函数 `constructor`**

- `url`: WebSocket的连接地址。
- `userId`: 用户的ID，用于业务逻辑处理。
- `receiveMessageCallback`: 接收消息时的回调函数。
- 初始化了一些成员变量，包括`socket`（WebSocket对象）、定时器对象（`sendTimeObj`和`reconnectTimeObj`）、重连间隔和尝试次数等。

## **`start` 方法**

- 检查`url`和`userId`是否存在，若存在则调用`connectWebSocket`方法建立WebSocket连接。

## **`connectWebSocket` 方法**

- 生成一个基于用户ID和随机数的唯一值作为WebSocket的子协议（或协议片段）。
- 创建新的WebSocket连接。
- 设置了WebSocket的`onopen`、`onmessage`、`onclose`和`onerror`事件处理器。

### **事件处理器**

- `onopen`: 当WebSocket连接打开时触发，开始发送消息给服务端（通过`startSendServe`方法，该方法在代码片段中未给出）。
- `onmessage`: 当接收到服务端发送的消息时触发，调用`receiveMessage`方法处理消息。
- `onclose`: 当WebSocket连接关闭时触发，首先清除相关定时器，然后尝试重连。如果重连次数未达到最大限制，则设置定时器在一段时间后重新调用`connectWebSocket`进行重连；如果达到最大重连次数，则重置重连次数并输出错误信息。
- `onerror`: 当WebSocket发生错误时触发，输出错误信息。当服务端断开后开始重连

![1723882704677](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723882704677.png)

![1723882718403](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1723882718403.png)

## **`receiveMessage` 方法**

- 该方法应该是用来处理从服务端接收到的消息，具体实现取决于业务逻辑。根据传入的回调函数`receiveMessageCallback`，可以对接收到的消息进行相应处理

# **使用Demo**

**index.html**

```
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="./webSocketManager.js"></script>
    <script>
        // const WebSocketManager = require('./webSocketManager.js')
        console.log(WebSocketManager)
        /**
         * 接收消息回调
         */
        const receiveMessage = (res)=>{
            console.log('接收消息回调：',res)
        }
        const socketManager = new WebSocketManager('ws://localhost:3000', 'userid292992', receiveMessage)
        socketManager.start()

    </script>
</head>
```

导入模块即可使用