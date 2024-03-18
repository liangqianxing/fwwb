[toc]

# vue

## 什么是 Vue？

```vue
import { createApp, ref } from 'vue'

createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')
```

```vue
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```

## 声明变量

```vue
const count = ref(0)
```

## 一个小样例

```vue
<script setup>
import {createApp,ref } from 'vue'
const count = ref(0)
const msg = ref('Hello World!')
createApp({
  setup() {
    return {
      count: ref(0)
    }
  }
}).mount('#app')
</script>
count=0
<template>
  <div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
  <h1>{{ msg }}</h1>
  <input v-model="msg" />
</template>

```

![image-20240318222213715](C:\Users\liangqianxing\AppData\Roaming\Typora\typora-user-images\image-20240318222213715.png)

## 单文件组件

在大多数启用了构建工具的 Vue 项目中，我们可以使用一种类似 HTML 格式的文件来书写 Vue 组件，它被称为**单文件组件** (也被称为 `*.vue` 文件，英文 Single-File Components，缩写为 **SFC**)。顾名思义，Vue 的单文件组件会将一个组件的逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里。下面我们将用单文件组件的格式重写上面的计数器示例：

```vue
<script setup>
import { ref } from 'vue'
const count = ref(0)
</script>

<template>
  <button @click="count++">Count is: {{ count }}</button>
</template>

<style scoped>
button {
  font-weight: bold;
}
</style>
```

## 选项式 API (Options API)

使用选项式 API，我们可以用包含多个选项的对象来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的属性都会暴露在函数内部的 `this` 上，它会指向当前的组件实例。

```vue
<script>
export default {
  // data() 返回的属性将会成为响应式的状态
  // 并且暴露在 `this` 上
  data() {
    return {
      count: 0
    }
  },

  // methods 是一些用来更改状态与触发更新的函数
  // 它们可以在模板中作为事件处理器绑定
  methods: {
    increment() {
      this.count++
    }
  },

  // 生命周期钩子会在组件生命周期的各个不同阶段被调用
  // 例如这个函数就会在组件挂载完成后被调用
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

## 组合式 API (Composition API)

通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 [`<script setup>`](https://cn.vuejs.org/api/sfc-script-setup.html) 搭配使用。这个 `setup` attribute 是一个标识，告诉 Vue 需要在编译时进行一些处理，让我们可以更简洁地使用组合式 API。比如，`<script setup>` 中的导入和顶层变量/函数都能够在模板中直接使用。

下面是使用了组合式 API 与 `<script setup>` 改造后和上面的模板完全一样的组件：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 用来修改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`The initial count is ${count.value}.`)
})
</script>

<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

# Vue.js

## 声明式渲染

你在编辑器中看到的是一个 Vue 单文件组件 (Single-File Component，缩写为 SFC)。SFC 是一种可复用的代码组织形式，它将从属于同一个组件的 HTML、CSS 和 JavaScript 封装在使用 `.vue` 后缀的文件中。

Vue 的核心功能是**声明式渲染**：通过扩展于标准 HTML 的模板语法，我们可以根据 JavaScript 的状态来描述 HTML 应该是什么样子的。当状态改变时，HTML 会自动更新。

能在改变时触发更新的状态被称作是**响应式**的。我们可以使用 Vue 的 `reactive()` API 来声明响应式状态。由 `reactive()` 创建的对象都是 JavaScript [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为与普通对象一样：

js

```vue
import { reactive } from 'vue'

const counter = reactive({
  count: 0
})

console.log(counter.count) // 0
counter.count++
```

`reactive()` 只适用于对象 (包括数组和内置类型，如 `Map` 和 `Set`)。而另一个 API `ref()` 则可以接受任何值类型。`ref` 会返回一个包裹对象，并在 `.value` 属性下暴露内部值。

js

```vue
import { ref } from 'vue'

const message = ref('Hello World!')

