## Vue3![实例的生命周期](Vue3.assets/lifecycle.svg)





## vue2迁移至vue3



#### v-model

- 非兼容

  ：用于自定义组件时，`v-model` prop 和事件默认名称已更改：

  - prop：`value` -> `modelValue`；
  - event：`input` -> `update:modelValue`；

- **非兼容**：`v-bind` 的 `.sync` 修饰符和组件的 `model` 选项已移除，可用 `v-model` 作为代替；

- **新增**：现在可以在同一个组件上使用多个 `v-model` 进行双向绑定；

- **新增**：现在可以自定义 `v-model` 修饰符。



2.x: [v-model | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/migration/v-model.html#_2-x-语法)



3.x 语法

在 3.x 中，自定义组件上的 `v-model` 相当于传递了 `modelValue` prop 并接收抛出的 `update:modelValue` 事件：

```html
<ChildComponent v-model="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent
  :modelValue="pageTitle"
  @update:modelValue="pageTitle = $event"
/>
```



修改model名称:

若需要更改 `model` 名称，作为组件内 `model` 选项的替代，现在我们可以将一个 *argument* 传递给 `v-model`：

```html
<ChildComponent v-model:title="pageTitle" />

<!-- 是以下的简写: -->

<ChildComponent :title="pageTitle" @update:title="pageTitle = $event" />
```



在自定义组件上使用多个 `v-model`。

```html
<ChildComponent v-model:title="pageTitle" v-model:content="pageContent" />

<!-- 是以下的简写： -->

<ChildComponent
  :title="pageTitle"
  @update:title="pageTitle = $event"
  :content="pageContent"
  @update:content="pageContent = $event"
/>
```







#### emits

Vue 3 目前提供一个 `emits` 选项，和现有的 `props` 选项类似。这个选项可以用来定义组件可以向其父组件触发的事件。



2.x 的行为

在 Vue 2 中，你可以定义一个组件可接收的 prop，但是你无法声明它可以触发哪些事件：

```vue
<template>
  <div>
    <p>{{ text }}</p>
    <button v-on:click="$emit('accepted')">OK</button>
  </div>
</template>
<script>
  export default {
    props: ['text']
  }
</script>
```



3.x 的行为

和 prop 类似，组件可触发的事件可以通过 `emits` 选项被定义：

```vue
<template>
  <div>
    <p>{{ text }}</p>
    <button v-on:click="$emit('accepted')">OK</button>
  </div>
</template>
<script>
  export default {
    props: ['text'],
    emits: ['accepted']
  }
</script>
```

该选项也可以接收一个对象，该对象允许开发者定义传入事件参数的验证器，和 `props` 定义里的验证器类似。







## Vue3特性



#### 模板引用

[模板引用 | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/composition-api-template-refs.html#模板引用)



```vue
<template> 
  <div ref="root">This is a root element</div>
</template>

<script>
  import { ref, onMounted } from 'vue'

  export default {
    setup() {
      const root = ref(null)
      onMounted(() => {
        // DOM 元素将在初始渲染后分配给 ref
        console.log(root.value) // <div>This is a root element</div>
      })
      return {
        root
      }
    }
  }
</script>
```

**▼Steps**

1. 在html标签上添加 ref='refName'
2. 在setup中定义一个响应式变量  const refName = ref(null)
3. return 该变量 refName



**▼When will template-ref be rendered?**

经过实测, onBeforeCreated 时 template-ref并未被创建, 在onMounted时才有值

官方解释:  这里我们在渲染上下文中暴露 `root`，并通过 `ref="root"`，将其绑定到 div 作为其 ref。在虚拟 DOM 补丁算法中，如果 VNode 的 `ref` 键对应于渲染上下文中的 ref，则 VNode 的相应元素或组件实例将被分配给该 ref 的值。这是在虚拟 DOM 挂载/打补丁过程中执行的，因此模板引用只会在初始渲染之后获得赋值。





**▼more**

如果 ref 只是挂载到一般的 html 标签上, 那么会直接返回这个 html 标签, 但如果是挂载到一个vue组件上, 那么会返回一个 proxy 对象!

比如这里的 `validate-input` 是一个自定义组件, 我们按照之前的做法也 log 一下

```vue
<validate-input
  type="text"
  :rules="emailRules" v-model="emailValue"
  placeholder="Enter your email"
  ref="inputRef"
></validate-input>
```

 ![image-20210704210041251](D:\Study\FrontEnd\github_js_notes\FrontEnd_Notes\Vue学习\Vue3.assets\image-20210704210041251.png)

我们可以发现这里是一个 proxy 对象, 而且这里面还包含了许多我们在该自定义组件内部定义的属性和方法! 因此我们可以尝试使用 ref 来获取这些自定义组件的属性和方法! 





