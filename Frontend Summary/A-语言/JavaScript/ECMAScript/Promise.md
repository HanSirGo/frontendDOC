## Promise

## 异步处理ES6办法

为了兼容以往对异步处理的方法，以及解决以往异步处理事件和回调的缺陷，ES6使用了Promise A+规范，来专门处理异步问题：避免回调地狱，使异步代码更加整洁、统一。

### 传统的ajax发送请求

```js
// 发送请求
$.ajax({
    url: 'https://api.example.com/create',
    data: {
        name: 'John',
        age: 30
    },
    success: function (data) {
        console.log('请求成功', data);
    },
    error: function (error) {
        console.log('请求失败', error);
    }
});
```

看起来也很简洁，但如果我们需要在请求**成功后**再通过请求结果的值发送新的请求，代码就会成为这样：

```js
// 发送请求
$.ajax({
  url: 'https://api.example.com/create',
  data: {
    name: 'John',
    age: 30
  },
  success: function (data) {
    console.log('请求成功', data);
    $.ajax({
      url: 'https://api.example.com/query',
      data: data,
      success: function (data) {
        console.log('请求成功', data);
      },
      error: function (error) {
        console.log('请求失败', error);
      }
    });
  },
  error: function (error) {
    console.log('请求失败', error);
  }
});
```

此时代码就出现了嵌套，实际的项目中可能会有多级的嵌套请求，在代码中就会出现**多级嵌套**，这样的代码维护起来简直要命。

于是Promise出现了。我们如果用Promise来改造一下ajax，代码就会成为这样：

```js
ajaxPromise("https://api.example.com/create", {
  name: "John",
  age: 30,
})
  .then(function (data) {
    return ajaxPromise("https://api.example.com/query", data);
  })
  .catch(function (error) {
    console.log("请求失败", error);
  });
```

怎么样，是不是代码一下就简洁了很多，就算我们还要再循环嵌套多个请求，也无非就是：

```js
ajaxPromise("https://api.example.com/create", {
  name: "John",
  age: 30,
})
  .then(function (data) {
    return ajaxPromise("https://api.example.com/query", data);
  })
  .then(function (data) {
    return ajaxPromise("https://api.example.com/query1", data);
  })
  .then(function (data) {
    return ajaxPromise("https://api.example.com/query2", data);
  })
  .catch(function (error) {
    console.log("请求失败", error);
  });
```

依然有高可读性和高维护性，不会出现任何多余的代码嵌套。那么，这个ajaxPromise是如何实现的呢？代码其实也很简单：

```js
function ajaxPromise(url, data) {
  return new Promise((resolve, reject) => {
    $.ajax({
      url: url,
      data: data,
      success: function (data) {
        resolve(data);
      },
      error: function (error) {
        reject(error);
      },
    });
  });
}
```

## Promise初级版源码

### Promise基本功能

我们先来说说，如何实现一个只能实现基本功能的Promise源码：

首先使用Promise的第一步，就是新建一个Promise对象，并且要传入一个方法作为参数。类似这样：

```
new Promise(() => {
  ...
});
```

对于Promise来说，接收的方法要立即执行。所以我们要先新建一个Promise类，并将接收的一个参数方法立即执行。

我们用ES6的`class`关键字来定义一个类，把自定义方法以`executor`进行命名，并在类的构造函数`constructor`中进行接收调用：

```
class Promise {
  constructor(executor) {
    executor(); // 立即执行参数方法
  }
}
```

随后我们要知道，一个promise对象需要有哪些**属性**？我们来看看promise对象长啥样：

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053029599.png)截屏2023-11-03 17.02.48.png

可以看到，图上的三个不同的Promise实例对象，它们都有两个共同属性值：`PromiseState`和`PromiseResult`，这就是我们下一步要定义到到`Promise`中的值。

我们要先介绍一下第一个属性值：`PromiseState`，也就是Promise的“状态”：每个Promise对象都会有一个“状态”，“状态”无非上图三种情况：

- pending（等待状态）
- fulfilled（成功状态）
- rejected（失败状态）

每一个Promise对象在初始阶段都是pending状态，也就是“等待执行完成”的状态，而随着我们的自定义方法`executor`的执行，根据方法执行结果来对这个Promise对象的状态`PromiseState`进行相应修改。

另一个属性值`PromiseResult`是Promise对象的值，每个promise都有自己的值，这个值可以是任何类型，默认是`undefined`。当然这个值和“状态”一样，都是在参数方法`executor`执行时被修改的。

我们来在代码上定义这两个属性值的初始状态：

```
class Promise {
  constructor(executor) {
 const self = this;
    self.state = "pending";
    self.result = undefined;

    executor();
  }
}
```