console.log(message.value) // "Hello World!"
message.value = 'Changed'
```

`reactive()` 和 `ref()` 的细节在[指南 - 响应式基础](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html)一节中有进一步讨论。

在组件的 `<script setup>` 块中声明的响应式状态，可以直接在模板中使用。下面展示了我们如何使用双花括号语法，根据 `counter` 对象和 `message` ref 的值渲染动态文本：

传入 `createApp()` 的对象是一个 Vue 组件。组件的状态应该在 `setup()` 函数中声明，并使用一个对象返回。

js

```vue
setup() {
  const counter = reactive({ count: 0 })
  const message = ref('Hello World!')
  return {
    counter,
    message
  }
}
```

返回对象中的属性可以在模板中使用。下面展示了我们如何使用双花括号语法，根据 `message` 的值来渲染动态文本：

template

```vue
<h1>{{ message }}</h1>
<p>count is: {{ counter.count }}</p>
```

注意我们在模板中访问的 `message` ref 时不需要使用 `.value`：它会被自动解包，让使用更简单。

能在改变时触发更新的状态被认为是**响应式**的。在 Vue 中，响应式状态被保存在组件中。在示例代码中，传递给 `createApp()` 的对象是一个组件。

我们可以使用 `data` 组件选项来声明响应式状态，该选项应该是一个返回对象的函数：

js

```vue
export default {
  data() {
    return {
      message: 'Hello World!'
    }
  }
}
```

js

```vue
createApp({
  data() {
    return {
      message: 'Hello World!'
    }
  }
})
```

`message` 属性可以在模板中使用。下面展示了我们如何使用双花括号法，根据 `message` 的值来渲染动态文本：

template

```vue
<h1>{{ message }}</h1>
```

在双花括号中的内容并不只限于标识符或路径——我们可以使用任何有效的 JavaScript 表达式。

template

```vue
<h1>{{ message.split('').reverse().join('') }}</h1>
```

现在，试着自己创建一些响应式状态，用它来为模板中的 `<h1>` 渲染动态的文本内容。

现在，试着自己创建一个数据属性，用它来为模板中的 `<h1>` 渲染动态的文本内容。

```vue
<script setup>
import { reactive, ref } from 'vue'

const counter = reactive({ count: 0 })
const message = ref('Hello World!')
</script>

<template>
  <h1>{{ message }}</h1>
  <p>Count is: {{ counter.count }}</p>
</template>
```

Attribute 绑定
-------------------

在 Vue 中，mustache 语法 (即双大括号) 只能用于文本插值。为了给 attribute 绑定一个动态值，需要使用 `v-bind` 指令：

template

    <div v-bind:id="dynamicId"></div>

**指令**是由 `v-` 开头的一种特殊 attribute。它们是 Vue 模板语法的一部分。和文本插值类似，指令的值是可以访问组件状态的 JavaScript 表达式。关于 `v-bind` 和指令语法的完整细节请详阅[指南 - 模板语法](https://cn.vuejs.org/guide/essentials/template-syntax.html)。

冒号后面的部分 (`:id`) 是指令的“参数”。此处，元素的 `id` attribute 将与组件状态里的 `dynamicId` 属性保持同步。

由于 `v-bind` 使用地非常频繁，它有一个专门的简写语法：

template

    <div :id="dynamicId"></div>

现在，试着把一个动态的 `class` 绑定添加到这个 `<h1>` 上，并使用 `titleClass` 的数据属性 ref 作为它的值。如果绑定正确，文字将会变为红色。

```vue
<script setup>
import { ref } from 'vue'

const titleClass = ref('title')
</script>

<template>
  <h1 :class="titleClass">Make me red</h1> <!-- 此处添加一个动态 class 绑定 -->
</template>

<style>
.title {
  color: red;
}
</style>
```

事件监听
-----------

我们可以使用 `v-on` 指令监听 DOM 事件：

template

    <button v-on:click="increment">{{ count }}</button>

因为其经常使用，`v-on` 也有一个简写语法：

template

    <button @click="increment">{{ count }}</button>

此处，`increment` 引用了一个使用 `methods` 选项声明的函数：

js

    export default {
      data() {
        return {
          count: 0
        }
      },
      methods: {
        increment() {
          // 更新组件状态
          this.count++
        }
      }
    }

js

    createApp({
      data() {
        return {
          count: 0
        }
      },
      methods: {
        increment() {
          // 更新组件状态
          this.count++
        }
      }
    })

在方法中，我们可以使用 `this` 来访问组件实例。组件实例会暴露 `data` 中声明的数据属性。我们可以通过改变这些属性的值来更新组件状态。

此处，`increment` 引用了一个在 `<script setup>` 中声明的函数：

vue

    <script setup>
    import { ref } from 'vue'
    
    const count = ref(0)
    
    function increment() {
      // 更新组件状态
      count.value++
    }
    </script>

此处，`increment` 引用了一个从 `setup()` 对象返回的方法：

js

    setup() {
      const count = ref(0)
    
      function increment(e) {
        // 更新组件状态
        count.value++
      }
    
      return {
        count,
        increment
      }
    }

在函数中，我们可以通过修改 ref 来更新组件状态。

事件处理函数也可以使用内置表达式，并且可以使用修饰符简化常见任务。这些细节包含在[指南 - 事件处理](https://cn.vuejs.org/guide/essentials/event-handling.html)。

现在，尝试自行实现 `increment` 方法函数并通过使用 `v-on` 将其绑定到按钮上。

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)
function increment() {
  // 更新组件状态
  count.value++
}
</script>

<template>
  <!-- 使此按钮生效 -->
<!--   <button v-on:click="increment">count is: {{ count }}</button> -->
  <button @click="increment">count is: {{ count }}</button>
</template>
```

