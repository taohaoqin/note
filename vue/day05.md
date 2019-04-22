### vue插槽
Vue插槽，是学习vue中必不可少的一节，当初刚接触时，对这些掌握的一知半解，特别是作用域插槽一直没明白。
#### 插槽内容
插槽内可以是任意内容
```bash
<div id="app">
    <child-component></child-component>
</div>
```
```bash
<script>
    Vue.component('child-component',{
        template:`
            <div>Hello,World!</div>
        `
    })
    let vm = new Vue({
        el:'#app',
        data:{

        }
    })
</script>
```
输出的内容
```bash
Hello,World!
```
此时如果在`<child-component>你好</child-component>`中加入你好，里面的内容不起作用，输出结果还是原来的Hello,World! 

现在给组件增加一个`<slot></slot>`插槽
```bash
Vue.component('child-component',{
        template:`
            <div>
            Hello,World!
            <slot></slot>
            </div>
        `
    })
```
在`<child-component>你好</child-component>`内写的"你好"起作用了
输出结果：
```bash
Hello,World!你好
```
此时，如果在`<slot></slot>`中间加内容也是不会显示的 
```bash
<div id="app">
    <child-component>我是插槽</child-component>
</div>
```
```bash
<script>
  Vue.component('child-component', {
    template: `
         <div>
            <header>我是头部</header>
            <slot>我是中间</slot>
            <footer>我是尾部</footer>
        </div>
        `
})
new Vue({
    el: '#app',
})
</script>
```
结果：
```bash
我是头部
我是插槽
我是尾部
```
`<slot>我是中间</slot>`中间的内容没有显示出来

上面传入到组价的 内容 替换了组件原有的 slot 标签，slot 就像一个事先预留的位置，任何渲染时传入的内容都可以插入到这个位置，所以形象的叫它插槽。不光可以插标签，还可以插图片等等

#### 具名插槽
具名插槽，就是给这个插槽起个名字。不带 name 属性的 slot 插槽都是匿名插槽，如果有多个插槽，则传入的值会替换所有插槽。如果只想让值插入某一个插槽，就要使用具名插槽。
```bash
<div id="app">
    <child-component>
        <template slot="girl">
            女生
        </template>
        <template slot="boy">
            男生
        </template>
        <div>
            我是一类人，
            我是默认的插槽
        </div>
    </child-component>
</div>
```
```bash
<script>
    Vue.component('child-component',{
        template:`
            <div>
            <slot name="girl"></slot>
            <br>
            <slot name="boy"></slot>
            <slot></slot>
            <slot></slot>
            <slot></slot>
            </div>
        `
    })
    let vm = new Vue({
        el:'#app',
        data:{

        }
    })
</script>
```
结果：
```bash
女生
男生
我是一类人， 我是默认的插槽
我是一类人， 我是默认的插槽
我是一类人， 我是默认的插槽 
```

#### 作用域插槽
* 注意：下面操作是在基于 vue-cli 搭建的 vue 项目中进行的

如果想把子组件的数据渲染出来，但又不想给父组件传值，则可以在子组件的 slot 标签上绑定数据，然后通过父组件渲染。例如我想让子组件的数组渲染出来，则用下面代码：
```bash
//父组件部分
<template>
    <div>
        <son>
            <template slot-scope="scope" slot="item" >
                <ul>
                    <li>{{ scope.item.name}}  </li>
                </ul>
            </template>
        </son>
    </div>
</template>
<script>
import son from './son.vue'
export default {
    components: {
        son
    }
}
</script>
```

```bash
//子组件部分
<template>
    <div>
        <slot  v-for="item in lists" :item="item" name="item"></slot>
    </div>
</template>
<script>
export default {
    data() {
        return {
            lists: [
            {id:1,name:'孙悟空'},
            {id:2,name:'猪八戒'},
            {id:3,name:'沙和尚'}
            ]
        }
    },
    components: {
    }
}
</script>
```
显示结果:
```bash
[ { "id": 1, "name": "孙悟空" }, { "id": 2, "name": "猪八戒" }, { "id": 3, "name": "沙和尚" } ]
```
就是在父组件创建 template 标签，然后使用 slot-scope 特性从子组件获取数据。

* 为什么这种绑定数据的插槽叫作用域插槽，因为虽然数据看起来在父组件渲染了，其实并不是，父组件只是告诉子组件如何渲染而已，最终数据还是在子组件作用域渲染的，因为数据在 son 标签里面。在父组件无法拿到并使用这个数据，这也证明了数据是在子组件作用域的。