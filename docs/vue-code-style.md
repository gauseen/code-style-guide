
### Vue 编码风格探讨

#### JavaScript standard  [代码规范][4]

#### 什么是组件？
* 组件是可复用的 Vue 实例，且带有一个名字。

#### 组件衡量维度：
* 现在被很多地方使用
* 将来有很大可能性被复用

#### 组件命名：
* 有意义的：见名知意
* 简短: 1 到 3 个单词（不包含组件前缀命名空间）
* 具有可读性: 组件名应该倾向于完整单词而不是缩写，以便于沟通交流
* 多个单词必须符合[自定义元素规范][1] [vue code style][3]: 使用连字符（`-`）分隔单词，切勿使用保留字
* `sh-` 前缀作为命名空间：这样可以防止命名冲突，方便其它项目复用

#### 为什么？
* 组件是 Vue 主要功能之一，用的地方多，所以命名需要简短、见名知意（具有可读性）

#### 组件分类
* [基础组件—无状态][2]：`el-button、el-icon`。
  * 只是展示类的
  * 无业务、无第三方依赖
  * 无状态，只接受`props`，无 `data () {}`
  * 基础组件根据不同的`props`展现出不同的样式，并且会抛出事件来通知父级组件。

* 基础组件—有状态：`vue-lazyload、el-upload`
  * 有自己维护的 data 数据（状态）
  * 可复用的逻辑组件

* [业务组件][6]：`van-address-list(地址列表)、van-submit-bar(提交订单栏)、print-net(网络打印)`
  * 根据项目类型封装定制化组件

* 二次封装组件
  * 基于第三方 UI 组件库再次封装的组件

* [紧密耦合的组件命名规则][5]：如：`sideBar、sideBarItem`
  * 和父组件紧密耦合的子组件应该以父组件名作为前缀命名


#### 组件表达式简单化
组件内的表达式非常便利，但是设计它们的初衷是用于简单运算的。

* 复杂行内表达式可读性差，不利于二次开发
* 不能复用，可能导致重复编码

所以，对于任何复杂逻辑，都应当使用计算属性。

```html
<!-- 避免 -->
<template>
  <div id="example">
    {{ message.split('').reverse().join('') }}
  </div>
</template>

<!-- 推荐 -->
<template>
  <div id="example">
    <p>{{ reversedMessage }}</p>
  </div>
</template>
<script type="text/javascript">
  export default {
      data: {
        message: 'Hello'
      },
      computed: {
        reversedMessage () {
          return this.message.split('').reverse().join('')
        },
    },
  }
</script>
```

#### props 定义应该尽量详细
* 更容易的看懂组件的用法
* 在开发环境下，如果向一个组件提供格式不正确的 prop，Vue 将会告警，以帮助捕获潜在的错误来源

```js
// 避免
props: ['status']

// 推荐
props: {
  status: {
    type: String,
  },
}
```
* props 名大小写  
在声明 prop 的时候，其命名应该始终使用 camelCase  
我们单纯的遵循每个语言的约定。在 JavaScript 中更自然的是 camelCase。而在 HTML 中则是 kebab-case。

```js
// 避免
props: {
  'greeting-text': String,
}

<welcome-message greetingText="hi"></welcome-message>

// 推荐
props: {
  greetingText: String,
}

<welcome-message greeting-text="hi"></welcome-message>
```

#### 避免 v-if 和 v-for 用在一起
永远不要把 v-if 和 v-for 同时用在同一个元素上
```html
<!-- 避免 -->
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

<ul>
  <li
    v-for="user in users"
    v-if="isShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

<!-- 推荐 -->
<ul>
  <li
    v-for="user in activeUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
<script>
  data () {
    return {
      users: [...],
    }
  },
  computed: {
    activeUsers () {
      return this.users.filter(user => user.isActive)
    }
  },
</script>

<ul v-if="isShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

```

#### 组件结构化
按照一定的结构组织，组件易于团队阅读、理解

```html
<template lang="html">
  <div class="todo-list">
    <!-- ... -->
  </div>
</template>

<script>
  export default {
    // name 属性，全局感知
    name: 'todo-list',
    // 使用组件 mixins 共享通用功能
    mixins: [],
    // 禁用子组件根元素 继承 父组件属性（不影响 class 和 style 绑定）
    inheritAttrs: false,
    // 组件属性、变量
    props: {
      // 按字母顺序
      bar: {},
      foo: {},
      fooBar: {},
    },
    // 变量
    data() {},
    computed: {},
    // 模板依赖 (模板内使用的资源)
    // 使用其它组件
    components: {},
    directives: {},
    filters: {},
    // 事件 (通过响应式事件触发的回调)
    watch: {},
    // 生命周期钩子 (按照它们被调用的顺序)
    beforeCreate() {},
    created() {},
    beforeMount() {},
    mounted() {},
    beforeUpdate() {},
    updated() {},
    activated() {},
    deactivated() {},
    beforeDestroy() {},
    destroyed() {},
    // 非响应式的属性 (不依赖响应系统的实例属性)
    methods: {},
    // 渲染 (组件输出的声明式描述)
    template: ``,
    render (h) {},
    renderError (h, err) {},
  }
</script>

<style scoped>
  .todo-list { /* ... */ }
</style>
```