表单绑定
-----------

我们可以同时使用 `v-bind` 和 `v-on` 来在表单的输入元素上创建双向绑定：

template

    <input :value="text" @input="onInput">

js

    methods: {
      onInput(e) {
        // v-on 处理函数会接收原生 DOM 事件
        // 作为其参数。
        this.text = e.target.value
      }
    }

js

    function onInput(e) {
      // v-on 处理函数会接收原生 DOM 事件
      // 作为其参数。
      text.value = e.target.value
    }

试着在文本框里输入——你会看到 `<p>` 里的文本也随着你的输入更新了。

为了简化双向绑定，Vue 提供了一个 `v-model` 指令，它实际上是上述操作的语法糖：

template

    <input v-model="text">

`v-model` 会将被绑定的值与 `<input>` 的值自动同步，这样我们就不必再使用事件处理函数了。

`v-model` 不仅支持文本输入框，也支持诸如多选框、单选框、下拉框之类的输入类型。我们在[指南 - 表单绑定](https://cn.vuejs.org/guide/essentials/forms.html)中讨论了更多的细节。

现在，试着用 `v-model` 把代码重构一下吧。

```vue
<script setup>
import { ref } from 'vue'

const text = ref('')
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

条件渲染
-----------

我们可以使用 `v-if` 指令来有条件地渲染元素：

template

    <h1 v-if="awesome">Vue is awesome!</h1>

这个 `<h1>` 标签只会在 `awesome` 的值为[真值 (Truthy)](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 时渲染。若 `awesome` 更改为[假值 (Falsy)](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)，它将被从 DOM 中移除。

我们也可以使用 `v-else` 和 `v-else-if` 来表示其他的条件分支：

template

    <h1 v-if="awesome">Vue is awesome!</h1>
    <h1 v-else>Oh no 😢</h1>

现在，示例程序同时展示了两个 `<h1>` 标签，并且按钮不执行任何操作。尝试给它们添加 `v-if` 和 `v-else` 指令，并实现 `toggle()` 方法，让我们可以使用按钮在它们之间切换。

更多细节请查阅 `v-if`：[指南 - 条件渲染](https://cn.vuejs.org/guide/essentials/conditional.html)

```vue
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() {
  awesome.value = !awesome.value
}
</script>

<template>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
```

列表渲染
-----------

我们可以使用 `v-for` 指令来渲染一个基于源数组的列表：

template

    <ul>
      <li v-for="todo in todos" :key="todo.id">
        {{ todo.text }}
      </li>
    </ul>

这里的 `todo` 是一个局部变量，表示当前正在迭代的数组元素。它只能在 `v-for` 所绑定的元素上或是其内部访问，就像函数的作用域一样。

注意，我们还给每个 todo 对象设置了唯一的 `id`，并且将它作为[特殊的 `key` attribute](https://cn.vuejs.org/api/built-in-special-attributes.html#key) 绑定到每个 `<li>`。`key` 使得 Vue 能够精确的移动每个 `<li>`，以匹配对应的对象在数组中的位置。

更新列表有两种方式：

1.  在源数组上调用[变更方法](https://stackoverflow.com/questions/9009879/which-javascript-array-functions-are-mutating)：
    
    js
    
        todos.value.push(newTodo)
    
    js
    
        this.todos.push(newTodo)
    
2.  使用新的数组替代原数组：
    
    js
    
        todos.value = todos.value.filter(/* ... */)
    
    js
    
        this.todos = this.todos.filter(/* ... */)
    

这里有一个简单的 todo 列表——试着实现一下 `addTodo()` 和 `removeTodo()` 这两个方法的逻辑，使列表能够正常工作！

关于 `v-for` 的更多细节：[指南 - 列表渲染](https://cn.vuejs.org/guide/essentials/list.html)

```vue
<script setup>
import { ref } from 'vue'

// 给每个 todo 对象一个唯一的 id
let id = 0

const newTodo = ref('')
const todos = ref([
  { id: id++, text: 'Learn HTML' },
  { id: id++, text: 'Learn JavaScript' },
  { id: id++, text: 'Learn Vue' }
])

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in todos" :key="todo.id">
      {{ todo.text }}
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
</template>
```

计算属性
-----------

让我们在上一步的 todo 列表基础上继续。现在，我们已经给每一个 todo 添加了切换功能。这是通过给每一个 todo 对象添加 `done` 属性来实现的，并且使用了 `v-model` 将其绑定到复选框上：

template

    <li v-for="todo in todos">
      <input type="checkbox" v-model="todo.done">
      ...
    </li>

下一个可以添加的改进是隐藏已经完成的 todo。我们已经有了一个能够切换 `hideCompleted` 状态的按钮。但是应该如何基于状态渲染不同的列表项呢？

介绍一个新概念：[计算属性](https://cn.vuejs.org/guide/essentials/computed.html)。我们可以使用 `computed` 选项声明一个响应式的属性，它的值由其他属性计算而来：

js

```vue
export default {
  // ...
  computed: {
    filteredTodos() {
      // 根据 `this.hideCompleted` 返回过滤后的 todo 项目
    }
  }
}
```

js

```vue
createApp({
  // ...
  computed: {
    filteredTodos() {
      // 根据 `this.hideCompleted` 返回过滤后的 todo 项目
    }
  }
})
```

介绍一个新 API：[`computed()`](https://cn.vuejs.org/guide/essentials/computed.html)。它可以让我们创建一个计算属性 ref，这个 ref 会动态地根据其他响应式数据源来计算其 `.value`：

js

    import { ref, computed } from 'vue'
    
    const hideCompleted = ref(false)
    const todos = ref([
      /* ... */
    ])
    
    const filteredTodos = computed(() => {
      // 根据 `todos.value` & `hideCompleted.value`
      // 返回过滤后的 todo 项目
    })

js

    import { createApp, ref, computed } from 'vue'
    
    createApp({
      setup() {
        const hideCompleted = ref(false)
        const todos = ref([
          /* ... */
        ])
    
        const filteredTodos = computed(() => {
          // 根据 `todos.value` & `hideCompleted.value`
          // 返回过滤后的 todo 项目
        })
    
        return {
          // ...
        }
      }
    })

diff

    - <li v-for="todo in todos">
    + <li v-for="todo in filteredTodos">

计算属性会自动跟踪其计算中所使用的到的其他响应式状态，并将它们收集为自己的依赖。计算结果会被缓存，并只有在其依赖发生改变时才会被自动更新。

现在，试着添加 `filteredTodos` 计算属性并实现计算逻辑！如果实现正确，在隐藏已完成项目的状态下勾选一个 todo，它也应当被立即隐藏。

```vue
<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Learn HTML', done: true },
  { id: id++, text: 'Learn JavaScript', done: true },
  { id: id++, text: 'Learn Vue', done: false }
])

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="new todo">
    <button>Add Todo</button>
  </form>
  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span :class="{ done: todo.done }">{{ todo.text }}</span>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>
  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
