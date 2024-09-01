## **vue3 中的 hooks 是什么?**

简单来说如果你的函数中用到了诸如 ref,reactive,onMounted 等 vue 提供的 api 的话,那么它就是一个 hooks 函数,如果没用到它就是一个普通工具函数。至于它为什么叫 hooks,我的理解则是

> 它可以通过特定的函数将逻辑 "钩入" 组件中，使得开发者能够更灵活地构建和管理组件的功能从而提高代码的可读性以及可维护性等

## **在 vue3 中使用**

上面说到 hooks 函数里包含了 vue 提供的 api,下面我们就简单的来举个例子看一下 vue3 中的 hooks 函数。一般来说,如果一个你得函数为 hooks 函数,那么你需要将其以 use 开头命名。在 src 下新建一个 hooks 目录专门存放 hooks 函数,然后写下第一个非常简单的 hooks 函数 useAdd

```js
import { ref } from "vue";
export const useAdd = () => {
  const a = ref(1);
  setInterval(() => {
    a.value++;
  }, 1000);
  return a;
};
```

这是一个非常简单的 hooks 函数,每隔一秒就让 a.value 加 1,最后返回一个响应式的 a,我们在组件中引用一下

```js
<template>
  <div>{{ a }}</div>
</template>

<script lang='ts' setup>
import { useAdd } from "../hooks/useAdd"
const a = useAdd()
</script>
```

此时我们会看到每隔一秒页面上的值就会加 1,所以说 a 还是保持了它的响应式特性

## **mixin 与 hooks**

我们都知道 Vue 3 引入 `Composition API`的写法,当我们引入一个 hooks 函数的时候其实就像在 vue2 中使用一个 mixin 一样,hooks 函数中的`ref`,`reactive`就相当于 mixin 中的`data`,同时 hooks 还可以引入一些生命周期函数,watch 等在 mixin 中都有体现。下面展示一下 mixin 的写法,这里不过多的讲解 mixin

```js
export const mixins = {
  data() {
    return {
      msg: "",
    };
  },
  computed: {},
  created() {
    console.log("我是mixin中的created生命周期函数");
  },
  mounted() {
    console.log("我是mixin中的mounted生命周期函数");
  },
  methods: {
    clickMe() {
      console.log("我是mixin中的点击事件");
    },
  },
};
```

组件中引入

```js
export default {
  name: "App",
  mixins: [mixins],
  components: {},
  created() {
    console.log("组件调用minxi数据", this.msg);
  },
  mounted() {
    console.log("我是组件的mounted生命周期函数");
  },
};
```

用过 vue2 的 mixin 的都知道它虽然可以封装一些逻辑,但是它同时也带来了一些问题.比如你引入多个 mixin 它们的 data,methods 命名可能会冲突,当 mixin 多了可能会出现维护性问题,另外 mixin 不是一个函数,因此不能传递参数来改变它的逻辑,具有一定的局限性等,但这些问题到了 vue3 的 hooks 中则迎刃而解

## **hooks 中生命周期执行顺序**

hooks 中生命周期与组件中的生命周期执行顺序其实很好判断,就看它们谁的同级生命周期函数先创建那就先执行谁的,比如在 useAdd 中加几个生命周期

```js
import { ref, onMounted, onBeforeUnmount, onUnmounted } from "vue";
export const useAdd = () => {
  const a = ref(1);
  setInterval(() => {
    a.value++;
  }, 1000);
  onMounted(() => {
    console.log("hooks---onMounted");
  });
  onBeforeUnmount(() => {
    console.log("hooks---onMounted");
  });
  onUnmounted(() => {
    console.log("hooks---onUnmounted");
  });

  return a;
};
```

然后在组件中也引入这些生命周期

```js
<template>
  <div>{{ a }}</div>
</template>

<script lang='ts' setup>
import { useAdd } from "../hooks/useTest"
import { onMounted, onBeforeUnmount, onUnmounted } from "vue";

onMounted(() => {
  console.log("组件---onMounted");
});
onBeforeUnmount(() => {
  console.log("组件---onMounted");
});
onUnmounted(() => {
  console.log("组件---onUnmounted");
});
const a = useAdd()
</script>

```

