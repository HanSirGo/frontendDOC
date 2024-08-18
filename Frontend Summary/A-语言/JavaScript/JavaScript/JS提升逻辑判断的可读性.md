# 如何通过JavaScript提升逻辑判断的可读性？

在前端开发过程中，我们经常会遇到需要根据不同条件执行不同逻辑的场景。对于初学者来说，这样的逻辑判断可能会导致代码冗长且难以维护。那么，如何才能写出既简洁又易读的代码呢？本文将带你逐步优化 JavaScript 中的条件判断，让你的代码变得更加优雅。

# **1. 初识逻辑判断的复杂性**

在 JavaScript 编程中，条件判断是非常常见的操作。然而，当条件变得复杂时，代码中的 if/else 或 switch 语句会变得冗长且难以维护。我们来看看下面这个例子：

```
const handleButtonClick = (status) => {
  if (status === 1) {
    navigateTo('HomePage');
  } else if (status === 2 || status === 3) {
    navigateTo('ErrorPage');
  } else if (status === 4) {
    navigateTo('SuccessPage');
  } else if (status === 5) {
    navigateTo('CancelPage');
  } else {
    navigateTo('DefaultPage');
  }
}
```

在这个例子中，我们根据不同的状态码跳转到不同的页面。虽然逻辑清晰，但随着条件的增加，代码会变得越来越臃肿和难以维护。为了优化这种代码，我们需要探索更简洁的方法。

# **2. 利用 Switch 语句优化代码**

我们可以通过 switch 语句来优化上述代码，使其更易读：

```
const handleButtonClick = (status) => {
  switch (status) {
    case 1:
      navigateTo('HomePage');
      break;
    case 2:
    case 3:
      navigateTo('ErrorPage');
      break;
    case 4:
      navigateTo('SuccessPage');
      break;
    case 5:
      navigateTo('CancelPage');
      break;
    default:
      navigateTo('DefaultPage');
  }
}
```

使用 switch 语句后，每种状态对应的处理逻辑更加直观，并且减少了重复代码。通过 switch，我们可以在一个更结构化的方式中处理多种条件。然而，随着条件的增加，switch 语句也会变得冗长。

# **3. 通过对象字面量提升代码可读性**

将条件判断放入对象中是一个非常有效的优化方式：

```
const actions = {
  1: 'HomePage',
  2: 'ErrorPage',
  3: 'ErrorPage',
  4: 'SuccessPage',
  5: 'CancelPage',
  default: 'DefaultPage',
};

const handleButtonClick = (status) => {
  const action = actions[status] || actions.default;
  navigateTo(action);
}
```

在这个示例中，我们使用对象的键值对来存储状态和对应的页面。当按钮被点击时，通过访问对象属性来决定跳转到哪个页面。这种写法不仅简洁，而且更容易维护和扩展。我们可以通过对象的键值对快速找到对应的逻辑，而不需要写一大堆 if/else 或 switch 语句。

# **4. 引入 Map 对象进一步优化**

我们还可以利用 ES6 的 Map 对象来实现相同的功能，Map 对象的键可以是任意类型，使用起来更加灵活：

```
const actions = new Map([
  [1, 'HomePage'],
  [2, 'ErrorPage'],
  [3, 'ErrorPage'],
  [4, 'SuccessPage'],
  [5, 'CancelPage'],
  ['default', 'DefaultPage'],
]);

const handleButtonClick = (status) => {
  const action = actions.get(status) || actions.get('default');
  navigateTo(action);
}
```

Map 对象允许使用任意类型的值作为键，这使得我们的代码更加灵活。我们可以通过调用 `actions.get(status)` 来获取对应的页面路径。如果状态值不存在，我们可以使用 `actions.get('default')` 获取默认值。这种方式不仅简洁，还可以避免一些键值对的冲突问题。

# **5. 处理多条件判断**

当我们的逻辑升级为二元判断时，代码量会急剧增加，例如：

```
const handleButtonClick = (status, role) => {
  if (role === 'guest') {
    if (status === 1) {
      // do something for guest
    } else if (status === 2) {
      // do something for guest
    } // ... more conditions
  } else if (role === 'admin') {
    if (status === 1) {
      // do something for admin
    } else if (status === 2) {
      // do something for admin
    } // ... more conditions
  }
}
```

当有多个条件时，代码的复杂性会进一步增加。为了简化代码，我们可以通过 Map 并将条件拼接成字符串作为键：

