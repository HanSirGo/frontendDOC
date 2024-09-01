# 前端性能优化之web worker  

# 什么是web worker？

Web Worker 是一种用于在浏览器中运行脚本操作的技术，允许JavaScript代码在一个独立的线程中运行。它可以在不阻塞用户界面的情况下执行任务，从而提高Web应用的性能和响应速度。

### Web Worker的主要特点：

1. **并行处理**：Web Workers在独立于主线程的线程中运行，这意味着它们可以处理密集型计算任务，而不会导致用户界面变得迟缓或无响应。
2. **线程隔离**：Web Workers不能直接访问主线程中的DOM和全局变量，只能通过消息传递与主线程通信。这种隔离提高了安全性和稳定性。
3. **消息传递**：主线程和Web Worker之间通过`postMessage`方法发送消息，并使用`onmessage`事件处理消息的接收。

### 使用场景：

- **复杂计算任务**：如图像处理、数据分析等需要大量计算的任务。
- **后台任务**：如在后台进行数据同步、缓存处理等任务，不会影响前台的用户体验。
- **定时任务**：处理定时器任务或长时间运行的任务而不影响用户界面响应。

本文主要介绍：DedicatedWorker（专用工作线程）、SharedWorker（共享工作线程）、ServiceWorker（服务工作线程）的基本使用、注意事项及总结

# 一. Dedicated Worker

Dedicated Worker 是最常见的 Web Worker 类型，它允许在独立的线程中运行 JavaScript 代码，从而避免阻塞主线程，提升 Web 应用的性能。每个 Dedicated Worker 仅供一个页面或模块使用，与页面的主线程之间通过消息传递进行通信。

### 使用 Dedicated Worker 的步骤

1. **创建 Worker**

2. - 在主线程中，通过 `new Worker()` 创建一个 Dedicated Worker 实例，并指定 Worker 的脚本文件。

3. **通信**

4. - 主线程和 Worker 之间通过 `postMessage()` 方法发送消息，并使用 `onmessage` 事件监听消息的接收。

5. **终止 Worker**

6. - 可以通过 `terminate()` 方法在主线程中手动终止 Worker。

### 示例代码

#### **1. 创建 Dedicated Worker**

**在主线程中的代码：**

```
// 创建一个新的 Worker 实例，加载 worker.js 脚本
let worker = new Worker('worker.js');

// 向 Worker 发送消息
worker.postMessage('Hello, Worker!');

// 监听来自 Worker 的消息
worker.onmessage = function(event) {
    console.log('主线程收到消息:', event.data);
};

// 监听 Worker 中的错误
worker.onerror = function(event) {
    console.error('Worker 中发生错误:', event.message, event.filename, event.lineno);
};

// 在需要时终止 Worker
// worker.terminate();
```

#### **2. Worker 脚本（worker.js）**

**在 worker.js 中的代码：**

```
// 监听主线程发送的消息
self.onmessage = function(event) {
    console.log('Worker 收到消息:', event.data);

    // 执行一些任务，例如处理数据
    let result = event.data + ' - 这是来自 Worker 的响应';

    // 将结果发送回主线程
    self.postMessage(result);
};

// 处理错误（可选）
self.onerror = function(event) {
    console.error('Worker 中发生错误:', event.message, event.filename, event.lineno);
};
```

### 详细解释

#### **1. 创建 Worker**

- 使用 `new Worker()` 创建一个 Dedicated Worker 实例，并指定要执行的 JavaScript 文件（`worker.js`）。这个文件会在独立的线程中执行，不会影响主线程的性能。

#### **2. 通信机制**

- 主线程和 Worker 之间通过 `postMessage()` 方法进行消息传递。任何类型的数据都可以作为消息发送（如字符串、对象等），但需要注意的是，传递的数据是通过结构化克隆算法（structured clone）来复制的，而不是引用传递，因此在 Worker 和主线程之间传递的对象是互相独立的。
- Worker 使用 `self.onmessage` 监听来自主线程的消息，接收到消息后可以执行相应的处理，并通过 `self.postMessage()` 将结果返回主线程。

#### **3. 终止 Worker**

- 当不再需要 Worker 时，可以使用 `worker.terminate()` 方法立即终止 Worker 的执行。这对于清理资源和避免不必要的性能开销非常重要。

### 使用场景