如果将 hooks 放到最后那么它们的顺序是这样的

![1712480189889](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480189889.png)

如果放到前面那就是这样的

![1712480202607](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1712480202607.png)

# 在vue3中如何编写一个标准的hooks？

## **前言**

在 Vue 3 中，组合式 API 为开发者提供了更加灵活和高效的方式来组织和复用逻辑，其中 Hooks 是一个重要的概念。Hooks 允许我们将组件中的逻辑提取出来，使其更具可复用性和可读性，让我们的代码编写更加灵活。

## **hooks的定义**

其实，事实上官方并未管这种方式叫做**hooks**，而似乎更应该叫做compositions更加确切些，更加符合vue3的设计初衷。由于react的hooks设计理念在前，而vue3的组合式使用也像一个hook钩子挂载vue框架的生命周期中，对此习惯性地称作hooks。

对于onMounted、onUnMounted等响应式API都必须在setup阶段进行同步调用。

![1724493095685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724493095685.png)

要理解 Vue 3 中的 Hooks，需要明白它的本质是一个函数，这个函数可以包含与组件相关的状态和副作用操作。

- 状态是应用中存储的数据，这些数据可以影响组件的外观和行为。在 Vue 3 中，可以使用 `ref` 和 `reactive` 来创建状态。
- 副作用操作是指在应用执行过程中会产生外部可观察效果的操作，比如数据获取、订阅事件、定时器等。这些操作可能会影响应用的状态或与外部系统进行交互。

记住：hooks就是特殊的函数，可以在vue组件外部使用，可以访问vue的响应式系统。

## **vue3中hooks和react的区别**

vue3的compositions和react的hooks还是有所区别的，对此官方还特别写了两者的比较，原文如下：

![1724493121636](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1724493121636.png)

大抵意思如下，Vue Composition API 与 React Hooks 都具有逻辑组合能力，但存在一些重要差异。

**React Hooks 的问题**：

- 每次组件更新都会重复调用，存在诸多注意事项，可能使经验丰富的开发者也感到困惑，并导致性能优化问题。
- 对调用顺序敏感且不能有条件调用。
- 变量可能因依赖数组不正确而“过时”，开发者需依赖 ESLint 规则确保正确依赖，但规则不够智能，可能过度补偿正确性，遇到边界情况会很麻烦。
- 昂贵的计算需使用 `useMemo`，且要手动传入正确依赖数组。
- 传递给子组件的事件处理程序默认会导致不必要的子组件更新，需要显式使用 `useCallback` 和正确的依赖数组，否则可能导致性能问题。陈旧闭包问题结合并发特性，使理解钩子代码何时运行变得困难，处理跨渲染的可变状态也很麻烦。

**Vue Composition API 的优势**：

- `setup()` 或 `<script setup>` 中的代码仅执行一次，不存在陈旧闭包问题，调用顺序不敏感且可以有条件调用。
- Vue 的运行时响应式系统自动收集计算属性和监听器中使用的响应式依赖，无需手动声明依赖。
- 无需手动缓存回调函数以避免不必要的子组件更新，精细的响应式系统确保子组件仅在需要时更新，手动优化子组件更新对 Vue 开发者来说很少是问题。

## **自定义hooks需要遵守的原则**

那么，在编写自定义Hooks时，有哪些常见的错误或者陷阱需要避免？

以下是一些需要注意的点：