完成了对promise对象基本属性的定义，我们要来做关键一步了：刚刚一直在说promise对象的状态和值都是被参数方法`executor`所改变的，我们就来看看自定义方法`executor`是怎么改变promise对象的**状态**和**值**的。

我们先来想想，Promise到底干了个啥？举个例子：Promise是一个很靠谱的人，他可以完成你交给他的所有事情，你只用事先把要办事的具体流程单填好（executor）：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053300459.png" alt="1710053300459" style="zoom:50%;" />截屏2023-11-09 18.18.14.png

除了告诉它你要办的事是啥，还有一个很重要的特点：Promise办事，并不是只把你交代的事情“做完”就算完事，而是“你们事先说好怎样算成功，怎样算失败”（比如上图例子，偷东西并不算成功，而是东西要符合条件才算成功）。只要你在流程单上，把什么是“办成了”的状态用**绿色的笔**打个勾，把什么是“办垮了”用**红笔**打个叉。

Promise并不会以事情做没做完来决定这一单是否成功，而是以你定义的方式来决定。最后不管事情结果咋样，Promise都会告诉你结果。

所以回到代码上，你传入的`executor`参数就是流程单，你在里面写上要交给Promise执行的代码，并且在其中调用`resolve`方法表示这事成功了，而调用`reject`方法表示这事失败了。比如这样：

```
new Promise((resolve, reject) => {
  const 采购清单 = 偷东西()
  if (采购清单.include('海鱼')) {
    resolve() // 绿笔打的勾，说明采购清单中的采购物品是海鱼，任务成功
  } else if (采购清单.include('河鱼')) {
    reject() // 红笔打的叉，说明采购清单中的采购物品是河鱼，任务失败
  }
})
```

这下你应该明白Promise是怎么用的了吧！至于这个`resolve`和`reject`是从哪来的？当然是源码中定义的，你就当它们是Promise这个人给你准备的绿笔和红笔，方便你写流程单用的就好。

这两个方法做的事情其实并不神秘，相信有同学已经猜到了，这两个方法只不过是用来改变promise状态和值而已。我们来简单的实现一下：

```
class Promise {
  constructor(executor) {
    const self = this;
    self.state = "pending";
    self.result = undefined;

    executor(
   // 在调用自定义方法executor时，传入两个方法供executor调用
      function resolve(value) {
        self.state = 'fulfilled';
        self.result = value;
      },
      function reject(reason) {
        self.state = "rejected";
        self.result = reason;
      }
    );
  }
}
```

其实不过就是这样，Promise在调用你传入的函数`executor`时，传入对应的`resolve`和`reject`方法作为参数，方法功能就是改变当前promise对象的`state`和`result`：

- 如果你调用了resolve，则表示任务成功，promise的状态会变为fulfilled；
- 如果你调用了reject，则表示任务失败，promise的状态会变为rejected；
- 不管你调用哪个方法，你调用方法时传递的参数会成为promise的result的值；

我们来调用试试看：

```
const shoppingList = ['海鱼', '海鱼', '海鱼'];  // 采购清单
const steal = () => {  // 偷采购清单的方法
  return shoppingList;
}

var p = new Promise((resolve, reject) => {
  const list = steal()
  if (list.includes('海鱼')) {
    resolve(`任务成功，购买清单中有${list.filter(item => item === '海鱼').length}份海鱼`)
  } else if (list.includes('河鱼')) {
    reject(`任务成功，购买清单中有${list.filter(item => item === '河鱼').length}份河鱼`)
  }
});

console.log(p);
```

![1710053235499](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053235499.png)截屏2023-11-22 10.45.32.png

结果看起来已经很像那么回事了，Promise已经帮我们把交给他的任务顺利完成，并且把任务状态和结果都修改成了正确的样子。

让我们再来完善一下代码，Promise有个特点：Promise的状态一旦被确定就不能再被改变了。所以我们把`resolve`和`reject`方法的功能抽到一个`change`方法中并进行判断。这个`change`方法后面还有大用处，我们多看它几眼：

```
class Promise {
  constructor(executor) {
    self = this;
    self.state = "pending";
    self.result = undefined;
 // 新建change方法来管理promise的状态和值
    const change = function change(state, data) {
      if (self.state !== "pending") return;  // 判断是否已为确定状态（fulfilled, rejected）
      self.state = state;
      self.result = data;
    };

    executor(
      function resolve(value) {
        change("fulfilled", value);
      },
      function reject(reason) {
        change("rejected", reason);
      }
    );
  }
}
```

我们来试试“防止二次篡改状态”的逻辑是否生效：先调用`resolve`方法再来调用`reject`方法会怎么样：

```
var p = new Promise((resolve, reject) => {
  resolve('成功了');
  reject('失败了');
});

console.log(p);
```

![图片](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053177124.png)截屏2023-11-22 15.33.18.png