```

生命周期和模板引用
----------------

目前为止，Vue 为我们处理了所有的 DOM 更新，这要归功于响应性和声明式渲染。然而，有时我们也会不可避免地需要手动操作 DOM。

这时我们需要使用**模板引用**——也就是指向模板中一个 DOM 元素的 ref。我们需要通过[这个特殊的 `ref` attribute](https://cn.vuejs.org/api/built-in-special-attributes.html#ref) 来实现模板引用：

template

```vue
<p ref="pElementRef">hello</p>
```

要访问该引用，我们需要声明并暴露一个同名的 ref：

js

```vue
const pElementRef = ref(null)
```

js

```vue
setup() {
  const pElementRef = ref(null)

  return {
    pElementRef
  }
}
```

注意这个 ref 使用 `null` 值来初始化。这是因为当 `<script setup>``setup()` 执行时，DOM 元素还不存在。模板引用 ref 只能在组件**挂载**后访问。

要在挂载之后执行代码，我们可以使用 `onMounted()` 函数：

js

```vue
import { onMounted } from 'vue'

onMounted(() => {
  // 此时组件已经挂载。
})
```

js

```vue
import { onMounted } from 'vue'

createApp({
  setup() {
    onMounted(() => {
      // 此时组件已经挂载。
    })
  }
})
```

此元素将作为 `this.$refs.pElementRef` 暴露在 `this.$refs` 上。然而，你只能在组件**挂载**之后访问它。

要在挂载之后执行代码，我们可以使用 `mounted` 选项：

js

```vue
export default {
  mounted() {
    // 此时组件已经挂载。
  }
}
```

js

```vue
createApp({
  mounted() {
    // 此时组件已经挂载。
  }
})
```

这被称为**生命周期钩子**——它允许我们注册一个在组件的特定生命周期调用的回调函数。还有一些其他的钩子如 `created` 和 `updated``onUpdated` 和 `onUnmounted`。更多细节请查阅[生命周期图示](https://cn.vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram)。

现在，尝试添加一个 `mounted``onMounted` 钩子，然后通过 `this.$refs.pElementRef``pElementRef.value` 访问 `<p>`，并直接对其执行一些 DOM 操作。(例如修改它的 `textContent`)。

```vue
<script setup>
import { ref, onMounted } from 'vue'