1. **状态共享问题**：不要在自定义Hooks内部创建状态（使用`ref`或`reactive`），除非这些状态是暴露给使用者的API的一部分。Hooks应该是无状态的，避免在Hooks内部保存状态。
2. **副作用处理不当**：副作用（例如API调用、定时器等）应该在生命周期钩子（如`onMounted`、`onUnmounted`）中处理。不要在自定义Hooks的参数处理或逻辑中直接执行副作用。
3. **过度依赖外部状态**：自定义Hooks应尽量减少对外部状态的依赖。如果必须依赖，确保通过参数传递，而不是直接访问组件的状态或其他全局状态。
4. **参数验证不足**：自定义Hooks应该能够处理无效或意外的参数。添加参数验证逻辑，确保Hooks的鲁棒性。
5. **使用不稳定的API**：避免使用可能在未来版本中更改或删除的API。始终查阅官方文档，确保你使用的API是稳定的。
6. **性能问题**：避免在自定义Hooks中进行昂贵的操作，如深度比较或复杂的计算，这可能会影响组件的渲染性能。
7. **重渲染问题**：确保自定义Hooks不会由于响应式依赖不当而导致组件不必要的重渲染。
8. **命名不一致**：自定义Hooks应该遵循一致的命名约定，通常是`use`前缀，以便于识别和使用。
9. **过度封装**：避免创建过于通用或复杂的Hooks，这可能会导致难以理解和维护的代码。Hooks应该保持简单和直观。
10. **错误处理不足**：自定义Hooks应该能够妥善处理错误情况，例如API请求失败或无效输入。
11. **生命周期钩子滥用**：不要在自定义Hooks中滥用生命周期钩子，确保只在必要时使用。
12. **不遵循单向数据流**：Hooks应该遵循Vue的单向数据流原则，避免创建可能导致数据流混乱的逻辑。
13. **忽视类型检查**：使用TypeScript编写Hooks时，确保进行了适当的类型检查和类型推断。
14. **使用不恰当的响应式API**：例如，使用`ref`而不是`reactive`，或者在应该使用`readonly`的场景中使用了可变对象。
15. **全局状态管理不当**：如果你的Hooks依赖于全局状态，确保正确处理，避免造成状态管理上的混乱。

## **我们自定义一个hooks方法**

记住这些军规后，我们尝试自己写一个自定义hooks函数。下面代码实现了一个自定义的钩子函数，用于处理组件的事件监听和卸载逻辑，以达到组件逻辑的封装和复用目的。

```
import { ref, onMounted, onUnmounted } from 'vue';

function useEventListener(eventType, listener, options = false) {
  const targetRef = ref(null);

  onMounted(() => {
    const target = targetRef.value;
    if (target) {
      target.addEventListener(eventType, listener, options);
    }
  });

  onUnmounted(() => {
    const target = targetRef.value;
    if (target) {
      target.removeEventListener(eventType, listener, options);
    }
  });

  return targetRef;
}
```

对于简单的数字累加自定义hooks方法，我们可以这样写：

```
import { ref } from 'vue';

function useCounter(initialValue = 0) {
  const count = ref(initialValue);
  const increment = () => {
    count.value++;
  };

  return { count, increment };
}
```

编写单元测试来验证你的自定义Hooks是否按预期工作：

```
import { mount } from '@vue/test-utils';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('should increment count', () => {
    const { count, increment } = useCounter();
    increment();
    expect(count.value).toBe(1);
  });
});
```

使用hooks：

```
<template>
  <div>{{ count }}</div>
</template>

<script setup>
import { useCounter } from './useCounter';

const { count } = useCounter(10);
</script>
```

## **hooks工具库vueuse和vue-hooks-plus**

对于常用的hooks方法可以单独抽取进行发包成hooks工具。在业务开发中常用的vue hooks方法库有：vueuse和vue-hooks-plus。那么，咱们看看这两个库对于useCounter的封装是什么样的。

vueuse：

```
// eslint-disable-next-line no-restricted-imports
import { ref, unref } from 'vue-demi'

import type { MaybeRef } from 'vue-demi'

export interface UseCounterOptions {
  min?: number
  max?: number
}

/**
 * Basic counter with utility functions.
 *
 * @see https://vueuse.org/useCounter
 * @param [initialValue]
 * @param options
 */
export function useCounter(initialValue: MaybeRef<number> = 0, options: UseCounterOptions = {}) {
  let _initialValue = unref(initialValue)
  const count = ref(initialValue)

  const {
    max = Number.POSITIVE_INFINITY,
    min = Number.NEGATIVE_INFINITY,
  } = options

  const inc = (delta = 1) => count.value = Math.min(max, count.value + delta)
  const dec = (delta = 1) => count.value = Math.max(min, count.value - delta)
  const get = () => count.value
  const set = (val: number) => (count.value = Math.max(min, Math.min(max, val)))
  const reset = (val = _initialValue) => {
    _initialValue = val
    return set(val)
  }

  return { count, inc, dec, get, set, reset }
}

```