```
const actions = new Map([
  ['guest_1', () => { /* do something for guest and status 1 */ }],
  ['guest_2', () => { /* do something for guest and status 2 */ }],
  // ... more mappings
  ['admin_1', () => { /* do something for admin and status 1 */ }],
  ['admin_2', () => { /* do something for admin and status 2 */ }],
  ['default', () => { /* default action */ }],
]);

const handleButtonClick = (role, status) => {
  const action = actions.get(`${role}_${status}`) || actions.get('default');
  action();
}
```

我们通过拼接字符串将角色和状态组合成键，并在 Map 中查找对应的处理函数。这种方式使代码更加简洁，易于维护和扩展。每个组合键对应一个特定的处理函数，避免了嵌套的 if/else 或 switch 语句。

# **6. 使用对象作为 Map 的键**

如果将查询条件拼接成字符串感觉不太优雅，可以使用对象作为 Map 的键：

```
const actions = new Map([
  [{ identity: 'guest', status: 1 }, () => { console.log('Guest, status 1'); }],
  [{ identity: 'guest', status: 2 }, () => { console.log('Guest, status 2'); }],
  [{ identity: 'admin', status: 1 }, () => { console.log('Admin, status 1'); }],
  //...
]);

const handleButtonClick = (identity, status) => {
  // 将 Map 对象转换为数组并查找匹配的键值对
  const action = [...actions].find(([key]) => key.identity === identity && key.status === status);
  if (action) {
    // action[1] 是找到的键值对中的值（处理函数）
    action[1]();
  } else {
    console.log('Default action');
  }
}

// 测试调用
handleButtonClick('guest', 1);  // 输出: Guest, status 1
handleButtonClick('admin', 1);  // 输出: Admin, status 1
handleButtonClick('guest', 3);  // 输出: Default action
```

在这个例子中，我们将条件组合成对象，并使用这些对象作为 Map 的键。通过 `find` 方法，我们可以查找符合条件的键值对，并执行对应的处理函数。这种方法使代码更具可读性，并且更容易管理复杂的条件判断。

# **7. 使用正则表达式处理复杂条件**

假设在某些场景下，我们需要在同一个身份下处理多个相同状态的逻辑，例如，对于 guest 的 status 1 到 4 处理逻辑相同，status 5 的处理逻辑不同：

```
const actions = () => {
  const functionA = () => { console.log('Guest, status 1-4'); };
  const functionB = () => { console.log('Guest, status 5'); };
  return new Map([
    [{ identity: 'guest', status: 1 }, functionA],
    [{ identity: 'guest', status: 2 }, functionA],
    [{ identity: 'guest', status: 3 }, functionA],
    [{ identity: 'guest', status: 4 }, functionA],
    [{ identity: 'guest', status: 5 }, functionB],
    //...
  ]);
}

const handleButtonClick = (identity, status) => {
  const action = [...actions()].find(([key]) => key.identity === identity && key.status === status);
  if (action) {
    action[1]();
  } else {
    console.log('Default action');
  }
}

// 测试调用
handleButtonClick('guest', 1);  // 输出: Guest, status 1-4
handleButtonClick('guest', 5);  // 输出: Guest, status 5
```

这种方法虽然能解决问题，但需要重复定义多个相同的逻辑。为了减少重复代码，可以利用正则表达式：

```
const actions = () => {
  const functionA = () => { console.log('Guest, status 1-4'); };
  const functionB = () => { console.log('Guest, status 5'); };
  return new Map([
    [/^guest_[1-4]$/, functionA],
    [/^guest_5$/, functionB],
   

 //...
  ]);
}

const handleButtonClick = (identity, status) => {
  const action = [...actions()].find(([key]) => key instanceof RegExp && key.test(`${identity}_${status}`));
  if (action) {
    action[1]();
  } else {
    console.log('Default action');
  }
}

// 测试调用
handleButtonClick('guest', 2);  // 输出: Guest, status 1-4
handleButtonClick('guest', 5);  // 输出: Guest, status 5
handleButtonClick('admin', 3);  // 输出: Default action
```

使用正则表达式作为键，我们可以更高效地处理多个相同逻辑的条件，避免重复定义相同的处理函数。这种方法不仅减少了代码量，还使得代码更具可读性和可维护性。

# **结束**

通过以上方法，我们可以看到在 JavaScript 中进行复杂逻辑判断时，有许多方法可以让代码变得更简洁、更易维护。无论是使用对象还是 Map，都可以根据具体需求选择合适的方案。

希望这些技巧能帮助你在实际项目中写出更加优雅的代码。如果你觉得这篇文章对你有帮助，不妨点个赞并分享给更多的朋友吧！有任何疑问或建议，欢迎在评论区留言，我们一同讨论。

