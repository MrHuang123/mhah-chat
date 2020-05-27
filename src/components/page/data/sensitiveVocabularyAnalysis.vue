<template>
  <div class='w-800'>
    <!-- <el-date-picker
      v-model="value2"
      @change='changeTime_1'
      type="month"
      class="mgb20"
      value-format="yyyy-MM"
      placeholder="选择月">
    </el-date-picker> -->
    <el-row :gutter="20">
        <el-col :span="24">
          <el-card shadow="hover">
              <div id="char2" ref="chart2" style="width:100%;height:300px;"></div>   
              <!-- <div id='pieChart' style="width:100%;height:300px;"></div> -->
          </el-card>
      </el-col>
      
    </el-row>
    <el-date-picker
      v-model="value2"
      @change='changeTime_1'
      type="month"
      class="mgb20"
      value-format="yyyy-MM"
      placeholder="选择月">
    </el-date-picker>
    <el-row :gutter="20">
      <el-col :span="24">
        <el-card shadow="hover">
           <div id="char3" ref="chart3" style="width:100%;height:300px;"></div> 
          <!-- <schart
            ref="line"
            class="schart"
            canvasId="line"
            :data="data"
            type="line"
            :options="options2"
          ></schart> -->
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import Schart from "vue-schart";
import echarts from 'echarts';
import "echarts-wordcloud/dist/echarts-wordcloud";
import "echarts-wordcloud/dist/echarts-wordcloud.min";
import bus from "../../common/bus";
import {sensitiveData} from '../../../mock/data/sensitiveVocabularyAnalysis';
export default {
  name: "dashboard",
  data() {
    return {
      value2:'2020-04',
      name: localStorage.getItem("ms_username"),
      Echarts:null,
      worddata: [{
                        name: "爆炸",
                        value: 50
                    },
                    {
                        name: "枪支",
                        value: 20
                    },
                    {
                        name: "燃烧",
                        value: 70
                    },
                    {
                        name: "吸烟",
                        value: 78
                    },
                    {
                        name: "110",
                        value: 90
                    },
                    {
                        name: "打电话",
                        value: 30
                    },
                    {
                        name: "油罐",
                        value: 80
                    },
                    {
                        name: "打击",
                        value: 60
                    },
                    {
                        name: "报警",
                        value: 40
                    },
                    {
                        name: "游行",
                        value: 50
                    },
                    {
                        name: "逃逸",
                        value: 50
                    },
                    {
                        name: "毁灭",
                        value: 40
                    },
                    {
                        name: "毒品",
                        value: 100
                    },
                    {
                        name: "管制刀具",
                        value: 85
                    },
                    {
                        name: "政治",
                        value: 43
                    },
                    {
                        name: "犯罪",
                        value: 34
                    },
                    {
                        name: "破坏",
                        value: 22
                    },
                    {
                        name: "违法行为",
                        value: 15
                    },
                    {
                        name: "有组织",
                        value: 35
                    },
                    {
                        name: "有预谋",
                        value: 30
                    },
                ],
      options: {
        title: "最近七天每天的用户访问量",
        showValue: false,
        fillColor: "rgb(45, 140, 240)",
        bottomPadding: 30,
        topPadding: 30
      },
      options2: {
        title: "2019年各终端数据趋势月度分析",
        fillColor: "#FC6FA1",
        axisColor: "#008ACD",
        contentColor: "#EEEEEE",
        bgColor: "#F5F8FD",
        bottomPadding: 30,
        topPadding: 30
      },
      data:[],
    };
  },
  components: {
    Schart
  },
  computed: {
    role() {
      return this.name === "admin" ? "超级管理员" : "普通用户";
    }
  },
  created() {
    this.handleListener();
    this.changeDate();
  },
  activated() {
    this.handleListener();
  },
  deactivated() {
    window.removeEventListener("resize", this.renderChart);
    bus.$off("collapse", this.handleBus);
  },
  methods: {
    initCharts_2(xData,yData){
        let myChart3 = echarts.init(this.$refs.chart3);
        this.$nextTick(()=>{
            myChart3.setOption({
                // title: { text: '在Vue中使用echarts' },
                tooltip: {},
                xAxis: {
                    triggerEvent: true,
                    type: 'category',
                    data: xData,
                    axisLabel: {
                            formatter: function(value) {
                            var res = value;
                                if(res.length > 5) {
                                    res = res.substring(0, 4) + "..";
                                }
                            return res;
                        }
                    },
                },
                yAxis: {
                    splitLine:{show: false},   //去除网格线
                },
                series: [{
                    name: '接口调用次数',
                    type: 'bar',
                    itemStyle: {   
                        //通常情况下：
                        normal:{  
        　　　　　　　　　　　　//每个柱子的颜色即为colorList数组里的每一项，如果柱子数目多于colorList的长度，则柱子颜色循环使用该数组
                            color: function (params){
                                var colorList = ['rgba(194, 53, 49, 1)','rgba(47, 69, 84, 1)','rgba(97, 160, 168, 1)','rgba(212, 130, 101, 1)','rgba(145, 199, 174, 1)'];
                                return colorList[params.dataIndex];
                            },
                            label: {
                                show: true, //开启显示
                                position: 'top', //在上方显示
                                textStyle: { //数值样式
                                    color: 'black',
                                    fontSize: 12
                                }
                            }
                        },
                        //鼠标悬停时：
                        emphasis: {
                                shadowBlur: 10,
                                shadowOffsetX: 0,
                                shadowColor: 'rgba(0, 0, 0, 0.5)'
                        }
                    },
                    data: yData
                }]
                })
            // myChart3.setOption({
            //     title: {
            //         text: '敏感词Top10',
            //         // subtext: '数据来自网络'
            //     },
            //     tooltip: {
            //         trigger: 'axis',
            //         axisPointer: {
            //             type: 'shadow'
            //         }
            //     },
            //     legend: {
            //         data: ['2011年', '2012年']
            //     },
            //     grid: {
            //         left: '3%',
            //         right: '4%',
            //         bottom: '3%',
            //         containLabel: true
            //     },
            //     xAxis: {
            //         type: 'value',
            //         boundaryGap: [0, 0.01]
            //     },
            //     yAxis: {
            //         type: 'category',
            //         data: ['巴西', '印尼', '美国', '印度', '中国', '世界人口(万)']
            //     },
            //     series: [
            //         {
            //             name: '2011年',
            //             type: 'bar',
            //             data: [18203, 23489, 29034, 104970, 131744, 630230]
            //         },
            //         {
            //             name: '2012年',
            //             type: 'bar',
            //             data: [19325, 23438, 31000, 121594, 134141, 681807]
            //         }
            //     ]
            // })
        })
    },
    initCharts() {
        let myChart2 = echarts.init(this.$refs.chart2);
        this.$nextTick(()=>{
            myChart2.setOption({
                    title: {
                        //                        text: "热爱祖国",
                        x: "center"
                    },
                    backgroundColor: "#fff",
                    // tooltip: {
                    //   pointFormat: "{series.name}: <b>{point.percentage:.1f}%</b>"
                    // },
                    series: [{
                        type: "wordCloud",
                        //用来调整词之间的距离
                        gridSize: 10,
                        //用来调整字的大小范围
                        // Text size range which the value in data will be mapped to.
                        // Default to have minimum 12px and maximum 60px size.
                        sizeRange: [14, 60],
                        // Text rotation range and step in degree. Text will be rotated randomly in range [-90,                                                                             90] by rotationStep 45
                        //用来调整词的旋转方向，，[0,0]--代表着没有角度，也就是词为水平方向，需要设置角度参考注释内容
                        // rotationRange: [-45, 0, 45, 90],
                        // rotationRange: [ 0,90],
                        rotationRange: [0, 0],
                        //随机生成字体颜色 
                        textStyle: {
                            normal: {
                                color: function() {
                                    return(
                                        "rgb(" +
                                        Math.round(Math.random() * 255) +
                                        ", " +
                                        Math.round(Math.random() * 255) +
                                        ", " +
                                        Math.round(Math.random() * 255) +
                                        ")"
                                    );
                                }
                            }
                        },
                        //位置相关设置 
                        left: "center",
                        top: "center",
                        right: null,
                        bottom: null,
                        width: "200%",
                        height: "200%",
                        //数据
                        data: this.worddata
                    }]

                })
        })
                

    },
    changeTime_1(value){
       this.getData(this.value2)
    },
    changeTime_2(value){
      console.log(value,this.value3)
    },
   
    
    changeDate() {
      const now = new Date().getTime();
      this.data.forEach((item, index) => {
        const date = new Date(now - (6 - index) * 86400000);
        item.name = `${date.getFullYear()}/${date.getMonth() +
          1}/${date.getDate()}`;
      });
    },
    handleListener() {
      bus.$on("collapse", this.handleBus);
      // 调用renderChart方法对图表进行重新渲染
      window.addEventListener("resize", this.renderChart);
    },
    handleBus(msg) {
      setTimeout(() => {
        this.renderChart();
      }, 300);
    },
    renderChart() {
      this.$refs.bar.renderChart();
      this.$refs.line.renderChart();
    },
    getData(time){
      var keys=[];//定义一个数组用来接受key    
      var values_1=[];//定义一个数组用来接受value
      var values_2=[];//定义一个数组用来接受value
      var values_3=[];//定义一个数组用来接受value
      var xData = []
      var yData = []
      for(var key in sensitiveData){    
        console.log(key)
        if(time == key){
          this.data = sensitiveData[key]
           this.data.map((item,index)=>{
               xData.push(item.name)
               yData.push(item.data)
           })
        }
      }
      this.initCharts_2(xData,yData)
    //   this.showEcharts(this.data)
    //   this.showLineEcharts(keys,values_1,values_2,values_3)
      console.log(this.data)    
    }
  },
  mounted(){
      console.log(sensitiveData)
    //   this.getData(this.value2)
    this.getData(this.value2)
    this.initCharts()
    //   this.showLineEcharts(keys,values_1,values_2,values_3)
  }
};
</script>


