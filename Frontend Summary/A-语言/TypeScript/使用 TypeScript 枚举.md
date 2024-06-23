# 通过五个真实应用场景，深入理解如何使用 TypeScript 枚举（enum）

# **一、简单的示例：方向操作**

枚举的一个常见用例是：在有限的选项集合中进行选择，使代码更清晰明了。下面我们来看看一个简单的例子，通过枚举来处理方向操作。![1719050696370](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719050696370.png)

```js
enum Movement {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT"
}

function handlePlayerInput(key: string) {
  switch (key) {
    case Movement.Up:
      // 移动玩家角色向上
      console.log("移动玩家角色向上");
      break;
    case Movement.Down:
      // 移动玩家角色向下
      console.log("移动玩家角色向下");
      break;
    case Movement.Left:
      // 移动玩家角色向左
      console.log("移动玩家角色向左");
      break;
    case Movement.Right:
      // 移动玩家角色向右
      console.log("移动玩家角色向右");
      break;
    default:
      console.log("无效的输入");
  }
}

```

在这个例子中，我们定义了一个名为 Movement 的枚举，它包含四个成员，分别代表四个方向：上、下、左、右。然后，我们使用这个枚举在 handlePlayerInput 函数中处理玩家的输入。

## **为什么要用枚举？**

- 代码更清晰：使用枚举后，代码更具可读性。你可以清楚地看到每个方向对应的具体操作，而不必依赖字符串或数字。
- 防止错误：枚举使得输入值更加有限，减少了拼写错误的可能性。例如，使用字符串时，容易出现拼写错误，而使用枚举则可以避免这种情况。
- 易于维护：如果需要添加新的方向或修改现有的方向，只需在枚举中进行修改，而不需要在多个地方进行字符串替换。

总之，枚举让代码更加直观和可靠，是组织和管理固定选项集合的有效工具。

# **二、 HTTP 状态码**

枚举不仅可以表示简单的选项集合，还可以关联特定的值（如数字、字符串等）。下面我们通过一个示例展示如何使用带值的枚举来确保类型安全，并防止使用任意数字。

![1719050731622](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719050731622.png)

```js
enum StatusCode {
  OK = 200,
  BadRequest = 400,
  NotFound = 404
}

function handleResponse(code: StatusCode): string {
  if (code === StatusCode.OK) {
    return "请求成功";
  } else if (code === StatusCode.NotFound) {
    return "资源未找到";
  } else if (code === StatusCode.BadRequest) {
    return "错误请求";
  } else {
    return "未知响应码";
  }
}

// 假设我们有几个不同的响应码
const responseCode1 = 200;
const responseCode2 = 404;
const responseCode3 = 400;

// 将数字转换为StatusCode类型，并调用handleResponse函数
console.log(handleResponse(responseCode1 as StatusCode)); // 输出：请求成功
console.log(handleResponse(responseCode2 as StatusCode)); // 输出：资源未找到
console.log(handleResponse(responseCode3 as StatusCode)); // 输出：错误请求
```

在这个例子中，我们定义了一个名为 StatusCode 的枚举，它包含三个成员，分别代表 HTTP 状态码：200（OK），400（BadRequest），404（NotFound）。然后，我们在 handleResponse 函数中使用这个枚举来处理不同的 HTTP 响应码。

# **三、在 Redux Toolkit 中使用枚举**

Redux Toolkit 是一个流行的状态管理库，特别适用于 React 应用。它大量使用 TypeScript 来确保类型安全。以下是一个定义异步操作状态的枚举，这在状态管理库中非常常见。

![1719050766653](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719050766653.png)

```js
enum PayloadActionLoadingState {
  Idle = "idle",
  Loading = "loading",
  Failed = "failed",
  Success = "success"
}
```

这个枚举定义了异步操作的不同状态，如“空闲”（Idle）、“加载中”（Loading）、“失败”（Failed）和“成功”（Success）。在 Redux Toolkit 中，管理这些状态非常常见。

## **在 Redux Toolkit 中应用枚举**

假设我们有一个 Redux slice 来管理异步数据获取操作的状态。我们可以使用 PayloadActionLoadingState 枚举来定义状态并处理相应的操作。

### **定义 Slice**

首先，我们定义一个 Redux slice：