const pElementRef = ref(null)

onMounted(() => {
  pElementRef.value.textContent = 'mounted!'
})
</script>

<template>
  <p ref="pElementRef">hello</p>
</template>
```

侦听器
----------

有时我们需要响应性地执行一些“副作用”——例如，当一个数字改变时将其输出到控制台。我们可以通过侦听器来实现它：

js

    import { ref, watch } from 'vue'
    
    const count = ref(0)
    
    watch(count, (newCount) => {
      // 没错，console.log() 是一个副作用
      console.log(`new count is: ${newCount}`)
    })

`watch()` 可以直接侦听一个 ref，并且只要 `count` 的值改变就会触发回调。`watch()` 也可以侦听其他类型的数据源——更多详情请参阅[指南 - 侦听器](https://cn.vuejs.org/guide/essentials/watchers.html)。

js

    export default {
      data() {
        return {
          count: 0
        }
      },
      watch: {
        count(newCount) {
          // 没错，console.log() 是一个副作用
          console.log(`new count is: ${newCount}`)
        }
      }
    }

这里，我们使用 `watch` 选项来侦听 `count` 属性的变化。当 `count` 改变时，侦听回调将被调用，并且接收新值作为参数。更多详情请参阅[指南 - 侦听器](https://cn.vuejs.org/guide/essentials/watchers.html)。

一个比在控制台输出更加实际的例子是当 ID 改变时抓取新的数据。在右边的例子中就是这样一个组件。该组件被挂载时，会从模拟 API 中抓取 todo 数据，同时还有一个按钮可以改变要抓取的 todo 的 ID。现在，尝试实现一个侦听器，使得组件能够在按钮被点击时抓取新的 todo 项目。

```vue
<script setup>
import { watch ,ref } from 'vue'

const todoId = ref(1)
const todoData = ref(null)

async function fetchData() {
  todoData.value = null
  const res = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  todoData.value = await res.json()
}

fetchData()
watch(todoId, fetchData)
</script>

<template>
  <p>Todo id: {{ todoId }}</p>
  <button @click="todoId++" :disabled="!todoData">Fetch next todo</button>
  <p v-if="!todoData">Loading...</p>
  <pre v-else>{{ todoData }}</pre>
</template>
```

组件
---------

目前为止，我们只使用了单个组件。真正的 Vue 应用往往是由嵌套组件创建的。

父组件可以在模板中渲染另一个组件作为子组件。要使用子组件，我们需要先导入它：

js

    import ChildComp from './ChildComp.vue'

js

    import ChildComp from './ChildComp.vue'
    
    export default {
      components: {
        ChildComp
      }
    }

我们还需要使用 `components` 选项注册组件。这里我们使用对象属性的简写形式在 `ChildComp` 键下注册 `ChildComp` 组件。

然后我们就可以在模板中使用组件，就像这样：

template

    <ChildComp />

js

    import ChildComp from './ChildComp.js'
    
    createApp({
      components: {
        ChildComp
      }
    })

我们还需要使用 `components` 选项注册组件。这里我们使用对象属性的简写形式在 `ChildComp` 键下注册 `ChildComp` 组件。

因为我们是在 DOM 中编写模板语法，因此需要遵循浏览器的大小写敏感的标签解析规则。所以，我们需要使用 kebab-case 的名字来引用子组件：

template

    <child-comp></child-comp>

现在自己尝试一下——导入子组件并在模板中渲染它。

**app.vue**

```vue
<script setup>
import ChildComp from './ChildComp.vue'
</script>