没问题，后面调用的reject并没有再次改变promise的状态！

好了，Promise本体部分已经接近完成了，我们接下来又有新问题了：这偷采购清单可是件大事，任务成功后，怎么都得开个庆功宴；但如果失败了，我们就要开复盘大会。

而Promise不仅可以帮你完成任务本身，你还可以把要在**任务结束后要做的事情**也交代给他，他一样会帮你实现。

##### promise.then

`.then`方法的调用其实非常简单，就是传入两个参数，分别对应成功后的方法和失败后的方法：

```
p.then(  (data) => {    // 成功方法  },  (reason) => {    // 失败方法  });
```

`.then`方法的实现也很简单，我们在其中判断当前promise的状态，如果是`fulfilled`就执行第一个参数方法，如果是`rejected`就执行第二个参数方法。并且把当前promise的值（result）作为参数传递给调用的参数方法。

而`.then`方法是每个promise对象都能调用的方法，所以我们把它定义在Promise的原型对象上

```
Promise.prototype.then= function then(onfulfilled, onrejected) {
  if (this.state === "fulfilled") {
    onfulfilled(this.result);
  } else if (this.state === "rejected") {
    onrejected(this.result);
  }
}
```

很好理解吧！我们来试试效果：

```
var p = new Promise((resolve, reject) => {
    const list = steal();
    if (list.includes("海鱼")) {
      resolve(list.filter((item) => item === "海鱼").length);
    } else if (list.includes("河鱼")) {
      reject(list.filter((item) => item === "河鱼").length);
    }
});

p.then(
  (data) => {
    console.log(`任务成功，开庆功宴，开了${data}瓶香槟`);
  },
  (reason) => {
    console.log(`任务失败，开庆功宴，抽了${reason}个嘴巴`);
  }
);
```

![1710054006558](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710054006558.png)截屏2023-11-22 14.09.28.png

结果如我们所愿，promise把我们在`.then`中传递的成功方法顺利调用了，你也可以试试如果我们把任务清单内容改成“河鱼”，这里的结果会不会有变化呢？

### 用Promise处理异步问题

但我们现在的代码有点问题：一般我们需要交给Promise的任务都不是可以马上完成的（同步任务），而是需要等待一段时间才可以完成的（异步任务）。

我们假设“偷购物清单”这件事需要一秒钟，我们再来看看会怎么样：

```
var p = new Promise((resolve, reject) => {
  console.log('开始执行任务，一秒后执行完毕...');
  setTimeout(() => { // 设定定时器，一秒后偷到购物清单
    const list = steal();
    if (list.includes("海鱼")) {
      resolve(list.filter((item) => item === "海鱼").length);
    } else if (list.includes("河鱼")) {
      reject(list.filter((item) => item === "河鱼").length);
    }
  }, 1000)
});

p.then(
  (data) => {
    // 成功方法
    console.log(`任务成功，开庆功宴，开了${data}瓶香槟`);
  },
  (reason) => {
    // 失败方法
    console.log(`任务失败，开庆功宴，抽了${reason}个嘴巴`);
  }
);
```

这时候我们的代码就出问题了，我们没有获得任何输出结果。

原因也不复杂：`.then`中的方法是立即执行的，此时`p`这个promise还没有确定任务结果（还没有执行完任务），在`then`方法执行时`p`还是pending状态，既不属于成功（fulfilled）的，也不属于失败（rejected）的，自然就不会有任何输出结果了。那这个多简单！我们再写一个判断条件不就行了：

```
Promise.prototype.then= function then(onfulfilled, onrejected) {
  if (this.state === "fulfilled") {
    onfulfilled(this.result);
  } else if (this.state === "rejected") {
    onrejected(this.result);
  } else if (this.state === "pending") {
    // 还未完成任务的情况
    console.log(this);
  }
}
```

![1710053964391](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053964391.png)截屏2023-11-22 14.20.11.png

这下有点不好办了，`then`方法在被执行的时候任务还没有执行完，此时我们也不知道任务结果，这也没法去调用相应的回调函数呀。

我们想想怎么解决这个问题。还记得当promise任务执行完成，不论成功失败，唯一可以改变promise状态的方法是什么吗？没错，就是我们前面写的`change`方法：

```
class Promise {
  constructor(executor) {
    self = this;
    self.state = "pending";
    self.result = undefined;

    // 新建change方法来管理promise的状态和值
    const change = function change(state, data) {
      if (self.state !== "pending") return;
      self.state = state;
      self.result = data;
    };

    executor(
      (data) => {
        //resolve
        change("fulfilled", data);
      },
      (reason) => {
        // reject
        change("rejected", reason);
      }
    );
  }
}
```