vue-hooks-plus：

```
import { Ref, readonly, ref } from 'vue'
import { isNumber } from '../utils' // export const isNumber = (value: unknown): value is number => typeof value === 'number'

export interface UseCounterOptions {
  /**
   *  Min count
   */
  min?: number

  /**
   *  Max count
   */
  max?: number
}

export interface UseCounterActions {
  /**
   * Increment, default delta is 1
   * @param delta number
   * @returns void
   */
  inc: (delta?: number) => void

  /**
   * Decrement, default delta is 1
   * @param delta number
   * @returns void
   */
  dec: (delta?: number) => void

  /**
   * Set current value
   * @param value number | ((c: number) => number)
   * @returns void
   */
  set: (value: number | ((c: number) => number)) => void

  /**
   * Reset current value to initial value
   * @returns void
   */
  reset: () => void
}

export type ValueParam = number | ((c: number) => number)

function getTargetValue(val: number, options: UseCounterOptions = {}) {
  const { min, max } = options
  let target = val
  if (isNumber(max)) {
    target = Math.min(max, target)
  }
  if (isNumber(min)) {
    target = Math.max(min, target)
  }
  return target
}

function useCounter(
  initialValue = 0,
  options: UseCounterOptions = {},
): [Ref<number>, UseCounterActions] {
  const { min, max } = options

  const current = ref(
    getTargetValue(initialValue, {
      min,
      max,
    }),
  )

  const setValue = (value: ValueParam) => {
    const target = isNumber(value) ? value : value(current.value)
    current.value = getTargetValue(target, {
      max,
      min,
    })
    return current.value
  }

  const inc = (delta = 1) => {
    setValue(c => c + delta)
  }

  const dec = (delta = 1) => {
    setValue(c => c - delta)
  }

  const set = (value: ValueParam) => {
    setValue(value)
  }

  const reset = () => {
    setValue(initialValue)
  }

  return [
    readonly(current),
    {
      inc,
      dec,
      set,
      reset,
    },
  ]
}

export default useCounter
```

两段代码都在代码实现上都遵守了上面的hook军规，实现了相似的功能，即创建一个可复用的计数器模块，具有增加、减少、设置特定值和重置等操作，并且都可以配置最小和最大计数范围。

**差异点**

1. **代码细节**：

- 第一段代码使用了`unref`函数来获取初始值的实际数值，第二段代码没有使用这个函数，而是直接在初始化响应式变量时进行处理。
- 第二段代码引入了一个辅助函数`isNumber`和`getTargetValue`来确保设置的值在合法范围内，第一段代码在设置值的时候直接进行范围判断，没有单独的辅助函数。

1. **返回值处理**：

- 第二段代码返回的响应式变量是只读的，这可以提高代码的安全性，防止在组件中意外修改计数器的值；第一段代码没有对返回的响应式变量进行只读处理。

## **那么什么场景下需要抽取hooks呢？**

在以下几种情况下，通常需要抽取 Hooks 方法：

**1.逻辑复用**
当多个组件中存在相同或相似的逻辑时，抽取为 Hooks 可以提高代码的复用性。
例如，在多个不同的页面组件中都需要进行数据获取和状态管理，如从服务器获取用户信息并显示加载状态、错误状态等。可以将这些逻辑抽取为一个`useFetchUser`的 Hooks 方法，这样不同的组件都可以调用这个方法来获取用户信息，避免了重复编写相同的代码。