<template>
 <ChildComp />
</template>
```

**ChildComp.vue**

```vue
<template>
  <h2>A Child Component!</h2>
</template>
```

Props
------------

子组件可以通过 **props** 从父组件接受动态数据。首先，需要声明它所接受的 props：

vue

    <!-- ChildComp.vue -->
    <script setup>
    const props = defineProps({
      msg: String
    })
    </script>

注意 `defineProps()` 是一个编译时宏，并不需要导入。一旦声明，`msg` prop 就可以在子组件的模板中使用。它也可以通过 `defineProps()` 所返回的对象在 JavaScript 中访问。

js

    // 在子组件中
    export default {
      props: {
        msg: String
      },
      setup(props) {
        // 访问 props.msg
      }
    }

一旦声明，`msg` prop 就会暴露在 `this` 上，并可以在子组件的模板中使用。接收到的 props 会作为第一个参数传递给 `setup()`。

js

    // 在子组件中
    export default {
      props: {
        msg: String
      }
    }

一旦声明，`msg` prop 就会暴露在 `this` 上，并可以在子组件的模板中使用。

父组件可以像声明 HTML attributes 一样传递 props。若要传递动态值，也可以使用 `v-bind` 语法：

template

    <ChildComp :msg="greeting" />

template

    <child-comp :msg="greeting"></child-comp>

现在在编辑器中自己尝试一下吧。

**App.vue**

```vue
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const greeting = ref('Hello from parent')
</script>

<template>
  <ChildComp :msg="greeting" />
</template>
```

**ChildComp.vue**

```vue
<script setup>
const props = defineProps({
  msg: String
})
</script>

<template>
  <h2>{{ msg || 'No props passed yet' }}</h2>
</template>
```

Emits
------------

除了接收 props，子组件还可以向父组件触发事件：

```vue
<script setup>
// 声明触发的事件
const emit = defineEmits(['response'])

// 带参数触发
emit('response', 'hello from child')
</script>
```

```js
export default {
  // 声明触发的事件
  emits: ['response'],
  setup(props, { emit }) {
    // 带参数触发
    emit('response', 'hello from child')
  }
}
```

js

    export default {
      // 声明触发的事件
      emits: ['response'],
      created() {
        // 带参数触发
        this.$emit('response', 'hello from child')
      }
    }

`this.$emit()``emit()` 的第一个参数是事件的名称。其他所有参数都将传递给事件监听器。

父组件可以使用 `v-on` 监听子组件触发的事件——这里的处理函数接收了子组件触发事件时的额外参数并将它赋值给了本地状态：

template

    <ChildComp @response="(msg) => childMsg = msg" />

template

    <child-comp @response="(msg) => childMsg = msg"></child-comp>

现在在编辑器中自己尝试一下吧。

**App.vue**

```vue
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const childMsg = ref('No child msg yet')
</script>

<template>
  <ChildComp @response="(msg) => childMsg = msg" />
  <p>{{ childMsg }}</p>
</template>
```

**ChildComp.vue**

```vue
<script setup>
const emit = defineEmits(['response'])

emit('response', 'hello from child')
</script>

<template>
  <h2>Child component</h2>
</template>
```

插槽
---------

除了通过 props 传递数据外，父组件还可以通过**插槽** (slots) 将模板片段传递给子组件：

template

    <ChildComp>
      This is some slot content!
    </ChildComp>

template

    <child-comp>
      This is some slot content!
    </child-comp>

在子组件中，可以使用 `<slot>` 元素作为插槽出口 (slot outlet) 渲染父组件中的插槽内容 (slot content)：

template

    <!-- 在子组件的模板中 -->
    <slot/>

template

    <!-- 在子组件的模板中 -->
    <slot></slot>

`<slot>` 插口中的内容将被当作“默认”内容：它会在父组件没有传递任何插槽内容时显示：

template

    <slot>Fallback content</slot>

现在我们没有给 `<ChildComp>` 传递任何插槽内容，所以你将看到默认内容。让我们利用父组件的 `msg` 状态为子组件提供一些插槽内容吧。

**App.vue**

```vue
<script setup>
import { ref } from 'vue'
import ChildComp from './ChildComp.vue'

const msg = ref('from parent')
</script>

<template>
  <ChildComp>Message: {{ msg }}</ChildComp>
</template>
```

**ChildComp.vue**

```vue
<template>
  <slot>Fallback content</slot>
</template>
```

