### 组件
#### 组件之间的传值
1. 父组件向子组件传递
   1. 创建子组件，在src/components/文件夹下新建一个inputCount.vue文件
   2. 在inputCount.vue中创建props，进行传值，添加count
   3. 在父组件中注册并导入子组件 添加 
    ```bash
    import inputCount from "@/components/inputCount.vue"  
    ```
      在components中添加inputCount 把数据绑定到子组件上，
    ```bash
    <inputCount :count="count"></inputCount>
    ```
2. 子组件向父组件传递数据
   1. 子组件主要通过事件传递数据给父组件
   2. 使用了 $emit 来遍历 getValue 事件
   3. 父组件部分用 @input="input input事件回调input方法，获取到从子组件传递过来的value值。

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