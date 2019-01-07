# vue

---
### 声明式渲染
Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统
```bash
vue create test 
cd test 
npm run serve
```
在helloworld.vue测试
```bash
<template>
<div>
<p>{{ message }}</p>
</div>

</template>
```
```bash
<script>
 export default {
  name: 'HelloWorld',
   data(){
     message: 'Hello Vue!'
   }
 }
</script>
```
 http://localhost:8080/
 ```bash
 Hello Vue!
 ```
除了文本插值，我们还可以像这样来绑定元素特性：
```bash
<template>
<div>
<span :title="message">
    鼠标悬停几秒查看时间
  </span>
  </div>
</template>
```
```bash
<script>
 export default {
   data(){
     message: new Date().toLocaleString()
   }
 }
</script>
```

## 条件与循环
指令带有前缀 v-，以表示它们是 Vue 提供的特殊特性。
### v-if  条件渲染

```bash
<template>
<div>
<p v-if="see">提示</p>
</div>
</template>
```
```bash
<script>
 export default {
  data: {
    see: true
  }
 }
</script>
```
把true改成false 就会发现提示消失了
### v-model
双向数据绑定,负责监听用户的输入事件以更新数据。
```bash
<template>
<div>
 <p>{{ message }}</p>
  <input v-model="message">
  </div>
</template>
```
```bash
<script>
 export default {
  data: {
    message: '1234567890'
  }
 }
</script>
```
当改变输入框里的内容时 P标签里的内容同时发生改变。

### 生命周期
>每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

#### 准备数据
>1. beforeCreate	初始化事件和生命周期完后，也就是运行new Vue,但是vue实例还没有完全创建出来，元素和数据为undefined，不可以通this访问
>2. created	   vue实例创建出来了.拿到数据(data),但是没有渲染到页面（template）通过this可以访问变量

#### 准备模板
>3. beforeMount		vue实例创建之后，数据渲染之前

#### 初次渲染
>4. mounted			做完替换，数据渲染完到页面上了。执行一次，代表初次渲染。

#### 监听data改变，随时准备重新渲染（循环）会执行多次，其余只执行一次
>5. beforeUpdate		数据改变，但是值没有更新到模板上
>6. updated			数据改变，值更新到了模板上

#### 销毁
>7. beforeDestroy
>8. destroyed   对data的改变不会触发周期函数。