### VueX
vuex是一个专门为vue.js应用程序开发的状态管理模式，采用集中式存储管理所有组件的公共状态，并以相应的规则保证状态以一种可预测的方式发生变化。
![](img/4.png)  
虚线包裹起来的部分就是vuex的核心，state中保存的就是公共状态，改变state的唯一方法是通过mutations进行更改。 
安装 :
> npm install vuex --save
在src文件新建一个store文件夹，里面新建一个index.js内容如下： 
```bash
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);
const store = new Vuex.Store();
 
export default store;
```
接下来，在main.js里面引入store，然后全局注册一下
```bash
import store from './store/index.js'   
new Vue({
  router,
  store,
  render: h => h(App),
}).$mount('#app')
```
弄好了以后就可以在store里面的index.js里面操作了
```bash
const store = new Vuex.Store({
    //待添加
})
```
先是state 就是vuex中的公共状态，是用于保存所有组件的公共数据
```bash
const state={   //要设置的全局访问的state对象
     showFooter: true,
     changableNum:0
     //要设置的初始属性值
   };
```
这样，就可以用`this.$store.state.showFooter`或`this.$store.state.changebleNum`在任何一个组件里面获取showfooter和changebleNum定义的值了，但这不是理想的获取方式。vuex官方API提供了一个getters，和vue计算属性computed一样，来实时监听state值的变化(最新状态)，并把它也仍进Vuex.Store里面。
```bash
 const state={   //要设置的全局访问的state对象
     showFooter: true,
     changableNum:0
     //要设置的初始属性值
   };
const getters = {   //实时监听state值的变化(最新状态)
    isShow(state){  //方法名随意,主要是来承载变化的showFooter的值
       return state.showFooter
    },
    getChangedNum(){  //方法名随意,主要是用来承载变化的changableNum的值
       return state.changebleNum
    }
};
```
此时，要引用的话就用`this.$store.getters.isShow`
接下来要说的就是mutations了，mutattions也是一个对象，这个对象里面可以放改变state的初始值的方法
```bash
const mutations = {
    show(state) {   //自定义改变state初始值的方法，这里面的参数除了state之外还可以再传额外的参数(变量或对象);
        state.showFooter = true;
    },
    hide(state) {  //同上
        state.showFooter = false;
    },
    newNum(state,sum){ //同上，这里面的参数除了state之外还传了需要增加的值sum
       state.changableNum+=sum;
    }
};
```
这时候用`his.$store.commit('show') `或 `this.$store.commit('hide')` 以及 `this.$store.commit('newNum',6)` 在别的组件里面进行改变showfooter和changebleNum的值了，但这不是理想的方法，mutations里面的方法都是同步的。所以官方又提供了一个actions，这也是一个对象变量，作用是在action方法里面可以进行异步操作，这里面的方法是用异步触发mutations里面的方法。，actions里面自定义的函数接收一个context参数和要变化的形参，context与store实例具有相同的方法和属性，所以它可以执行context.commit(' '),然后也不要忘了把它也扔进Vuex.Store里面：
```bash
const actions = {
    hideFooter(context) {  //自定义触发mutations里函数的方法，context与store 实例具有相同方法和属性
        context.commit('hide');
    },
    showFooter(context) {  //同上注释
        context.commit('show');
    },
    getNewNum(context,num){   //同上注释，num为要变化的形参
        context.commit('newNum',num)
    }
};
```
在外部组件执行actions时，执行`this.$store.dispatch('hideFooter')`
或`this.$store.dispatch('showFooter')`
以及`this.$store.dispatch('getNewNum'，6) ` 
  
但有时有多方面需求的时候，state是关于很多块内容时，就可以使用vuex中的modules模块化了，先在store文件夹下新建一个modules文件夹，然后在modules里面建立需要管理状态的js文件，既然要把不同的状态分开管理，那就要把它们给分成独立的状态文件，要记得在store下的index.js的内容修改如：
```bash
import Vue from 'vue';
import Vuex from 'vuex';
import footerStatus from './modules/footerStatus.js'
import collection from './modules/collection.js'
Vue.use(Vuex);

export default new Vuex.Store({
    modules:{
         footerStatus,
         collection
    }
});
```
就可以在各个文件里写各种的内容了。