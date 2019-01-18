#### vue动态组件  
通过使用保留的`<component>`元素，动态地绑定到它的 is 特性，可以实现动态组件。  
用 v-bind 给属性 is 动态传递了值，实现了组件的动态切换。
完整代码如下：
```bash
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>动态组件</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
  <div id="dynamic-component-demo" >
  <button
    v-for="tab in tabs"
    v-bind:key="tab"
    v-on:click="currentTab = tab"
  >{{ tab }}</button>
  <component
    v-bind:is="currentTabComponent"
  ></component>
</div>
</body>
</html>
<script>
Vue.component('tab-线路1', { 
	template: '<div>我是线路1</div>' 
})
Vue.component('tab-线路2', { 
	template: '<div>我是线路2</div>' 
})
Vue.component('tab-线路3', { 
	template: '<div>我是线路3</div>' 
})
new Vue({
  el: '#dynamic-component-demo',
  data: {
    currentTab: '线路1',
    tabs: ['线路1', '线路2', '线路3']
  },
  computed: {
    currentTabComponent: function () {
      return 'tab-' + this.currentTab.toLowerCase()
    }
  }
})
</script>
```
在上述示例中，currentTabComponent 可以包括
* 已注册组件的名字，或
* 一个组件的选项对象

#### 在动态组件上使用 keep-alive
上面是使用is 特性来切换不同的组件，当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。为了解决这个问题，我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。
```bash
 <keep-alive>
    <component v-bind:is="currentTabComponent"></component>
 </keep-alive>
```
`<keep-alive>` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 `<transition>` 相似，`<keep-alive> `是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中。  

activated 和 deactivated 在`<keep-alive>`树内的所有嵌套组件中触发 
```bash
<div id="example">
  <button @click="change">切换页面</button>
  <keep-alive>
    <component :is="currentView" @pass-data="getData"></component> 
  </keep-alive>
  <p>{{msg}}</p>
</div>
```
```bash
<script>
new Vue({
  el: '#example',
  data:{
    index:0,
    msg:'',    
    arr:[
      { 
        template:`<div>我是主页</div>`,
        activated(){
          this.$emit('pass-data','主页被添加');
        },
        deactivated(){
          this.$emit('pass-data','主页被移除');
        },        
      },
      {template:`<div>我是提交页</div>`},
      {template:`<div>我是存档页</div>`}
    ],
  },
  computed:{
    currentView(){
        return this.arr[this.index];
    }
  },
  methods:{
    change(){
      var len = this.arr.length;
      this.index = (++this.index)% len;
    },
    getData(value){
      this.msg = value;
      setTimeout(()=>{
        this.msg = '';
      },1000)
    }
  }
})
</script>
```
切换时会触发activated 和 deactivated的内容 