既然只有`change`方法可以改变promise的状态，也就是说不管任务什么时候执行完毕，`change`方法都是第一个知道的。那我们不如这样：如果`then`方法执行时promise还是pending状态（还没有执行完任务），那就麻烦`change`方法帮帮忙，等任务执行完后，由`change`方法来调用对应的成功/失败回调。

逻辑很简单，但还是有个小问题：`then`方法和`change`方法直接没法直接沟通，毕竟`then`方法是原型方法，而`change`方法是个私有方法。我们不如直接给每个任务都建立一个备忘录，`then`方法如果遇到当前promise还没完成的情况，就把它所接收到的两个函数记录在备忘录上；而`change`方法执行的时候，就去看看备忘录上有没有记录，有的话就把对应的方法执行了。

说干就干：

```
class Promise {
  constructor(executor) {
    self = this;
    self.state = "pending";
    self.result = undefined;
    self.onfulfilledCallbacks = [];  // 成功回调备忘录
    self.onrejectedCallbacks = [];  // 失败回调备忘录

    const change = function change(state, data) {
      if (self.state !== "pending") return;
      self.state = state;
      self.result = data;
   // 执行“备忘录”中的回调方法
      const callbacks = self.state === "fulfilled" ? self.onfulfilledCallbacks : self.onrejectedCallbacks;  // 根据成功/失败状态，判断要执行的备忘录
      callbacks.forEach((callback) => {  // 遍历执行备忘录中所有方法
        callback(self.result);
      });
    };

    executor(
      (data) => {
        change("fulfilled", data);  //resolve
      },
      (reason) => {
        change("rejected", reason);  // reject
      }
    );
  }
}

Promise.prototype.then= function then(onfulfilled, onrejected) {
  if (this.state === "fulfilled") {
    onfulfilled(this.result);
  } else if (this.state === "rejected") {
    onrejected(this.result);
  } else if (this.state === "pending") {
 // 还未完成任务的情况
    this.onfulfilledCallbacks.push(onfulfilled);  // 将当前未执行的成功回调写到“成功备忘录”中
    this.onrejectedCallbacks.push(onrejected);  // 将当前未执行的失败回调写到“失败备忘录”中
  }
}
```

新增的代码不多，我们将“备忘录”设置为两个数组：“成功备忘录（onfulfilledCallbacks）”和“失败备忘录（onrejectedCallbacks）”。因为同一个Promise对象的`then`方法可以被多次调用（可以多开几次庆功宴），所以我们用两个数组来进行存储。

在`then`方法中，对于当前未完成的promise，我们将对应的回调方法存入到对应的备忘录中。

在`change`方法中，我们根据promise执行完成后的情况，对应地遍历执行对应的备忘录中的回调方法。

就是这么简单，我们来看看效果：

![1710053914008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1710053914008.png)录屏2023-11-22 14.46.54.2023-11-22 14_47_34.gif

完美实现了！

### 完整版初级Promise

当然，Promise还有一个特点就是：`executor`会同步执行，而`.then`中的回调方法都是异步执行的。我们来略微改造一下代码，把每个`.then`中的回调方法都放入到异步微任务队列中去等待执行（如果你不知道什么是异步宏任务和异步微任务，记得留言告诉我，下次教你浏览器的“事件循环机制”），我们来看看改造后的代码全貌：

```
class Promise {
  constructor(executor) {
    self = this;
    self.state = "pending";
    self.result = undefined;
    self.onfulfilledCallbacks = [];   // 成功回调备忘录
    self.onrejectedCallbacks = [];    // 失败回调备忘录

    const change = function change(state, data) {
      if (self.state !== "pending") return;
      self.state = state;
      self.result = data;
      // 执行“备忘录”中的回调方法
      const callbacks = self.state === "fulfilled" ? self.onfulfilledCallbacks : self.onrejectedCallbacks;  // 根据成功/失败状态，判断要执行的备忘录
      callbacks.forEach((callback) => { // 遍历执行备忘录中所有方法
        queueMicrotask(() => callback(self.result));
      });
    };

    executor(
      (data) => {
        change("fulfilled", data);  //resolve
      },
      (reason) => {
        change("rejected", reason); // reject
      }
    );
  }
}

Promise.prototype.then= function then(onfulfilled, onrejected) {
  if (this.state === "fulfilled") {
    queueMicrotask(() => onfulfilled(this.result));
  } else if (this.state === "rejected") {
    queueMicrotask(() => onrejected(this.result));
  } else if (this.state === "pending") {
    // 还未完成任务的情况
    this.onfulfilledCallbacks.push(onfulfilled);    // 将当前未执行的成功回调写到“成功备忘录”中
    this.onrejectedCallbacks.push(onrejected);    // 将当前未执行的失败回调写到“失败备忘录”中
  }
}

const shoppingList = ["海鱼", "海鱼", "海鱼"];
const steal = () => {
  return shoppingList;
};

var p = new Promise((resolve, reject) => {
  console.log('开始执行任务，一秒后执行完毕...');
  setTimeout(() => {    // 设定定时器，一秒后偷到购物清单
    const list = steal();
    if (list.includes("海鱼")) {
      resolve(list.filter((item) => item === "海鱼").length);
    } else if (list.includes("河鱼")) {
      reject(list.filter((item) => item === "河鱼").length);
    }
  }, 1000)
});

p.then(
  (data) => {
    // 成功方法
    console.log(`任务成功，开庆功宴，开了${data}瓶香槟`);
  },
  (reason) => {
    // 失败方法
    console.log(`任务失败，开庆功宴，抽了${reason}个嘴巴`);
  }
);
```