```js
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface DataState {
  data: any;
  loadingState: PayloadActionLoadingState;
  error: string | null;
}

const initialState: DataState = {
  data: null,
  loadingState: PayloadActionLoadingState.Idle,
  error: null,
};

const dataSlice = createSlice({
  name: 'data',
  initialState,
  reducers: {
    fetchDataStart(state) {
      state.loadingState = PayloadActionLoadingState.Loading;
      state.error = null;
    },
    fetchDataSuccess(state, action: PayloadAction<any>) {
      state.loadingState = PayloadActionLoadingState.Success;
      state.data = action.payload;
    },
    fetchDataFailed(state, action: PayloadAction<string>) {
      state.loadingState = PayloadActionLoadingState.Failed;
      state.error = action.payload;
    },
  },
});

export const { fetchDataStart, fetchDataSuccess, fetchDataFailed } = dataSlice.actions;

export default dataSlice.reducer;
```

### **使用 Slice 和 枚举**

在 React 组件中使用这个 slice 和枚举：

```js
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { fetchDataStart, fetchDataSuccess, fetchDataFailed } from './dataSlice';
import { RootState } from './store';
import { PayloadActionLoadingState } from './payloadActionLoadingState';

const DataComponent: React.FC = () => {
  const dispatch = useDispatch();
  const { data, loadingState, error } = useSelector((state: RootState) => state.data);

  useEffect(() => {
    const fetchData = async () => {
      dispatch(fetchDataStart());
      try {
        const response = await fetch('https://api.example.com/data');
        const result = await response.json();
        dispatch(fetchDataSuccess(result));
      } catch (err) {
        dispatch(fetchDataFailed(err.toString()));
      }
    };

    fetchData();
  }, [dispatch]);

  if (loadingState === PayloadActionLoadingState.Loading) {
    return <div>Loading...</div>;
  }

  if (loadingState === PayloadActionLoadingState.Failed) {
    return <div>Error: {error}</div>;
  }

  if (loadingState === PayloadActionLoadingState.Success) {
    return <div>Data: {JSON.stringify(data)}</div>;
  }

  return <div>Idle</div>;
};

export default DataComponent;
```

#### **解释**

1、**定义枚举**：

- PayloadActionLoadingState 枚举定义了异步操作的四种状态。

2、**创建 Slice**：

- 定义了 DataState 接口来表示状态结构。
- 使用 createSlice 创建了一个名为 data 的 slice，包含初始状态和 reducers。

3、**处理异步操作**：

- fetchDataStart：设置 loadingState 为 Loading，并清空错误信息。
- fetchDataSuccess：设置 loadingState 为 Success，并存储获取到的数据。
- fetchDataFailed：设置 loadingState 为 Failed，并存储错误信息。

4、**在组件中使用**：

- 使用 useDispatch 和 useSelector 访问 Redux 状态和 dispatch actions。
- 在 useEffect 中发起异步请求，处理不同的状态。根据 loadingState 渲染不同的 UI。

通过使用枚举 PayloadActionLoadingState，我们确保了状态的类型安全，并使代码更具可读性和可维护性。希望这个例子能帮助你更好地理解如何在 Redux Toolkit 中使用枚举来管理异步操作状态。

# **四、使用枚举作为判别联合类型**

这个例子展示了如何使用枚举来定义两个可能的形状：圆形（Circle）和矩形（Rectangle）。这是确保在处理不同形状时的类型安全的基础。

每个形状类型（Circle, Rectangle）都表示为 ShapeType 枚举的一个成员。

Shape 接口有一个 type 属性，它必须是 ShapeType 枚举的一个成员。

```js
enum ShapeType {
    Circle = "Circle",
    Rectangle = "Rectangle"
}

interface Shape {
    type: ShapeType;
}

interface Circle extends Shape {
    type: ShapeType.Circle;
    radius: number;
}

interface Rectangle extends Shape {
    type: ShapeType.Rectangle;
    width: number;
    height: number;
}

function calculateArea(shape: Shape): number {
    switch (shape.type) {
        case ShapeType.Circle:
            const circle = shape as Circle; // 类型断言为 Circle
            return Math.PI * circle.radius * circle.radius;
        case ShapeType.Rectangle:
            const rectangle = shape as Rectangle; // 类型断言为 Rectangle
            return rectangle.width * rectangle.height;
        default:
            throw new Error("Invalid shape type");
    }
}
```