**2.复杂逻辑的封装**
如果某个组件中有比较复杂的业务逻辑，将其抽取为 Hooks 可以使组件的代码更加清晰和易于维护。
比如，一个表单组件中包含了表单验证、数据提交、错误处理等复杂逻辑。可以将这些逻辑分别抽取为`useFormValidation`、`useSubmitForm`、`useFormErrorHandling`等 Hooks 方法，然后在表单组件中组合使用这些 Hooks，使得表单组件的主要逻辑更加专注于用户界面的呈现，而复杂的业务逻辑被封装在 Hooks 中。

**3.与特定功能相关的逻辑**
当有一些特定的功能需要在多个组件中使用时，可以抽取为 Hooks。
例如，实现一个主题切换功能，需要管理当前主题状态、切换主题的方法以及保存主题设置到本地存储等逻辑。可以将这些逻辑抽取为`useTheme` Hooks 方法，方便在不同的组件中切换主题和获取当前主题状态。

**4.提高测试性**
如果某些逻辑在组件中难以进行单元测试，可以将其抽取为 Hooks 以提高测试性。
比如，一个组件中的定时器逻辑可能与组件的生命周期紧密耦合，难以单独测试。将定时器相关的逻辑抽取为`useTimer` Hooks 方法后，可以更容易地对定时器的行为进行单元测试，而不依赖于组件的其他部分。

总之，抽取 Hooks 方法可以提高代码的复用性、可维护性和测试性，当遇到上述情况时，考虑抽取 Hooks 是一个很好的实践。

## **案例：vue-vben-admin中的usePermission**

我们看看关于在业务开发中如何进行hooks抽取封装的案例，vue-vben-admin（https://github.com/vbenjs/vue-vben-admin）是个优秀的中后台管理项目，在项目中设计很复杂也很全面，很多地方都充分体现了vue3的设计思想，也能窥见作者对于vue3源码的深入。

```
import type { RouteRecordRaw } from 'vue-router';

import { useAppStore } from '/@/store/modules/app';
import { usePermissionStore } from '/@/store/modules/permission';
import { useUserStore } from '/@/store/modules/user';

import { useTabs } from './useTabs';

import { router, resetRouter } from '/@/router';
// import { RootRoute } from '/@/router/routes';

import projectSetting from '/@/settings/projectSetting';
import { PermissionModeEnum } from '/@/enums/appEnum';
import { RoleEnum } from '/@/enums/roleEnum';

import { intersection } from 'lodash-es';
import { isArray } from '/@/utils/is';
import { useMultipleTabStore } from '/@/store/modules/multipleTab';

// User permissions related operations
export function usePermission() {
  const userStore = useUserStore();
  const appStore = useAppStore();
  const permissionStore = usePermissionStore();
  const { closeAll } = useTabs(router);

  /**
   * Change permission mode
   */
  async function togglePermissionMode() {
    appStore.setProjectConfig({
      permissionMode:
        appStore.projectConfig?.permissionMode === PermissionModeEnum.BACK
          ? PermissionModeEnum.ROUTE_MAPPING
          : PermissionModeEnum.BACK,
    });
    location.reload();
  }

  /**
   * Reset and regain authority resource information
   * 重置和重新获得权限资源信息
   * @param id
   */
  async function resume() {
    const tabStore = useMultipleTabStore();
    tabStore.clearCacheTabs();
    resetRouter();
    const routes = await permissionStore.buildRoutesAction();
    routes.forEach((route) => {
      router.addRoute(route as unknown as RouteRecordRaw);
    });
    permissionStore.setLastBuildMenuTime();
    closeAll();
  }

  /**
   * Determine whether there is permission
   */
  function hasPermission(value?: RoleEnum | RoleEnum[] | string | string[], def = true): boolean {
    // Visible by default
    if (!value) {
      return def;
    }

    const permMode = projectSetting.permissionMode;

    if ([PermissionModeEnum.ROUTE_MAPPING, PermissionModeEnum.ROLE].includes(permMode)) {
      if (!isArray(value)) {
        return userStore.getRoleList?.includes(value as RoleEnum);
      }
      return (intersection(value, userStore.getRoleList) as RoleEnum[]).length > 0;
    }

    if (PermissionModeEnum.BACK === permMode) {
      const allCodeList = permissionStore.getPermCodeList as string[];
      if (!isArray(value)) {
        return allCodeList.includes(value);
      }
      return (intersection(value, allCodeList) as string[]).length > 0;
    }
    return true;
  }

  /**
   * Change roles
   * @param roles
   */
  async function changeRole(roles: RoleEnum | RoleEnum[]): Promise<void> {
    if (projectSetting.permissionMode !== PermissionModeEnum.ROUTE_MAPPING) {
      throw new Error(
        'Please switch PermissionModeEnum to ROUTE_MAPPING mode in the configuration to operate!',
      );
    }

    if (!isArray(roles)) {
      roles = [roles];
    }
    userStore.setRoleList(roles);
    await resume();
  }

  /**
   * refresh menu data
   */
  async function refreshMenu() {
    resume();
  }

  return { changeRole, hasPermission, togglePermissionMode, refreshMenu };
}
```

