#### 子组件对父组件的的数据传递
之前写过一个例子，把一个+1按钮当作父组件，把一个输入框当作子组件，点按钮输入框里的数的大小+1  如果从输入框手动输入后，按按钮会从输入后的值上开始增加。
父组件
```bash
<template>
  <div>
    <p>{{count}}</p >
    <button @click="increase">+1</button>
    <inputCount :count="count" @getValue="getValue"></inputCount>
  </div>
</template>
<script>
import inputCount from "@/components/inputCount.vue";
export default {
  components: {
    inputCount
    },
  data() {
    return {
    count: 1
      };
    },
  methods: {
    increase() {
      this.count++;
      },
       getValue(value) {
       this.count = value;
      }
    }
  }
</script>
```
子组件inputCount.vue
```bash
<template>
    <div>
        <input type="text" :value="count" @input="input"> 
    </div>
</template>
<script>
export default {
    props: ["count"],
    methods: {
    input(event) {
      let value = event.target.value; 
      this.$emit("getValue", value);
    }
  }
    };
</script>
```
但是试了下把两个组件反一下 输入框当父组件，按钮当子组件，写一下。按照正常逻辑，就是把输入框的值用`props`传给子组件，然后在子组件里面写了`this.count++` 最后把值用`$emit`传给父组件，但是实际上试了一下不行，在子组件里不能直接对传过来的count进行改变，只能赋值，最后重新传给父组件的话，会发现它始终是在原来的值上进行改变，这个时候如果手动把输入框里面的值变成1000在按+1 只是在原来的数上增加 不能从1000开始加得到1001。  

后来经过各种修改 实现了原来的功能，但不是警告就是报错。因为`props`的数据流动改为了只能单向流动，即只能由组件外（调用组件方）通过组件的DOM属性attribute传递props给组件内，组件内只能被动接收组件外传递过来的数据，并且在组件内，不能修改由外层传来的props数据。  

后来通过向别人求助，如果要实现原来那个功能，那就不能用`$emit`传数值，只能用来传方法，把用来改变数值的`this.count++` 写到父组件里，子组件就是传个按钮的指令。这个指令让父组件去对数值进行改变。这样就简单明了很多了。
```bash
<template>//父组件
  <div>
      <input v-model="count" >
      <MyButton @increase="getCount"  @subtract="subCount"/>
  </div>
</template>
<script>
import MyButton from "@/components/MyButton.vue";
export default {
    components:{
       MyButton
    },
    data(){
      return{
        count:5
      }
    },
    methods:{
      getCount(){
          this.count++
      },
      subCount(){
          this.count--
      }
   }
}
</script>
```
```bash
//子组件
<template>
  <div>
        <button @click="increase">+</button> <button @click="subtract">-</button>
  </div>
</template>
<script>
export default {
    props: {
        count:{
            type:Number,
            default:Infinity
        }
    },
    methods:{
      increase(){
          this.$emit('increase')
      },
      subtract(){
          this.$emit('subtract')
      }
  }
}
</script>
```
一开始自己写是各种问题，本来以为很简单的，但就是没达到效果，花了好久，现在回想，原来这样就可以了啊，自己懂的还是太少了。。  
以后如果要从子组件对父组件的值进行改变就传方法，让父组件去改变。而不是传给子组件变了后在传回来。