具体的形状接口（Circle, Rectangle）扩展了基础的 Shape 接口，并且必须将其 type 属性设置为对应的枚举值。

我们可以创建一些形状实例，并使用 calculateArea 函数来计算它们的面积：

```js
const myCircle: Circle = {
    type: ShapeType.Circle,
    radius: 5
};

const myRectangle: Rectangle = {
    type: ShapeType.Rectangle,
    width: 10,
    height: 5
};

console.log(`Circle Area: ${calculateArea(myCircle)}`); // 输出：Circle Area: 78.53981633974483
console.log(`Rectangle Area: ${calculateArea(myRectangle)}`); // 输出：Rectangle Area: 50
```

## **代码解释**

1、**定义枚举**：

- ShapeType 枚举包含两个成员：Circle 和 Rectangle，表示两种形状类型。

2、**定义基础接口**：

- Shape 接口有一个 type 属性，类型为 ShapeType 枚举。

3、**扩展接口**：

- Circle 接口扩展了 Shape，并添加了 radius 属性，同时将 type 属性固定为 ShapeType.Circle。
- Rectangle 接口扩展了 Shape，并添加了 width 和 height 属性，同时将 type 属性固定为 ShapeType.Rectangle。

4、**实现面积计算函数**：

- calculateArea 函数接受一个 Shape 类型的参数，通过 switch 语句检查 type 属性，根据不同的形状类型执行相应的面积计算。
- 使用类型断言（Type Assertion）将 Shape 类型的参数转换为具体的形状类型（Circle 或 Rectangle），从而访问特定属性。

通过这种方式，我们使用枚举来创建判别联合类型，使得 calculateArea 函数能够根据 type 属性进行分支处理，确保类型安全并执行正确的计算。

# **五、使用枚举作为数据结构**

这个 TypeScript 示例展示了如何使用枚举来表示扑克牌的花色、等级以及根据花色派生的颜色属性。代码包括两个枚举、一个获取牌值的函数、一个描述牌结构的接口，以及一个创建牌的函数。

![1719050848983](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1719050848983.png)

```js
enum Suit {
  Hearts = "♥", // 红色花色
  Diamonds = "♦", // 红色花色
  Clubs = "♣", // 黑色花色
  Spades = "♠" // 黑色花色
}

enum Rank {
  Ace = 1,
  Two, Three, Four, Five, Six, Seven, Eight, Nine, Ten,
  Jack, Queen, King 
}

function getCardValue(rank: Rank): number {
  if (rank <= Rank.Ten) {
    return rank;
  } else {
    return 10;
  }
}

interface Card {
  suit: Suit;
  rank: Rank;
  color: string; // 根据花色派生的属性
}

function createCard(suit: Suit, rank: Rank): Card {
  return {
    suit,
    rank,
    color: suit === Suit.Hearts || suit === Suit.Diamonds ? 'Red' : 'Black'
  }
}

// 使用示例
let card1 = createCard(Suit.Hearts, Rank.Ace);
console.log(`The Ace of Hearts is red: ${card1.color}`); // 输出：The Ace of Hearts is red: Red

let card2 = createCard(Suit.Spades, Rank.Queen);
console.log(`The Queen of Spades is black: ${card2.color}`); // 输出：The Queen of Spades is black: Black

```

## **解释**

1、**定义枚举**：

- Suit 枚举定义了扑克牌的四种花色，每种花色都有一个符号表示。
- Rank 枚举定义了扑克牌的等级，从 Ace 到 King。

2、**获取牌值的函数**：

- getCardValue 函数接受一个 Rank 类型的参数，并返回该牌的数值。对于 Ace 到 Ten，它们的数值等于等级本身。对于 Jack、Queen 和 King，它们的数值为 10。

3、**定义牌的接口**：

- Card 接口描述了一张牌的结构，包括花色、等级和颜色属性。颜色属性是根据花色派生的，红色花色（Hearts 和 Diamonds）为红色，黑色花色（Clubs 和 Spades）为黑色。

4、**创建牌的函数**：