这段代码实现了一个与权限管理相关的模块，主要用于在 Vue 应用中处理用户权限、切换权限模式、重新获取权限资源信息以及刷新菜单等操作。

**主要结构和组成部分**

1. **引入依赖**：

2. - 引入了`RouteRecordRaw`类型，用于表示路由记录。
   - 从特定路径引入了应用的`store`模块，包括`useAppStore`、`usePermissionStore`和`useUserStore`，用于管理应用状态。
   - 引入了自定义的`useTabs`函数，用于处理标签页相关操作。
   - 引入了`router`和`resetRouter`，用于操作路由。
   - 引入了一些项目设置和工具函数，如`projectSetting`、`PermissionModeEnum`、`RoleEnum`、`intersection`和`isArray`。

3. **定义**`**usePermission**`**函数**：

4. - 该函数内部获取了用户存储、应用存储和权限存储的实例，并调用了`useTabs`函数获取标签页操作方法。
   - `togglePermissionMode`方法：用于切换权限模式，通过更新应用存储中的项目配置，然后重新加载页面。
   - `resume`方法：用于重置和重新获取权限资源信息。它先清除多标签页存储中的缓存标签，重置路由，重新构建路由并添加到路由实例中，设置最后构建菜单的时间，并关闭所有标签页。
   - `hasPermission`方法：用于判断用户是否具有特定的权限。根据不同的权限模式，检查用户的角色列表或权限代码列表是否包含给定的值。
   - `changeRole`方法：用于切换用户角色。如果当前权限模式不是`ROUTE_MAPPING`，则抛出错误。如果角色不是数组，则转换为数组，然后更新用户存储中的角色列表，并调用`resume`方法重新获取权限资源信息。
   - `refreshMenu`方法：用于刷新菜单数据，实际上是调用了`resume`方法。

5. **返回值**：

6. - `usePermission`函数最后返回一个包含`changeRole`、`hasPermission`、`togglePermissionMode`和`refreshMenu`方法的对象。

## **总结**

本文主要介绍了 Vue 3 中的组合式 API 及 Hooks 相关内容。首先说明了 Vue 3 组合式 API 中 Hooks 的概念、作用及与 React Hooks 的区别，指出 Vue Composition API 的优势。接着详细阐述了编写自定义 Hooks 时应避免的错误和陷阱，如状态共享、副作用处理、过度依赖外部状态等问题，并给出了自定义 Hooks 函数的示例及单元测试方法。然后对比了两个库（vueuse 和 vue-hooks-plus）对 `useCounter` 的封装差异。还探讨了抽取 Hooks 的场景，如逻辑复用、复杂逻辑封装等，并以 `vue-vben-admin` 项目中的权限管理模块为例进行分析。

**参考素材：**

- https://router.vuejs.org/
- https://inhiblabcore.github.io/docs/hooks/
- https://vueuse.org/
- https://juejin.cn/post/7083401842733875208

# Vue3新篇章：VueHooks Plus～！

