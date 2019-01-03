### 插值
```bash
<span>{{msg}}</span>
```
绑定的数据对象上 msg 
```bash
<span v-once>这个将不会改变: {{ msg }}</span>
```
使用v-once指令时，当数据改变时，插值处的内容不会更新，但这会影响到该节点的其它数据绑定。

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，可以使用 v-html 指令：
```bash
<p>{{datas}}</p>
<p><span v-html="datas"></span></p>

data:{
    datas:'<p>使用v-html指令</p>'
}
```
输出：
```bash
<p>使用v-html指令</p>
使用v-html指令
```
### 指令
指令是带有v- 前缀的特殊特性，指令的特性的值预期是单个的JavaScript表达式（v-for例外）。指令的职责是，当表达式的值改变时，将产生连带影响，响应式作用输DOM。
```bash
<p v-if="seen">现在你看到我了</p>
```
v-if 指令将根据表达式 seen 的值的真假来插入/移除 p元素

### 修饰符
修饰符是以半角句号.指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
```bash
.lazy    输入框的值与数据进行同步 
.number  自动将用户的输入值转为数值类型
.trim    自动过滤用户输入的首尾空白字符
.stop    阻止事件冒泡（内->外）
.prevent 阻止事件默认行为
.passive 一般用在屏幕滑动的情况下
.capture 阻止事件捕获（外->内）
.once	 事件执行一次
.self	 给自身绑定事件，只有在操作自身才触发事件
```
### 缩写
1. v-bind 缩写
```bash
完整语法 
<a v-bind:href="url">...</a>
缩写 
<a :href="url">...</a>
```
2. v-on 缩写
```bash
完整语法 
<a v-on:click="doSomething">...</a>
缩写 
<a @click="doSomething">...</a>
```

### Class 与 Style 绑定
#### class的绑定
1. 语法对象 我们可以传给 v-bind:class 一个对象
```bash
<div v-bind:class="{ active: isActive }"></div>
```
属性的值必须为boolean类型。此外，v-bind:class 指令也可以与普通的 class 属性共存：
```bash
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

data: {
  isActive: true,
  hasError: false
}
```
渲染结果为：
```bash
<div class="static active"></div>
```
2. 数组语法 我们可以把一个数组传给 v-bind:class，以应用一个 class 列表
```bash
<div v-bind:class="[activeClass, errorClass]"></div>

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染结果为：
```bash
<div class="active text-danger"></div>
```
如果要根据条件切换class 可以用三元表达式
```bash
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
3. 在组件上
当在自定义组件上使用class属性时，这些类将被添加到该组件的根元素上面。这个元素存在的类不会被覆盖。
```bash
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```
添加class
```bash
<my-component class="baz boo"></my-component>
```
渲染结果
```bash
<p class="foo bar baz boo">Hi</p>
```
数据绑定的class也同样适用
```bash
<my-component v-bind:class="{ active: isActive }"></my-component>
```
当isActive为true时 渲染结果为
```bash
<p class="foo bar active">Hi</p>
```
#### style的绑定
1. 对象语法
v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。
```bash
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```bash
data: {
  activeColor: 'red',
  fontSize: 30
}
```
或者
```bash
<div v-bind:style="styleObject"></div>
```
```bash
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
2. 数组语法
v-bind:style 的数组语法可以将多个样式对象应用到同一个元素上
```bash
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
从2.3.0起 style绑定的属性中 一个数组可以包含多个值，常用于提供多个带前缀的值
```bash
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
### 条件渲染
>在用v-if时 可以使用v-else来表示v-if的else块
>v-else-if 充到v-if的else-if块 可以连续使用
>类似于v-else，v-else-if 必须紧跟在带 v-if 或者 v-else-if的元素之后
```bash
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```
#### key
vue为我们提供了一种方式来表达‘两个元素是独立的，不要复用。只需要添加一个具有唯一值的key属性

#### v-show
繁的切换某个东西的显示与隐藏，不同的是带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地通过改变display属性的取值改变元素的显示与隐藏状态。不支持v-else。
### v-if vs v-show
1. v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
2. v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
3. 相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
4. 一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。