Dedicated Worker 适合以下场景：

- **复杂计算**：如大数据处理、图像处理、密码学计算等需要消耗大量 CPU 资源的操作，可以放在 Worker 中执行，以避免阻塞用户界面。
- **异步任务**：如长时间运行的异步任务，如文件处理、API 请求的批量处理等。
- **独立任务处理**：一些与页面交互较少的独立任务，可以放在 Worker 中执行，以优化页面的性能。

### 注意事项

- **无 DOM 访问**：Dedicated Worker 无法直接访问 DOM。如果需要操作 DOM，必须通过消息将数据传回主线程，由主线程负责更新 UI。
- **同源策略**：Worker 脚本必须与调用它的页面位于相同的源下，这意味着协议、主机名和端口必须一致。
- **调试**：浏览器的开发者工具通常支持调试 Worker，可以在控制台查看 Worker 的输出和错误信息。

# 二. SharedWorker

使用 `SharedWorker` 可以让多个浏览器上下文（如同一页面的多个标签页或 iframe）共享同一个 Worker 实例，从而实现数据共享或状态同步。以下是使用 `SharedWorker` 的基本步骤和示例：

### 1. **创建 SharedWorker**

在主线程中，通过 `new SharedWorker()` 创建一个 `SharedWorker` 实例，并指定 Worker 的脚本文件。

### 2. **通信接口**

与 `Dedicated Worker` 不同，`SharedWorker` 使用 `MessagePort` 对象来进行通信。每个与 `SharedWorker` 连接的页面都有一个专属的 `MessagePort`，用于发送和接收消息。

### 3. **消息传递**

使用 `postMessage` 方法向 `SharedWorker` 发送消息，`SharedWorker` 通过监听 `onmessage` 事件来处理这些消息。`SharedWorker` 也可以通过 `MessagePort` 发送消息回到主线程。

### 4. **SharedWorker 脚本**

`SharedWorker` 脚本通常监听 `connect` 事件，当一个新的页面连接时触发，接着可以处理该页面的通信请求。

### 示例代码

#### **主线程中的代码（页面脚本）**

```
// 创建一个 SharedWorker 实例，加载 sharedWorker.js 脚本
let worker = new SharedWorker('sharedWorker.js');

// 监听从 SharedWorker 发来的消息
worker.port.onmessage = function(event) {
    console.log('主线程收到消息:', event.data);
};

// 向 SharedWorker 发送消息
worker.port.postMessage('Hello, Shared Worker!');

// 启动通信接口
worker.port.start();
```

#### **SharedWorker 脚本（sharedWorker.js）**

```
// 监听 connect 事件，当有页面连接到 SharedWorker 时触发
self.onconnect = function(event) {
    // 获取连接的端口
    let port = event.ports[0];

    // 监听从页面发来的消息
    port.onmessage = function(event) {
        console.log('Shared Worker 收到消息:', event.data);

        // 向页面发送消息
        port.postMessage('Hello, Main Thread!');
    };

    // 启动通信接口
    port.start();
};
```

### 使用场景

`SharedWorker` 非常适合以下场景：

- **状态同步**：多个页面需要共享某些状态或数据，如同一网站的不同标签页共享用户会话状态。
- **数据共享**：多个页面需要共享一些数据或缓存，如多个窗口或标签页同时访问一个 WebSocket 连接。
- **后台任务**：处理需要跨页面维持的后台任务，如全局的消息处理或统计任务。

### 需要注意的事项

- **同源限制**：`SharedWorker` 只能在相同的源（协议、主机和端口）下被共享，确保安全性。
- **生命周期管理**：当所有引用 `SharedWorker` 的页面关闭后，`SharedWorker` 实例将被销毁。如果需要维持长时间的任务，确保相关页面始终保持活动状态。
- **浏览器支持**：虽然大多数现代浏览器都支持 `SharedWorker`，但在一些较旧的浏览器中可能不被支持，因此需要考虑兼容性问题。

# 三. service Worker

Service Worker 是一种特殊类型的 Web Worker，它主要用于在后台执行任务，如拦截和处理网络请求、缓存资源、离线支持、消息推送等。它与页面的生命周期独立，可以在页面关闭时继续运行，从而支持 PWA（渐进式 Web 应用）的功能。

### 使用 Service Worker 的步骤

1. **注册 Service Worker**

