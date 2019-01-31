### vue-router
路由，就是指向的意思，当我们点击home按钮时，页面中就显示home 的内容，如果点击about按钮，就显示about上的内容。
#### 路由配置
>执行 npm install vue-router –save
在src 目录下新建两个组件，home.vue 和 about.vue
```bash
<template>
    <div>
        <h1>home</h1>
        <p>{{homeMsg}}</p>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                homeMsg: "我是home组件"
            }
        }
    }
</script>
```
```bash
<template>
    <div>
        <h1>about</h1>
        <p>{{aboutMsg}}</p>
    </div>
</template>
<script>
    export default {
        data () {
            return {
                aboutMsg: "我是about组件"
            }
        }
    }
</script>
```
在App.vue中 定义`<router-link >` 和 `</router-view>`  
`<router-link>` 和`<router-view>`来对应点击和显示部分。`<router-link> `就是定义页面中点击的部分，`<router-view> `定义显示部分，就是点击后，区配的内容显示在什么地方。所以 `<router-link>` 还有一个非常重要的属性 to，定义点击之后，要到哪里去， 如：`<router-link  to="/home">Home</router-link>`
```bash
<template>
  <div id="app">
    <header>
      <router-link to="/home">Home</router-link>
      <router-link to="/about">About</router-link>
    </header>
    <router-view></router-view>   
  </div>
</template>
<script>
export default {
  
}
</script>
```
 在src目录下再新建一个router.js 定义router  
 ```bash
import Vue from "vue";
import Router from "vue-router";

// 引入组件
import home from "./home.vue";
import about from "./about.vue";

// 要告诉 vue 使用 vueRouter
Vue.use(Router);

export default new Router({
 routes:[
    {
        path:"/home",
        component: home
    },
    {
        path: "/about",
        component: about
    }
  ]
})
 ```
 把路由注入到根实例中，启动路由，在main.js中引入路由，注入到根实例中。
 ```bash
import Vue from 'vue'
import App from './App.vue'

// 引入路由
import router from "./router.js"    
new Vue({
  el: '#app',
  router,  // 注入到根实例中
  render: h => h(App)
})
 ```
 #### 动态路由匹配（多个路由对应一个组件）
 在router.js中配置
 ```bash
    {
        path:"/user/:id",
        component: user
    },
 ```
 ```bash
<router-link to="/user/1">user1</router-link>
<router-link to="/user/2">user2</router-link>
<router-link to="/user/3">user3</router-link>
 ```
 ```bash
<template>
    <div>
        <h1>User</h1>
        <div>我是user组件</div>
    </div>
</template>
<script>
    export default {

    }
</script>
 ```
关于路由跳转中的params，可以通过 return this.$route.params.id来获取。
```bash
<template>
    <div>
        <h1>User</h1>
        <div>我是user组件, 动态部分是{{dynamicSegment}}</div>
    </div>
</template>
<script>
    export default {
        computed: {
            dynamicSegment () {
                return this.$route.params.id
            }
        }
    }
</script>
```
如果改变动态路由中的参数，组件不会重新加载，也就说很多生命周期钩子函数只会执行一次（created,mounted），要想监控这种改变，可以通过watch来监听$route。
```bash
<script>
    export default {
        data () {
            return {
                dynamicSegment: ''
            }
        },
        watch: {
            $route (to,from){
                // to表示的是你要去的那个组件，from 表示的是你从哪个组件过来的，它们是两个对象，你可以把它打印出来，它们也有一个param 属性
                console.log(to);
                console.log(from);
                this.dynamicSegment = to.params.id
            }
        }
    }
</script>
```
#### 嵌套路由
就是路由下面还有分类。例如：当我们进入到home页面的时候，它下面还有分类，如手机系列，平板系列，电脑系列。当我们点击各个分类的时候，它还是需要路由到各个部分，如点击手机，它肯定到对应到手机的部分。
```bash
//home.vue 
<template>
    <div>
        <h1>home</h1>
<!-- router-link 的to属性要注意，路由是先进入到home,然后才进入相应的子路由如 phone,所以书写时要把 home 带上 -->
        <p>
            <router-link to="/home/phone">手机</router-link>
            <router-link to="/home/tablet">平板</router-link>
            <router-link to="/home/computer">电脑</router-link>
        </p>
        <router-view></router-view>
    </div>
</template>
```
router.js配置修改：
```bash
export default new Router({
  router:[
    {
        path:"/home",
        component: home,
        children: [
            {
                path: "phone",
                component: phone
            },
            {
                path: "tablet",
                component: tablet
            },
            {
                path: "computer",
                component: computer
            }
        ]
    },
    {
        path: "/about",
        component: about
    },{
        path: "/user",
        component: user
    }
  ]
})
```
#### 命名路由
那就直接给这个路由加一个name 属性，就可以了
```bash
{
    path: "/user/:id",
    name: "user",
    component: user
}
```