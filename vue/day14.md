###nextTick
`Vue.nextTick( [callback, context] )`
在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
```
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})
```

`vm.$nextTick( [callback] )`
将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。
```
new Vue({
  // ...
  methods: {
    // ...
    example: function () {
      // 修改数据
      this.message = 'changed'
      // DOM 还没有更新
      this.$nextTick(function () {
        // DOM 现在更新了
        // `this` 绑定到当前实例
        this.doSomethingElse()
      })
    }
  }
})
```

`activated(){}`:keep-alive 组件激活时调用 特别是在数据发生变化后要重新加载 不然用了keep-alive后只会现实原来的数据。

###mixin
`mixins`:选项接受一个混入对象的数组。这些混入实例对象可以像正常的实例对象一样包含选项，他们将在`Vue.extend()`里最终选择使用相同的选项合并逻辑合并。举例：如果你的混入包含一个钩子而创建组件本身也有一个，两个函数将被调用。
Mixin 钩子按照传入顺序依次调用，并在调用组件自身的钩子之前被调用。

创建一个mixin.js
```
const mixin = {
    data(){
        return {
            name:"啊啊啊啊啊啊b"
        }
    },
    created () {
        console.log('created mixin混入');
    },
    mounted(){
        console.log('mounted mixin混入');
    },
    methods: {
        hello(){
            console.log('mixin混入')
        }
    }
}
export default mixin
在文件引入一下就能掉用mixin.js里面的函数
import mixin from '../mixin.js'
export default {
  mixins: [mixin],
}
```
或者在main.js注册一个全局的mixin
```
Vue.mixin({
  data(){
    return{
      tes:'哈哈哈哈啊~~！',
      num:1
    }
  },
  methods:{
    hi(){
      console.log(this.tes)
    },
  }
})
```
所以mixin里面的函数都会被调用

### provide / inject
这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在起上下游关系成立的时间里始终生效。如果你熟悉 React，这与 React 的上下文特性很相似。
`provide`选项应该是一个对象或返回一个对象的函数。该对象包含可注入其子孙的属性。`inject`则用来接收`provide`返回的对象
第一个文件
```
<template>
    <div>  
        <childOne></childOne>
    </div>
</template>
<script>
 import childOne from './secend.vue'
export default {
    name:'Parent',
    provide(){
        return {
            for:"demo",   
        }
    },
  components:{
   childOne
  }
}
</script>
```
第二个文件
```
<template>
    <div>
        {{demo}}
        <childtwo></childtwo>
    </div>
</template>
<script>
import childtwo from './third.vue'
export default {
  name: "childOne",
  inject: ['for'],
  data() {
   return {
    demo: this.for + ' 1'
   }
  },
  components: {
   childtwo
  }
}
</script>
```
第三个文件
```
<template>
    <div>
       {{demo}}
    </div>
</template>
<script>
 export default {
  name: "",
  inject: ['for'],
  data() {
   return {
    demo: this.for + ' 2'
   }
  }
 }
</script>
```
最后显示
```
demo 1
demo 2
```
子孙两个文件都接收到了父文件传过来的for的属性值