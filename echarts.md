 ##ECharts 
 ##使用vue-lic创建vue项目
 * 通过 npm install -g @vue/cli 安装vue-cli3
 * vue-V查看安装成功
 * vue create Echarts 创建echarts的项目文件夹 安装好后进入文件夹
 * npm install echarts --save 通过 npm 安装 ECharts
 * 在HelloWorkd.vue中直接修改
 * 通过let echarts=require('echarts') 得到 ECharts


 
 <template>
<div id="myChart" style="width: 800px;height:400px;"></div>
</template> 

<script >
let echarts=require('echarts')
export default {

  mounted(){
     this.myChart=echarts.init(this.$el);
    this.drawLine();
   
  },
  methods: {
    drawLine(){
        // 基于准备好的dom，初始化echarts实例
        
        // 绘制图表
        this.myChart.setOption({
            title: { 
              text: 'ECharts 入门示例' 
            },
            tooltip: {},
            legend: {
                data:['销量']
            },
            xAxis: {
                data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
            },
            yAxis: {},
            series: [{
                name: '销量',
                type: 'bar',
                data: [5, 20, 36, 10, 10, 20]
            }]
        });
    }
  }
}
</script>


就能得到最基础的Echarts图标，可以在上面改数据，想要其它的图片可以在http://echarts.baidu.com/echarts2/doc/example.html上找到自己想要的图表，把网页上的option里的代码复制粘贴到上面setOption({})的大括号里就能显示出来了。

 