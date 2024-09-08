# Symbol在项目中使用

主要用到*Symbol*两个特性：

1. 每次使用的值都是唯一的
2. *Symbol*值作为属性名称时，不能被遍历

# **唯一值**

例子：前端维护一个*todoList*，每个*todoItem*需要唯一*Id*，点击删除按钮，根据*todoItem* *id*查找索引删除

```
<template>
 <div>
   <el-row :gutter="20">
     <el-col :span="15">
       <el-input v-model="newTodo" placeholder="添加新的待办事项"></el-input>
     </el-col>
     <el-col :span="8">
       <el-button @click="addTodo" type="primary">添加待办事项</el-button>
     </el-col>
   </el-row>
   <el-divider></el-divider>
   <el-table :data="todos" style="width: 100%">
     <el-table-column prop="title" label="待办事项"></el-table-column>
     <el-table-column label="操作">
       <template slot-scope="scope">
         <el-button @click="removeTodo(scope.row.id)" type="text" size="medium"
           >移除</el-button
         >
       </template>
     </el-table-column>
   </el-table>
 </div>
</template>

<script>
export default {
 data() {
   return {
     newTodo: "",
     todos: [],
   };
 },
 methods: {
   addTodo() {
     if (this.newTodo.trim() !== "") {
       this.todos.push({
           id: Date.now(),
           title: this.newTodo,
       });
       this.newTodo = "";
     }
   },
   removeTodo(id) {
     this.todos.splice(this.todos.findIndex((todo) => todo.id === id), 1);
   },
 },
};
</script>
```

![1724496363435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496363435.png)

当添加*todoItem*时，需要前端给这个*todoItem*生成一个**唯一** *id*

放在以前，你可能会想到时间戳，也就是上面代码中的做法

```
this.todos.push({
   id: Date.now(),
   title: this.newTodo,
});
```

确实，`Date.now()`可以保证*todoItem*每个*id*都是**唯一**

![1724496492592](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496492592.png)

但是如果这里需要同时添加多条*todoItem*数据喃？

使用`Date.now()`就会变成这样

```
this.todos.push(
   ...Array.from(
       {
           length: 2,
       },
       () => ({
           id: Date.now(),
           title: this.newTodo,
       })
   )
);
```

![1724496549574](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496549574.png)

但是你会发现*todoItem*的*id*并不**唯一**了

这很简单，js同步执行时间比1ms还要快，所以两个时间戳是一样的

这个时候你再上网百度，网上说在时间戳后面拼接一个随机数

```
this.todos.push(
   ...Array.from(
       {
           length: 2,
       },
       () => ({
           id: Date.now() + "_" + Math.floor(Math.random() * 10),
           title: this.newTodo,
       })
   )
);
```

![1724496572219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496572219.png)

但是这样真的可以吗？如果同时添加10000个*todoItem*

![1724496608108](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496608108.png)

或许你觉得可以生成更长位数的随机数，但是无论生成多长的随机数都会有相等的可能，再长也不过是自欺欺人罢了！！！

当然使用*UUID*等唯一值库可以实现上面的需求，但是你是否想过，原生JS真的不能自己实现这个需求吗？

`Symbol()`就登场了，`Symbol()`产生的值永远是唯一的，无论同时添加多少个*todoItem*都不会产生重复*id*

```
this.todos.push(
   ...Array.from(
       {
           length: 1000,
       },
       () => ({
           id: Symbol(),
           title: this.newTodo,
       })
   )
);

function hasDuplicates(arr) {
   const ids = new Set();
   return arr.some((item) => ids.size === ids.add(item.id).size);
}

if (hasDuplicates(this.todos)) {
   console.error("有重复");
} else {
   console.log("没有重复");
}
```

无论是生成1000个还是100000个*todoItem*，*id*永远不会重复

# **私有属性**

例子：需要封装一个播放器类，在实例化时向外暴露播放和停止方法，通过这些方法去修改播放器类的*播放状态属性*，并且只允许调用方法修改这个*播放状态属性*，也就是说这个*播放状态属性*必须是**私有属性**，不能向外暴露，防止其他开发者不通过方法调用直接去修改*播放状态属性*，造成不必要的BUG

```
// Player.js

const Play_State = Symbol('Play_State');

export default class Player {

   [Play_State] = "Paused"
   
   commonField = "普通字段"

   play() {
       this[Play_State] = "Playing"
   }

   stop() {
       this[Play_State] = "Paused"
   }
}


// index.js

import Player from './Player'

let player = new Player()
```

![1724496642283](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496642283.png)

你可以会觉得可以通过`player[Symbol("Play_State")]`方式访问到属性值

![1724496655278](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496655278.png)

但是你会发现，最后返回值永远为空，这是因为每次调用`Symbol("Play_State")`都会生成唯一的属性名称

不能直接访问，那可以通过遍历对象访问到吗？

```
// for in
for (const key in player) {
 console.log(key, player[key])
}

// for of
for (const key of Object.keys(player)) {
 console.log(key, player[key])
}
```

![1724496670513](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496670513.png)

只能遍历出普通属性，但万事不是绝对的，JS还是提供了遍历*Symbol属性*的方法的

```
for (const key of Object.getOwnPropertySymbols(player)) {
 console.log(key, player[key])
}
```

![1724496693632](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496693632.png)

但这并不代表使用*Symbol属性*当作私有属性是错误的，毕竟一般开发也不会使用`Object.getOwnPropertySymbols`获取对象属性呀~~~

# **全局唯一**

在控制台输出一个空数组

![1724496710874](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496710874.png)

在数组的原型上面`Symbol(Symbol.iterator)`、`Symbol(Symbol.unscopables)`等方法

如何访问这些方法喃？直接`([]).[Symbol(Symbol.iterator)]`显然是不行的，和上面私有属性原理是一样的

要想了解如何访问这些方法，先在控制台打印一下*Symbol*

![1724496737242](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496737242.png)

`Symbol.iterator`的属性值好像和数组原型上面的`Symbol(Symbol.iterator)`方法名称是一样

验证一下

![1724496751207](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724496751207.png)

果然如我们所想，那么就可以想到JS编译器在数组上定义`Symbol(Symbol.iterator)`是如何实现的

```
Object.defineProperty(Symbol, "iterator", {
 value: Symbol("Symbol.iterator"),
});

Array.prototype[Symbol.iterator] = function () {}
```

这样做的好处是，以后无论在`Array.prototype`添加什么方法和属性，都不会干扰到`Symbol(Symbol.iterator)`方法