2. - 首先，在主线程（通常是在页面的 JavaScript 文件中）注册 Service Worker。

3. **安装 Service Worker**

4. - 在 Service Worker 脚本中处理 `install` 事件。通常在这个事件中预缓存一些静态资源。

5. **激活 Service Worker**

6. - 在 Service Worker 脚本中处理 `activate` 事件。通常在这个事件中清理旧的缓存。

7. **拦截和处理请求**

8. - 通过 `fetch` 事件拦截网络请求，可以进行缓存策略的应用，比如从缓存中获取资源或回退到网络请求。

9. **后台任务**

10. - 处理后台任务如推送通知、后台同步等。

### 示例代码

#### **1. 在主线程中注册 Service Worker**

```
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/service-worker.js')
    .then(function(registration) {
        console.log('Service Worker 注册成功:', registration);
    })
    .catch(function(error) {
        console.log('Service Worker 注册失败:', error);
    });
}
```

#### **2. Service Worker 脚本（service-worker.js）**

##### **安装阶段：缓存资源**

```
self.addEventListener('install', function(event) {
    event.waitUntil(
        caches.open('v1').then(function(cache) {
            return cache.addAll([
                '/',
                '/index.html',
                '/styles.css',
                '/script.js',
                '/images/logo.png'
            ]);
        })
    );
});
```

##### **激活阶段：清理旧缓存**

```
self.addEventListener('activate', function(event) {
    var cacheWhitelist = ['v1'];

    event.waitUntil(
        caches.keys().then(function(keyList) {
            return Promise.all(keyList.map(function(key) {
                if (cacheWhitelist.indexOf(key) === -1) {
                    return caches.delete(key);
                }
            }));
        })
    );
});
```

##### **拦截网络请求**

```
self.addEventListener('fetch', function(event) {
    event.respondWith(
        caches.match(event.request)
        .then(function(response) {
            // 如果缓存中有匹配的响应，则返回缓存中的响应
            if (response) {
                return response;
            }
            // 否则执行网络请求并将其加入缓存
            return fetch(event.request).then(function(response) {
                // 只缓存GET请求并将成功响应加入缓存
                if (!response || response.status !== 200 || response.type !== 'basic') {
                    return response;
                }

                var responseToCache = response.clone();

                caches.open('v1')
                .then(function(cache) {
                    cache.put(event.request, responseToCache);
                });

                return response;
            });
        })
    );
});
```

### 其他功能

#### **推送通知**

Service Worker 可以接收和展示推送通知，常用于与用户的互动。

```
self.addEventListener('push', function(event) {
    var options = {
        body: '这是推送通知的内容。',
        icon: '/images/icon.png',
        badge: '/images/badge.png'
    };

    event.waitUntil(
        self.registration.showNotification('推送通知标题', options)
    );
});
```

#### **后台同步**

Service Worker 还可以用于在有网络连接时同步数据。

```
self.addEventListener('sync', function(event) {
    if (event.tag === 'syncTag') {
        event.waitUntil(doSomeBackgroundSync());
    }
});

function doSomeBackgroundSync() {
    // 执行后台同步操作
}
```

### 使用场景

- **离线支持**：在离线时为用户提供基本的网页功能。
- **缓存策略**：减少对网络的依赖，提高资源加载速度。
- **消息推送**：与用户保持互动，即使在页面关闭时也能发送通知。
- **数据同步**：当用户离线时暂存数据，等用户重新上线后再进行同步。

### 注意事项

- **HTTPS**：Service Worker 只能在 HTTPS 环境下运行（本地开发时可以在 `localhost` 上运行）。
- **生命周期**：Service Worker 的生命周期与页面独立，因此在处理缓存和更新时需要考虑缓存的更新策略。
- **调试**：Service Worker 的调试可以通过浏览器的开发者工具来完成，通常在 "Application" 选项卡中查看 Service Worker 的状态和缓存内容。

# 总结

1. worker: 一般用于复杂、大量的数据计算，不会阻塞主线程，可以提升性能
2. service worker: 一般用于缓存资源，提升性能，离线访问，无网络情况的基本内容展示
3. shared worker: 一般用于多页面的通信，可以共享数据，提升性能。此外多页面通信的方式还有：service worker、localStorage、cookie、BroadcastChannel、open、postMessage、websocket等