##### promise A+规范：

在promiseA+中，把每一个异步任务都看做一个promise对象，每一个任务对象都有两个阶段（unsettled，settled）,三个状态（pending，resolved，rejected，）

**两个阶段：**

- unsettled：事情未解决阶段，事情还没有结果。
- settled：事情已解决阶段，无论是成功，还是失败，已经有结果了。

**三个状态：**

- padding：挂起，处于未解决阶段。事情还没有结果输出，promise对象为挂起状态。
- resolved：已处理，处于已解决阶段。已经有了成功的结果，可以按照正常逻辑进行下去，并可以传递一个成功信息，后续可以用.then的onFulfiled处理。
- rejected：已拒绝，处于已解决阶段，已经有了失败的结果，无法按照正常逻辑进行下去，可以传递一个错误信息，后续可以用.then的onRejected处理，或使用catch处理。

两个阶段是由unsettle -> settled，不可逆。

三个状态是由padding -> resolved/rejected，不可逆，且resolved和rejected之间的状态不可以互相切换。

也就是说，promise对象一旦有了结果，结果不可改变，不可消失。

**一张图看懂promise A+：**

![1709980164020](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709980164020.png)PromiseA+.jpg

ES6为我们提供了PromiseAPI，来实现Promise对象的书写。

##### promise的基本使用

```js
const p1 = new Promise((resolve, reject) => {
  //此部分为同步部分
  //此部分决定Promise的状态，未决定之前为Padding不能进行后续对该Promise的处理
  //resolve和reject只能接受和传递一个参数
  //resolve将Promise对象推入已处理状态
  //reject将Promise对象推入已拒绝状态
  resolve("请求成功");
  //状态一旦改变就不可以更改
  //reject('请求失败')
});

//处理Promise的结果，为异步部分。
//若对应的promise为pending，则会加入作业队列，等待Promise结果。
//若Promise结果为resolved或rejected，加入微任务队列等待执行JS引擎轮询到立马执行。
p1.then(
  (value) => {
    //此部分为onFulfilled，也就是thenabled处理，Promise状态为resolved接收参数并执行此部分代码。
    console.log(`Promise已处理，接收到传值：${value}`);
  },
  (err) => {
    //此部分为onRejected，也就是catchabled处理，Promise状态为reject接收参数并执行此部分代码。
    //Promise对象的then方法调用中也可以不写此部分，不进行错误处理，在Promise对象的catch方法中进行错误处理。
    console.log(`Promise已拒绝,接收到原因：${err}`);
  }
);

//Promise并没有消除回调，只是让回调变得可控了。
```

##### Promise封装Ajax

```js
function myAjax(obj) {
  //返回一个Promise对象
  return new Promise((resolve, reject) => {
    let xhr = null;
    //在不同浏览器下创建ajax对象
    if (window.XMLHttpRequest) {
      xhr = new XMLHttpRequest();
    } else {
      xhr = new ActiveXObject();
    }

    //设置ajax默认值
    obj.type = obj.type || "get";//默认请求方法为get
    obj.data = obj.data || null;
    obj.async = obj.async || true;//默认请求为异步

    if (obj.type === "post") {
      let data = toQueryString(obj.data);
      xhr.setHeader("Content-Type", "application/x-www-form-urlencoded");
      xhr.open("post", url, obj.async);
      xhr.send(data);
    } else {
      let url = obj.url + toQueryString(obj.data);
      xhr.open("get", url, obj.async);
      xhr.send();
    }

    xhr.onreadyStateChange = function () {
      if (xhr.readyState == 4) {
        if ((xhr.status >= 200 && xhr <= 300) || xhr == 304) {
          resolve(JSON.parse(xhr.responseText));
        } else {
          reject(JSON.parse(xhr.status));
        }
      }
    };
  });
}

//请求参数处理
function toQueryString(data) {
  if (data) {
    let url = "";
    for (let [key, value] of Object.entries(data)) {
      url = `${key}&${value}`;
    }
    return url;
  } else {
    return "";
  }
}
```