在Vue3的浪潮中，Composition API凭借其灵活而强大的特性，已成为前端开发领域的一股新潮流。然而，对于从React转战Vue3的开发者而言，他们可能会发现现有的Hooks库，如VueUse，并不完全满足他们的需求。今天，我们来探讨一个更具吸引力的替代方案——VueHooks Plus。

![1725191501803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1725191501803.png)

***从React Hooks到Vue Hooks***

> 在React中，ahooks是我经常使用的库，它丰富的功能使我在业务开发中如鱼得水。然而，在Vue3中，我发现VueUse的useFetch和useAxios功能相对单一，缺乏一些如防抖、节流、轮询、缓存及SWR（stale-while-revalidate）等高级功能。**VueHooks Plus它就完美的解决了这些问题。**

***VueHooks Plus：基于Vue3特性的Hooks库***

> VueHooks Plus是一个基于Vue3特性开发的Hooks库，它不仅涵盖了ahooks的所有功能，还提供了更多针对Vue的优化。以useRequest Hooks为例，VueHooks Plus提供了诸多高级功能，包括：
>
> 自动请求/手动请求：通过参数化配置控制请求的自动或手动触发。
>
> 轮询：支持轮询模式，可定时触发请求。
>
> 防抖：提供参数化配置，可进入防抖模式。
>
> 节流：同样提供参数化配置，可进入节流模式。
>
> 屏幕聚焦重新请求：当浏览器窗口重新获得焦点或可见时，会自动重新发起请求。
>
> 错误重试：通过参数配置指定错误重试次数，useRequest在失败后会进行重试。
>
> loading delay：延迟loading状态变为true的时间，有效防止界面闪烁。
>
> 缓存：将请求成功的数据进行缓存。
>
> SWR（stale-while-revalidate）：优先返回缓存数据，并在背后发送新请求以更新数据。
>
> 滚动加载和分页加载：提供滚动加载和分页加载的能力。
>
> 并行请求：赋予useRequest并行请求的能力。

***插件化能力支持***

> 此外，useRequest还支持外置扩展插件的功能，可以根据自身需求自定义插件。目前官方已提供的插件能力包括：
>
> 全局请求状态管理插件：充当所有请求的状态中间态，用户可以在中间态中对收集的请求结果进行操作。
>
> 同源跨窗口广播插件：实现相同来源的浏览器选项卡/窗口之间的数据广播和同步。

***VueHooks Plus：更全面的Vue3解决方案，使用简单快捷***

npm一键安装，轻松上手：

```
npm i vue-hooks-plus
```

提供多种灵活的引入方式，满足不同需求：

**全量引入**：如果您希望使用VueHooks Plus提供的所有功能，可以简单地进行全量引入：

```
import { useRequest } from 'vue-hooks-plus';
```

**按需引入**：为了优化您的项目体积，我们也支持按需引入，只引入您需要的部分：

```
import useRequest from 'vue-hooks-plus/es/useRequest';
```

**自动引入**：配合unplugin-auto-import插件，您可以实现VueHooks Plus的自动引入，进一步提升开发效率。首先安装插件和解析器：

```
npm i -D @vue-hooks-plus/resolvers unplugin-auto-import
```

然后在您的配置文件中加入以下代码：

```
import AutoImport from 'unplugin-auto-import/vite';  import { VueHooksPlusResolver } from '@vue-hooks-plus/resolvers';  
export const AutoImportDeps = () =>    AutoImport({      imports: ['vue', 'vue-router'],      include: [/\.[tj]sx?$/, /\.vue$/, /\.vue\?vue/, /\.md$/],      dts: 'src/auto-imports.d.ts',      resolvers: [VueHooksPlusResolver()],    });
```

VueHooks Plus 为我们提供了一个功能全面、灵活且易于使用的 Hooks 解决方案。如果你正在寻找一个能够提升开发效率和代码质量的工具，那么 VueHooks Plus 绝对值得一试。



> 参考链接：官方文档：https://inhiblabcore.github.io/docs/hooks/
>
> https://inhiblabcore.github.io/docs/hooks/

