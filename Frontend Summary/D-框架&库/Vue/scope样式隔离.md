# vue中scope 是怎么做的样式隔离的？

在 Vue.js 中，`scoped` 样式是一种实现组件样式隔离的机制。它确保了一个组件的样式不会影响到其他组件的样式，从而避免了全局样式污染。下面是 Vue.js 中 `scoped` 样式的工作原理：

### 1. **基本使用**

在 Vue 单文件组件（`.vue` 文件）中，你可以通过在 `<style>` 标签上添加 `scoped` 属性来启用样式隔离：

```
<template>
  <div class="example">
    Scoped style example
  </div>
</template>

<style scoped>
.example {
  color: red;
}
</style>
```

### 2. **工作原理**

当你使用 `scoped` 样式时，Vue.js 会在编译时自动处理样式以确保它们只应用于当前组件。具体做法如下：

- **自动添加属性选择器**:

- - Vue.js 会为组件生成一个唯一的标识符，并将这个标识符作为属性选择器添加到样式规则中。

  - 生成的样式会被自动转换成类似以下的形式：

    ```
    .example[data-v-123456] {
      color: red;
    }
    ```

- **将标识符添加到 DOM 元素**:

- - 在组件的 HTML 元素上，Vue.js 会自动添加一个 `data-v-<hash>` 属性，这个属性值对应于样式中生成的唯一标识符。

    ```
    <div class="example" data-v-123456>
      Scoped style example
    </div>
    ```

- **样式选择器的作用范围**:

- - 由于样式选择器中包含了 `data-v-<hash>` 属性选择器，样式规则仅会应用到带有相应 `data-v-<hash>` 属性的元素上，确保样式只影响到当前组件的 DOM。

### 3. **样式继承**

- **子组件的样式**:

- - `scoped` 样式不会穿透到子组件或父组件。子组件的样式也需要添加 `scoped` 属性来实现隔离。

- **深度选择器**:

- - 如果你需要在 `scoped` 样式中选择子组件的样式，你可以使用深度选择器 `>>>` 或 `::v-deep`。例如：

    ```
    <style scoped>
    .parent >>> .child {
      color: blue;
    }
    </style>
    ```

    这将允许 `.parent` 选择器影响到 `.child` 的样式，但注意这样做会降低样式隔离的严格性。

### 4. **注意事项**

- **全局样式**:

- - 如果需要全局样式，可以不使用 `scoped` 属性，直接在全局样式文件中定义，或在 `<style>` 标签中省略 `scoped`。

- **动态样式**:

- - `scoped` 样式不会影响通过 JavaScript 动态添加的样式。这些样式需要单独处理。

### 示例

```
<template>
  <div>
    <p class="scoped-text">This is scoped text.</p>
  </div>
</template>

<style scoped>
.scoped-text {
  color: green;
}
</style>
```

在这个例子中，`.scoped-text` 类样式只会应用于当前组件的 `.scoped-text` 元素，不会影响其他组件中的同名类。

### 总结

Vue.js 的 `scoped` 样式通过为组件生成唯一的标识符，并将这些标识符应用到样式选择器和 DOM 元素上，从而实现了组件之间的样式隔离。这种机制帮助开发者避免样式冲突，并使组件更加模块化和可维护。