##### Promise的串联：

promise.then()，promise.catch()，promise.resolve()返回的都是一个Promise对象。所以我们可以继续对新返回的Promise对象进行处理。所以Promise可以链式调用。后续的Promise一定会等到前面的Promise状态返回并处理完成后，才会进入已处理阶段。

在当前Promise的阶段在已解决阶段时，Promise的处理返回的Promise对象默认是resolved状态。如果在Promise的处理过程中抛出错误，或是返回一个新的Promise对象状态为reject，将会影响到Promise处理返回的新的Promise状态。

##### Promise的API：

- Promise.then:接收两个参数，注册两个后续处理函数，第一个参数注册resolved时的处理函数，第二个参数注册rejected时的处理函数。我们通常用then来处理resolved，第二个参数可以不填
- Promise.catch：注册rejected时的处理函数，链式调用时，对catch以前的rejected都可以统一处理。
- Promise.finally：注册一个处理函数，无论resolved还是rejected都会执行。
- Promise.resolve：返回一个resolved状态的Promise对象，并可以传一个参数。
- Promise.reject：返回一个rejected状态的Promise对象，并可以传递一个参数。
- Promise.all：接收一个Promise对象的数组，当所有Promise对象都进入已决阶段时，执行并返回一个新的Promise对象 。若所有Promise的处理结果均为resolved，那么新返回的Promise对象状态为resolved，并传递一个拥有所有Promise对象resolved传值，并且按顺序排列的数组。若Promise中有rejected状态，则新的Promise对象状态也为rejected，并传递第一个rejected的结果。
- Promise.race：接收一个Promise对象的数组，以第一个进入已决阶段的Promise结果为准。返回第一个已决Promise的结果作为新的Promise对象的结果。
- Promise.allSettled：接收一个Promise对象的数组，等待所有Promise已决，便进入resolved状态，传递所有Promise结果的数组。
- Promise.any：接收一个Promise对象的数组，当出现第一个resolve状态的Promise时，发生短路，并传递resolve值。若全部rejected则会抛出一个记录错误信息的数组。

## 手写Promise

![1709980249447](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709980249447.png)手写Promise.jpg

```js

//起步构建
// 1.用类创建Promise，类中需要有个执行器executor
// 2.执行者中发生错误，交给异常状态处理
// 3.执行者中状态只能触发一次，状态触发一次之后，不能修改状态
// 4.执行者中的this，由调用执行者的作用域决定，因此我们需要将执行者中的this绑定为我们创建的Promise对象。
// 5.在构造函数中需要为Promise对象创建status和value记录Promise的状态和传值。

class MyPromise {
    static PENDING = 'pending'
    static FULFILLED = 'fulfilled'
    static REJECTED = 'rejected'

    constructor(executor) {
        this.status = MyPromise.PENDING;
        this.value = null;
        this.callbacks = [];
        try {
            executor(this.resolve.bind(this), this.reject.bind(this))
        } catch (error) {
            this.reject(error)
        }
    }

    resolve(value) {
        if (this.status == MyPromise.PENDING) {
            this.status = MyPromise.FULFILLED;
            this.value = value
            setTimeout(() => {
                this.callbacks.map(item => {
                    item.onFulfilled(this.value);
                })
            })

        }
    }

    reject(reason) {
        if (this.status == MyPromise.PENDING) {
            this.status = MyPromise.REJECTED;
            this.value = reason
            setTimeout(() => {
                this.callbacks.map(item => {
                    item.onRejected(this.value);
                })
            })

        }
    }

    //开始写then方法
    //1.then接收2个参数，一个成功回调函数，一个失败回调函数
    //2.then中发生错误，状态为rejected，交给下一个then处理
    //3.then返回的也是一个Promise
    //4.then的参数值可以为空，可以进行传值穿透
    //5.then中的方法是异步执行的
    //6.then需要等promise的状态改变后，才执行，并且异步执行
    //7.then是可以链式操作的
    //8.then的onFulfilled可以用来返回Promise对象，并且then的状态将以这个Promise为准
    //9.then的默认状态是成功的，上一个Promise对象的状态不会影响下一个then的状态
    //10.then返回的promise对象不是then相同的promise
    then(onFulfilled, onRejected) {
        if (typeof onFulfilled != 'function') {
            onFulfilled = value => value
        }

        if (typeof onRejected != 'function') {
            onRejected = reason => reason
        }

        let promise = new MyPromise((resolve, reject) => {
            if (this.status == MyPromise.FULFILLED) {
                setTimeout(() => {
                    this.parse(promise, onFulfilled(this.value), resolve, reject)
                });

            }

            if (this.status == MyPromise.REJECTED) {
                setTimeout(() => {
                    this.parse(promise, onRejected(this.value), resolve, reject)
                })

            }

            if (this.status == MyPromise.PENDING) {
                this.callbacks.push({
                    onFulfilled: value => {
                        this.parse(promise, onFulfilled(value), resolve, reject)
                    },
                    onRejected: reason => {
                        this.parse(promise, onRejected(reason), resolve, reject)
                    }
                });

            }
        })
        return promise
    }

    //整理冗余代码
    parse(promise, result, resolve, reject) {
        if (promise == result) {
            throw new TypeError('Chaining cycle detected for promise')
        }
        try {
            if (result instanceof MyPromise) {
                result.then(resolve, reject)
            } else {
                resolve(result)
            }
        } catch (error) {
            reject(error)
        }
    }

    //Promise的静态方法，resolve
    static resolve(value) {
        return new MyPromise((resolve, reject) => {
            if (value instanceof MyPromise) {
                value.then(resolve, reject)
            } else {
                resolve(value)
            }
        })
    }

    //Promise的静态方法，reject
    static reject(reason) {
        return new MyPromise((resolve, reject) => {
            reject(reason)
        })
    }

    //Promise的静态方法，all
    static all(promises) {
        let values = [];
        return new MyPromise((resolve, reject) => {
            promises.forEach(promise => {
                if (promise.status == MyPromise.FULFILLED) {
                    values.push(promise.value)
                } else if (promise.status == MyPromise.REJECTED) {
                    reject(promise.value)
                }
                if (values.length == promises.length) {
                    resolve(values)
                }
            });
        })
    }

    //Promise的静态方法，race
    static race(promises) {
        return new MyPromise((resolve, reject) => {
            promises.forEach(promise => {
                promise.then(value => {
                    resolve(value)
                })
            });
        })
    }

    //Promise的静态方法，race
    catch (onRejected) {
        return this.then(null, onRejected)
    }
}
```