#### 代码注释
* 单行注释 尽量独占一行。`//` 后跟一个空格，缩进与下一行被注释说明的代码一致。

便于编辑器 快捷键 删除、修改

```js
// 避免
if (status === 1) { // status: 1 成功，0 失败

}

// 推荐
// status: 1 成功，0 失败
if (status === 1) {

}
```

#### CSS 作用域
* 什么是 CSS 作用域？  

当 `<style>` 标签有 `scoped` 属性时，它的 `CSS` 只作用于当前组件中的元素，也就是说，该样式只能适用于当前组件元素，也就实现了组件之间 `CSS` 样式隔离  

* 原理  
`Vue` 中的 `scoped` 属性的效果主要通过`PostCSS`转译实现，`PostCSS`给一个组件中的所有`dom`添加了一个独一无二的动态属性，然后在`CSS`选择器中添加一个对应值的属性选择器，这样就实现了组件之间的样式隔离

```html
<!-- PostCSS 转译前 -->
<template>
  <div class="not-found">
    <div class="not-found__text">404</div>
  </div>
</template>
<style scoped>
.not-found__text {
  color: red;
}
</style>

<!-- PostCSS 转译后 -->
<div data-v-32a7c634 class="not-found">
  <div data-v-32a7c634 class="not-found__text">404</div>
</div>

<style>
.not-found__text[data-v-32a7c634] {
  color: red;
}
</style>
```

#### 父组件层叠（覆盖）子组件样式

* 全局样式  
  * 缺点：容易污染其他组件样式
```css
<style>
/* 全局样式 */
</style>
```

* 深度作用选择器  

```html
<!-- PostCSS 转译前 -->
<template>
  <div class="not-found">
    <svg-icon name="404" class="not-found__svg"></svg-icon>
  </div>
</template>

<style scoped>
/* 深度作用选择器 */
.not-found >>> .not-found__svg {
  font-size: 3.6rem;
  color: #fff;
}

.not-found .not-found__svg {
  font-size: 3.6rem;
  color: #fff;
}
</style>
```

```html
<!-- PostCSS 转译后 -->
<template>
  <div data-v-32a7c634 class="not-found">
    <svg data-v-6ba02d72 data-v-32a7c634 name="404" class="not-found__svg">
        <use data-v-6ba02d72>...</use>
    </svg>
  </div>
</template>

<style scoped>
/* 深度作用选择器 */
.not-found[data-v-32a7c634] .not-found__svg {
  font-size: 3.6rem;
  color: #fff;
}

/* 非深度作用选择器 */
.not-found .not-found__svg[data-v-32a7c634] {
  font-size: 3.6rem;
  color: #fff;
}
</style>
```

#### 动态生成的内容
通过`v-html`创建的`DOM`内容不受作用域内的样式影响，但是你仍然可以通过深度作用选择器来为他们设置样式。

#### 元素(标签)选择器应该避免在 CSS 中出现
类选择器比元素选择器更好，因为大量使用元素选择器很慢！

#### 单文件组件的顶级元素顺序（所有项目应保持统一）
```html
<!-- todo.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```


#### 元素(包括组件)特性应该有统一的顺序【非必须】
* 定义(提供组件的选项)
  * `is`

* 列表渲染(创建多个变化的相同元素)
  * `v-for`

* 条件渲染 (元素是否渲染/显示)
  * `v-if`
  * `v-else-if`
  * `v-else`
  * `v-show`
  * `v-cloak`
* 渲染方式 (改变元素的渲染方式)
  * `v-pre`
  * `v-once`
* 唯一的特性 (需要唯一值的特性)
  * `ref`
  * `key`
  * `slot`
* 双向绑定 (把绑定和事件结合起来)
  * `v-model`
* 其它特性 (所有普通的绑定或未绑定的特性 calss style)
* 事件 (组件事件监听器)
  * `v-on`
* 内容 (覆写元素的内容)
  * `v-html`
  * `v-text`


#### 参考
[vue-style-guide][20]
[vuejs-component-style-guide][21]


<!-- 引用文献 -->
[1]: https://www.zhangxinxu.com/wordpress/2018/03/htmlunknownelement-html5-custom-elements
[2]: https://cn.vuejs.org/v2/style-guide/#%E5%9F%BA%E7%A1%80%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90
[3]: https://cn.vuejs.org/v2/style-guide/#%E6%A8%A1%E6%9D%BF%E4%B8%AD%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D%E5%A4%A7%E5%B0%8F%E5%86%99-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90
[4]: https://github.com/gauseen/standard/blob/master/README.md
[5]: https://cn.vuejs.org/v2/style-guide/#%E7%B4%A7%E5%AF%86%E8%80%A6%E5%90%88%E7%9A%84%E7%BB%84%E4%BB%B6%E5%90%8D-%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90
[6]: https://youzan.github.io/vant/#/zh-CN/address-list

<!-- 参考文献 -->
[20]: https://cn.vuejs.org/v2/style-guide/
[21]: https://github.com/pablohpsilva/vuejs-component-style-guide/blob/master/README-CN.md