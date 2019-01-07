### 列表渲染
#### v-for 循环绑定(值的绑定，遍历)
用v-for指令根据一组数组的选项列表进行渲染。需要用item in items 的语法。items 是源数据数组并且 item 是数组元素迭代的别名。也可以用 of 替代 in 作为分隔符。
```bash
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```
```bash
new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
v-for还支持一个可选的第二个参数为当前项的索引
```bash
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```
```bash
new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
结果：

- Parent - 0 - Foo
- Parent - 1 - Bar 

也可以用 v-for 通过一个对象的属性来迭代
```bash
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```
```bash
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
- John
- Doe
- 30
提供第二个的参数为键名,第三个参数为索引
```bash
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>
```
```bash
0. firstName: John
1. lastName: Doe
2. age: 30 
```
 如果遍历的是数字，表示重复次数
```bash
 <span v-for="n in 10">{{ n }} </span>
```
### 事件处理
#### 监听事件v-on
 
添加一个事件监听器 将事件处理函数定义在methods属性中

```bash
<template>
<div>
 <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
  </div>
</template>
```
```bash
<script>
 export default {
data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage() {
      this.message = this.message.split('').reverse().join('')
    }
  }
 }
</script>
```
在页面上点击按钮 就能把输入框里的内容翻转，可以手动输入要翻转的内容。输入123456789 点击后为987654321

 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。
 ```bash
<div id="example-1">
  <button @click="counter += 1">Add 1</button>
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
 ```
 ```bash
 new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
 ```
 然而许多事件处理逻辑会更为复杂，所以直接把 JavaScript 代码写在 v-on 指令中是不可行的。因此v-on 还可以接收一个需要调用的方法名称。
```bash
<div id="example-2">
  <button @click="greet">Greet</button>
</div>
```
```bash
new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  methods: {
    greet: function (event) {
      alert('Hello ' + this.name + '!')
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})
example2.greet()
```
#### 事件修饰符
v-on 提供了事件修饰符
- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```bash
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

#### 表单输入绑定
用 v-model 指令在表单 input、textarea 及 select 元素上创建双向数据绑定
1. 单行文本框
```bash
<input type="text" v-model='message'>
```
2. 多行文本框
```bash
<textarea v-model='message'></textarea>
```
3. 复选框 复选框-必须有value值
   1. 单个复选框，绑定布尔值
   ```bash
   <input type="checkbox" id="checkbox" v-model="checked">
   ```
   2. 多个复选框，绑定到同一个数组
   ```bash
   <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
   <label for="jack">Jack</label>
   <input type="checkbox" id="john" value="John" v-model="checkedNames">
   <label for="john">John</label>
   ```

4. 单选按钮 也必须有value值
```bash
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
</div>
```
```bash
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
```
5. 下拉菜单
单选时
```bash
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```
```bash
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```
多选时要绑定到一个数组selected: []

- 用v-for渲染动态选项
```bash
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
```
```bash
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```
### 值绑定
对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)。
```bash
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```
#### v-model修饰符
- .number  如果想自动将用户的输入值转为数值类型，可以给 v-model 添加 number 修饰符
- .trim    如果要自动过滤用户输入的首尾空白字符，可以给 v-model 添加 trim 修饰符