## Async Await

Async Await是ES6中对Promise的进一步封装语法糖。它能使得我们的Promise的异步代码，看上去像是同步书写。Promise虽然可以解决多个异步任务的执行顺序问题，但写法依然采用了回调的形式，当多个Promise嵌套使用时，依然会变得很难看。采用Async Await可以很好的解决这个问题。

Async Await的原理借鉴了ES5中的生成器。目的是化简Promise API。

async加在函数声明之前，使得整个函数内部，变为Promise的同步部分。

await必须在async函数中使用，后面一般跟一个Promise对象，得到promise对象的thenabled的状态数据。若await后面非Promise对象，则返回该值。

await下一行开始的代码，需等待await后的表达式执行完毕后，才会执行。

## 面试真题与解答

1. 封装setTimeout，模拟setInterval

```js
//封装setTimeout
function delay(time) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, time);
  });
}

async function mySetTimeout(time) {
  await delay(time);
  console.log(`等待${time}s,执行...`);
}

mySetTimeout(2000);

//模拟setInterval
async function mySetInterval(time) {
  let i = 1;
  while (true) {
    await delay(time);
    console.log(i++);
  }
}

function delay(time) {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve();
    }, time);
  });
}

    mySetInterval(1000);

```

1. Promise的缺点：

   无法取消。





## Promise.allSettled

从 ES2020 开始，你可以使用 `Promise.allSettled`。当所有的 promises 都已经结束无论是完成状态或者是失败状态，它都会返回一个 promise，这个 promise 将会包含一个关于描述每个 promise 状态结果的对象数组。

```js
const promise1 = Promise.resolve(3)
const promise2 = Promise.reject(new Error('An error'))
const promise3 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'))
const promises = [promise1, promise2, promise3]

Promise.allSettled(promises).then((res) => console.log(res))
```

![1709973514550](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709973514550.png)

```
Promise.allSettled() 方法返回一个 Promise，该 Promise 在所有给定的 Promise 已经 resolve 或 reject 后 resolve，提供每个 Promise 的结果数组。
```

```js
const promises = [
  Promise.resolve('Resolved'),
  Promise.reject('Rejected')
];

Promise.allSettled(promises)
  .then(results => {
    console.log(results);
  });
// [{ status: "fulfilled", value: "Resolved" }, { status: "rejected", reason: "Rejected" }]
```

## **Promise.any**

`Promise.any()` 方法将一组可迭代的 Promises 作为输入，并返回一个 Promise，只要参数实例有一个变成 fulfilled 状态，包装实例就会变成 fulfilled 状态；如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态。

```js
const promises = [  
    Promise.reject('ERROR A'),  
    Promise.reject('ERROR B'),  						Promise.resolve('result'),
]
Promise.any(promises).then((value) => {  				console.log('value: ', value)   // value:  result
}).catch((err) => {  
    console.log('err: ', err)
})
```

如果所有传入的 `promises` 都失败：