<style scoped>
.el-row {
  margin-bottom: 20px;
}

.grid-content {
  display: flex;
  align-items: center;
  height: 100px;
}

.grid-cont-right {
  flex: 1;
  text-align: center;
  font-size: 14px;
  color: #999;
}

.grid-num {
  font-size: 30px;
  font-weight: bold;
}

.grid-con-icon {
  font-size: 50px;
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  color: #fff;
}

.grid-con-1 .grid-con-icon {
  background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
  color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
  background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
  color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
  background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
  color: rgb(242, 94, 67);
}

.user-info {
  display: flex;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 2px solid #ccc;
  margin-bottom: 20px;
}

.user-avator {
  width: 120px;
  height: 120px;
  border-radius: 50%;
}

.user-info-cont {
  padding-left: 50px;
  flex: 1;
  font-size: 14px;
  color: #999;
}

.user-info-cont div:first-child {
  font-size: 30px;
  color: #222;
}

.user-info-list {
  font-size: 14px;
  color: #999;
  line-height: 25px;
}

.user-info-list span {
  margin-left: 70px;
}

.mgb20 {
  margin-bottom: 20px;
}

.todo-item {
  font-size: 14px;
}

.todo-item-del {
  text-decoration: line-through;
  color: #999;
}

.schart {
  width: 100%;
  height: 300px;
}
</style>
