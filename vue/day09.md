#### vue异步组件
在vue开发中，会用到特别多的组件，当我们打开网页的时候，突然一下子把所有的组件加载上来，可能会造成页面打开很慢。所以，我们可以使用异步组件，异步组件的特点：
1. 用不到的组件不会加载，当要用到这个组件的时候，才会通过异步加载
2. 缓存组件，通过异步加载的组件会缓存起来，当下次用到的时候，组件会很快加载出来  

Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。例如：
```bash
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```
这个工厂函数会收到一个 resolve 回调，这个回调函数会在你从服务器得到组件定义的时候被调用。。一个推荐的做法是将异步组件和 webpack 的 code-splitting 功能一起配合使用：
```bash
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```
你也可以在工厂函数中返回一个 Promise
```bash
Vue.component(
  'async-webpack-example',
  // 这个 `import` 函数会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

官方示例代码的实际使用方法，但我看得一脸懵逼。  

假如你写一个test.vue文件，在`<script>`标签里，实际方法：
```bash

//test.vue的部分
<script>
    import Vue from 'vue'
 
    //关键是以下这部分代码
    //需要将引入的异步组件，赋值给变量searchSearch
    //然后在下方components对象里，将变量正常添加进去，就可以使用异步组件了
    //第一个参数是组件名，第二个是异步引入的方法
    const searchSearch = Vue.component('searchSearch', function (resolve) {
        require(['./service-search.vue'], resolve)
    })
 
    export default{
        data(){
            return {}
        },
        methods: {},
        components: {
            searchSearch: searchSearch
        }
    }
</script>
```
在webpack中，更简单的异步组件使用方法。先引入Vue实例先，然后引入组件
```bash
<script>
    export default{
        data(){
            return {}
        },
        methods: {},
        components: {
            searchSearch: function (resolve) {
                //异步组件写法
                require(['./service-search.vue'], resolve)
            }
        }
    }
</script>
```
#### Promise
用于封装异步代码  
兑现（成功resolved）失败（rejected）  
1. Promise的实例  
```bash
var p=new Promise((resolve,reject)=>{
	setTimeout(()=>{
		let random = Math.random();
		if(random > 0.5){
			//承诺兑现
			resolve(random);
		} else {
			//承诺失败
			reject(random);
		}
    },1000);
  });
p.then((data)=>{
	console.log(data);
}).catch((error)=>{
	console.log(error);
});
```
2. 监听承诺对象状态的改变  
```bash
p.then().catch().finally();
.then(()=>{
	//承诺兑现的时候执行
})
.catch(()=>{
	//承诺异常的时候执行
})
.finally(()=>{
	//无论如何都需要执行的代码,当前node不支持，需要安装一个垫片
})
```
3. Promise.all([p1,p2])  
将数组中的多个承诺对象合并为一个承诺对象。当p1,p2,p3都成功的时候，p的状态才会转换为成功状态，但是如果p1,p2,p3有一个状态为失败状态,p的状态就转换为失败状态  
页面聊天系统，需要从两个不同的URL分别获得用户的个人信息和好友列表，这两个任务是可以并行执行的，用Promise.all()实现如下：
```bash
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
// 同时执行p1和p2，并在它们都完成后执行then:
Promise.all([p1, p2]).then(function (results) {
    console.log(results); // 获得一个Array: ['P1', 'P2']
});
```
同时向两个URL读取用户的个人信息，只需要获得先返回的结果即可。这种情况下，用Promise.race()实现：   
```bash
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
Promise.race([p1, p2]).then(function (result) {
    console.log(result); // 'P1'
});
```
由于p1执行较快，Promise的then()将获得结果'P1'。p2仍在继续执行，但执行结果将被丢弃。