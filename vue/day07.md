### 组件
#### 基本实例
```bash
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```
```bash
<script>
    Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
}),
new Vue({ el: '#components-demo' })
</script>
```
组件是一个复用Vue实例 带一个名字：在这个例子中是`<button-counter>`。通过new Vue创建Vue根实例，把这个组件的自定义元素拿来使用，组件能进行任意次的复用，每个组件都会各自维护它的count。
> data必须是个函数 如果不是函数 count的影响到其他的实例。

#### 通过prop向子组件传递数据，通过事件向父级组件发送消息，用$emit方法并传入事件的名字，来向父级组件触发一个事件
#### 实例2
```bash
<div id="blog-posts-events-demo">
  <div :style="{ fontSize: postFontSize + 'em' }">
    <blog-post
      v-for="post in posts"
      v-bind:key="post.id"
      v-bind:post="post"
      v-on:enlarge-text="postFontSize += $event"
    ></blog-post>
  </div>
</div>
```
```bash
<script>
Vue.component('blog-post', {
  props: ['post'],
  template: `
    <div class="blog-post">
      <h3>{{ post.title }}</h3>
      <button v-on:click="$emit('enlarge-text',0.1)">
  增大
</button>
      <div v-html="post.content"></div>
    </div>
  `
}),
new Vue({
  el: '#blog-posts-events-demo',
  data: {
    posts: [
      { id: 1, title: '老大' },
      { id: 2, title: '老二' },
      { id: 3, title: '老三' }
    ],
    postFontSize: 1
  }
})
</script>
```
点击按钮三个标题同时增大

#### Prop
组件实例的作用域是孤立的，这意味着不能在子组件的模板内直接引父组件的数据，如果要让子组件使用父组件的数据，则需要通过子组件的prop选项；prop是单向绑定的，当父组件的属性变化时，将传递给子组件，但是反过来不行；这样主要是防止子组件无意修改父组件的状态；每次父组件更新时，子组件的所有prop都会更新为最新值。这意味着你不能在子组件内部改变prop。prop在组件上注册的一些自定义特性，当一个值传递给一个prop特性的时候，它就变成了那个组件的一个属性，我们可以使用 v-bind 来动态传递 prop，如上所示。不论何时为 post 对象添加一个新的属性，它都会自动地在 `<blog-post>`内可用。

#### Prop类型
通常你希望每个prop都有指定的值类型。这时，你可以以对象形式列出prop，这些属性的名称和值分别是prop各自的名称和类型
```bash
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```
一个数字，一个布尔值，数组，对象，一个对象的所有属性，任何类型的值都可以传给一个 prop。
#### Prop验证
```bash
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```
#### $emit的用法
子组件可以使用 `$emit` 触发父组件的自定义事件 
`this $emit('自定义事件名',要传送的数据)`
```bash
//子组件
 <el-form-item label="种类">
    <el-input  v-model="name"></el-input>
  </el-form-item>
  <el-form-item label="销量" >
    <el-input  v-model="number"></el-input>
  </el-form-item>
  <el-form-item>
    <el-button type="primary" @click="submitForm">确定</el-button>//主要这行
    <el-button @click="resetForm">取消</el-button>
  </el-form-item>
```
```bash
 methods:{
       submitForm(){ 
          this.$emit('submitForm',this.name, this.number)     
    },
  }
```
```bash
//父组件
<MyForm @submitForm="submitForm"></MyForm>//调用
```
```bash
 methods: {
            submitForm(name,number){
                this.tableData.push({'name':name, 'number':number})
            }
        }
```
这样就能将数据 name number 发送给父组件

#### 组件名
- 组件名分大小写使用 kebab-case
`Vue.component('my-component-name', { /* ... */ })`
- 使用 PascalCase
`Vue.component('MyComponentName', { /* ... */ })`
当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的。

#### 组件注册
#### 全局注册
到目前为止，用Vue.component来创建的组件都是全局注册的，也就是说它们在注册之后可以用在任何新创建的 Vue 根实例 (new Vue) 的模板中，在所有子组件中也是如此。
```bash
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
  <component-c></component-c>
</div>

Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })
new Vue({ el: '#app' })
```
#### 局部注册
全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。
```bash
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }

new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```
对于 components 对象中的每个属性来说，其属性名就是自定义元素的名字，其属性值就是这个组件的选项对象。
- 注意局部注册的组件在其子组件中不可用。例如，如果你希望 ComponentA 在 ComponentB 中可用，则你需要这样写：
```bash
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```