```js
const promises2 = [      
    Promise.reject('ERROR A'), 
    Promise.reject('ERROR B'),
    Promise.reject('ERROR C'),
]    
Promise.any(promises2).then((value) => {      			console.log('value：', value)
}).catch((err) => {      
    console.log('err:', err)      						console.log(err.errors)    
})
```

![1709973322937](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1709973322937.png)

## **Promise.race**

`Promise.race()`方法将一组可迭代的 Promises 作为输入，并返回一个 Promise ，顾名思义，race 就是赛跑的意思，Promise.race([p1, p2, p3]) 里面哪个结果获得的快，就返回那个结果。

```js
const promise1 = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, 'one')
})
const promise2 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'two')
})
Promise.race([promise1, promise2]).then((value) => {
  console.log(value)  // 'two'
})
```

## **Promise.all**

`Promise.all()`方法将一组可迭代的 Promises 作为输入，并返回一个 Promise ，该 Promise resolve 的结果为刚才那组输入 promises 的返回结果。

```js
const promise1 = Promise.resolve(3)
const promise2 = 42
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo')
})

Promise.all([promise1, promise2, promise3]).then((values) => {
  console.log(values)  //  [3, 42, "foo"]
})
```

当三个 promise 都完成时，`Promise.all` 就完成了，并且输出被打印了。如果其中一个 promise 失败了，则 Promise.all 整体将会失败。

```js
const promise1 = Promise.resolve(3)
const promise2 = Promise.reject('error')
const promise3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, 'foo')
})

Promise.all([promise1, promise2, promise3])
    .then(values => ...)
    .catch(err => console.log(err))   // 'error'
```

当需要处理多个 Promise 并行时，大多数情况下 Promise.all 用起来是非常顺手的，但是一旦有一个promise 出现了异常（reject），情况就会变的麻烦。尽管能用 catch 捕获其中的异常，但你会发现其他执行成功的 Promise 的消息都丢失了，仿佛石沉大海一般。

要么全部成功，要么全部重来，这是 Promise.all 本身的强硬逻辑，也是痛点的来源，这给 Promise.allSettled 留下了立足的空间。

问题：Promise.all 如果有报错，是在 then 还是 catch 接收数据？

```js
let p1 = Promise.resolve(1)
let p2 = Promise.resolve(2)
let p3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 100, "3")
});

let p4 = Promise.reject(4)
let p5 = Promise.reject(5).catch((e) => e)

// 情况1：promise全部resolve
Promise.all([p1, p2, p3]).then(values => {
    console.log(values)  // 其中p1、p2、p3都成功，所以进入then()
}).catch(function(err) {
    console.log(err)    // 永远走不到这里
});

// 情况2：promise有一个reject，且reject没有主动捕获异常
Promise.all([p1, p2, p4]).then(values => {
    console.log(values)  // 其中p4失败，且没有主动捕获异常，永远走不到这里
}).catch(function(err) {
    console.log(err)     // 此时走到catch  4
});

// 情况3：promise有一个reject，且reject主动捕获异常
Promise.all([p1, p2, p5]).then(values => {
    console.log(values)    // 其中p5失败，但是主动捕获了p5的异常，所以进入then()
}).catch(function(err) {
    console.log(err)      // 永远走不到这里
});

// 情况4：使用Promise.allSettled
Promise.allSettled([p1, p2, p4]).then(values => {
    console.log(values)   // 其中p4失败，但是使用Promise.allSettled，所以进入then()
}).catch(function(err) {
    console.log(err)
})
```

## **Promise.any vs Promise.all**

- `Promise.all()` ：任意一个 promise 被 reject ，就会立即被 reject ，并且 reject 的是第一个抛出的错误信息；只有所有的 promise 都 resolve 时整体才会 resolve。
- `Promise.any()` ：任意一个 promise 被 resolve ，就会立即被 resolve ，并且 resolve 的是第一个正确结果；只有所有的 promise 都 reject 时整体才会 reject。

## **Promise.any vs Promise.race**

- `Promise.race()` ：返回最先完成的 promise 的结果，无论成功还是失败；假如第一个 promise 失败了，那么就会结束。
- `Promise.any()`：不会因为某个 Promise 变成 rejected 而结束，哪怕首个 promise 是 rejected，可能还会有另一个 promise 成功。

## **Promise.all vs Promise.allSettled**

一句话概括两者的最大不同：Promise.allSettled 永远不会被 reject

- `Promise.all` 将在 Promises 数组中的其中一个 Promises 失败后立即失败。
- `Promise.allSettled`将永远不会失败，一旦数组中的所有 Promises 被完成或失败，它就会完成。
- `Promise.allSettled`是对 Promise.all 的一种补充，解决了当多个 promise 并行时 reject 的出现会伴随着其他 promise 数据丢失的问题。