- createCard 函数接受花色和等级作为参数，并返回一个 Card 对象。该函数根据花色来设置颜色属性。

这个示例展示了如何使用 TypeScript 的枚举和接口来创建一个简单的扑克牌模型。通过枚举，我们可以确保花色和等级的类型安全，通过接口，我们可以定义牌的结构，使代码更加清晰和易于维护。

**推荐阅读**

- [让你的TypeScript代码更优雅，这10个特性你需要了解下](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517609&idx=1&sn=a2338d43db681096cdf5908512ddab73&chksm=a6a6961591d11f035b51d8d470a7d46e42fba6470322de43d815c47bb29bb3ff3902d7c287d5&scene=21#wechat_redirect)
- [在 TypeScript 中，定义类型时你用 Types 还是 Interfaces？](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517651&idx=1&sn=5d5c8fc61825d203d8d3db40409555db&chksm=a6a6966f91d11f795984dae1173152b48f20331e13152f41cfb28d09b78cf3b702d47007a0d8&scene=21#wechat_redirect)
- [深入理解 TypeScript 中的 Keyof 运算符，让你的代码更安全、更灵活！](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517713&idx=1&sn=1854ca6ff3341f6ebd38b5bfd2347844&chksm=a6a695ad91d11cbb9631c82bdef4ebac3db1ff0b4e658d40f843bdec242fb75db12d18f0b639&scene=21#wechat_redirect)
- [一文搞懂TypeScript泛型，让你的组件复用性大幅提升](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517742&idx=1&sn=a454c60a33eb067862fa6c5d4c140130&chksm=a6a6959291d11c846367dfcd460a1cf454ed0cef7cb321739f1da66b681ad68d8bfa25287181&scene=21#wechat_redirect)
- [如何利用 TypeScript 的 Extract 提升类型定义与代码清晰度](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517800&idx=1&sn=4e49aed00ea1bfa8d90810bd5eabcfc3&chksm=a6a695d491d11cc296deaed1cf38ac0543c6ae5d42348ef08ae947d3f380084e1677862cd984&scene=21#wechat_redirect)
- [如何利用 TypeScript 的 Exclude 提升状态管理与代码健壮性](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517819&idx=2&sn=5abf6d03bd736309c31c9e9dc21b346d&chksm=a6a695c791d11cd155d2b5fdb3cdbbb23fded34af63d263047013c9287c91cba8aec27c29c9e&scene=21#wechat_redirect)
- [TypeScript 进阶，深入理解并运用索引访问类型提升代码质量](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517828&idx=1&sn=27e9a4acd5a7785ceeecb61ed0c1b104&chksm=a6a6953891d11c2eff49b95bd28d87493f70f2d03bb2d6b88546a96ee6dec78a562690ce8d41&scene=21#wechat_redirect)
- [如何利用 TypeScript 的判别联合类型提升错误处理与代码安全性](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517857&idx=1&sn=cefb4b5b1a6ff9b669ae68fc64a21adb&chksm=a6a6951d91d11c0b5bf7fb66af87f416b6960e942645ecc6d4a942a07564b2b658c92ac660aa&scene=21#wechat_redirect)
- [探索TypeScript的映射类型，从简单到高级的7个实例](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517897&idx=1&sn=977123153d0330b0a5de490fdfeb26a3&chksm=a6a6957591d11c63940a97c8e728ff8910a8dbf2364a0c3e676e88d85fdc056f8be3c5e24a09&scene=21#wechat_redirect)
- [深入解析 TypeScript 索引签名：通过 4 个实例轻松掌握](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517921&idx=1&sn=8fc13e275d5be5e166edc6b76f58bcb7&chksm=a6a6955d91d11c4b2ec37b2656cf74936a233b1f7760bb39a5e2477ccab133ff3ac0c8f0b4fe&scene=21#wechat_redirect)
- [通过三个实例掌握如何使用 TypeScript 泛型创建可重用的 React 组件](http://mp.weixin.qq.com/s?__biz=MjM5MjU2NDk0Nw==&mid=2247517956&idx=1&sn=83548dd1e738fb9a78b2eaca6a29ebce&chksm=a6a694b891d11daecf7084dc0ade93edfd8c465caa2bbb2da61f2ef9c2f24547d8498b5a1e05&scene=21#wechat_redirect)