<template>
  <div class="w-800">
    <el-date-picker
      v-model="value2"
      @change="changeTime_1"
      type="month"
      class="mgb20"
      value-format="yyyy-MM"
      placeholder="选择月"
    >
    </el-date-picker>
    <el-row :gutter="20">
      <el-col :span="24">
        <el-card shadow="hover">
          <div id="pieChart" style="width:100%;height:300px;"></div>
        </el-card>
      </el-col>
    </el-row>
    <!-- <el-date-picker
      v-model="value3"
      @change='changeTime_2'
      type="year"
      class="mgb20"
      value-format="yyyy"
      placeholder="选择年">
    </el-date-picker> -->
    <el-select
      class="mgb20"
      v-model="value3"
      placeholder="请选择"
      @change="changeTime_2"
    >
      <el-option label="2019年" value="2019"></el-option>
      <el-option label="2020年" value="2020"></el-option>
    </el-select>
    <el-row :gutter="20">
      <el-col :span="24">
        <el-card shadow="hover">
          <div id="lineChart" style="width:100%;height:300px;"></div>
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
import bus from "../common/bus";
import {echartsData, data_2020} from '../../mock/data/originData';
export default {
  name: "dashboard",
  data() {
    return {
      value2:'2020-04',
      value3:'2020',
      name: localStorage.getItem("ms_username"),
      Echarts:null,
      data: [
        {
          name: "2018/09/04",
          value: 1083
        },
        {
          name: "2018/09/05",
          value: 941
        },
        {
          name: "2018/09/06",
          value: 1139
        },
        {
          name: "2018/09/07",
          value: 816
        },
        {
          name: "2018/09/08",
          value: 327
        },
        {
          name: "2018/09/09",
          value: 228
        },
        {
          name: "2018/09/10",
          value: 1065
        }
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
      option3: {
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)"
        },
        legend: {
          orient: "vertical",
          x: "left",
          data: [
            "直达",
            "营销广告",
            "搜索引擎",
            "邮件营销",
            "联盟广告",
            "视频广告",
            "百度",
            "谷歌",
            "必应",
            "其他"
          ]
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            selectedMode: "single",
            radius: [0, "30%"],

            label: {
              normal: {
                position: "inner"
              }
            },
            labelLine: {
              normal: {
                show: false
              }
            },
            data: [
              { value: 335, name: "直达", selected: true },
              { value: 679, name: "营销广告" },
              { value: 1548, name: "搜索引擎" }
            ]
          },
          {
            name: "访问来源",
            type: "pie",
            radius: ["40%", "55%"],
            label: {
              normal: {
                  show:false,
                formatter: "{a|{a}}{abg|}\n{hr|}\n  {b|{b}：}{c}  {per|{d}%}  ",
                backgroundColor: "#eee",
                borderColor: "#aaa",
                borderWidth: 1,
                borderRadius: 4,
                // shadowBlur:3,
                // shadowOffsetX: 2,
                // shadowOffsetY: 2,
                // shadowColor: '#999',
                // padding: [0, 7],
                rich: {
                  a: {
                    color: "#999",
                    lineHeight: 22,
                    align: "center"
                  },
                  // abg: {
                  //     backgroundColor: '#333',
                  //     width: '100%',
                  //     align: 'right',
                  //     height: 22,
                  //     borderRadius: [4, 4, 0, 0]
                  // },
                  hr: {
                    borderColor: "#aaa",
                    width: "100%",
                    borderWidth: 0.5,
                    height: 0
                  },
                  b: {
                    fontSize: 16,
                    lineHeight: 33
                  },
                  per: {
                    color: "#eee",
                    backgroundColor: "#334455",
                    padding: [2, 4],
                    borderRadius: 2
                  }
                }
              }
            },
            data: [
              { value: 335, name: "直达" },
              { value: 310, name: "邮件营销" },
              { value: 234, name: "联盟广告" },
              { value: 135, name: "视频广告" },
              { value: 1048, name: "百度" },
              { value: 251, name: "谷歌" },
              { value: 147, name: "必应" },
              { value: 102, name: "其他" }
            ]
          }
        ]
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
   
   
  },
  activated() {
   
  },
  deactivated() {
    window.removeEventListener("resize", this.renderChart);
    bus.$off("collapse", this.handleBus);
  },
  methods: {
    changeTime_1(value){
       this.getData(this.value2)
    },
    changeTime_2(value){
      console.log(value, this.value3)

      var keys=[];//定义一个数组用来接受key    
      var values_1=[];//定义一个数组用来接受value
      var values_2=[];//定义一个数组用来接受value
      var values_3=[];//定义一个数组用来接受value
      var key_1 =  '';
      for(var key in echartsData){
        keys.push(key);
        values_1.push(echartsData[key][0].value);//取得value   
        values_2.push(echartsData[key][1].value);//取得value
        values_3.push(echartsData[key][2].value);//取得value
      }
      this.$nextTick(()=>{
        const year = value.split('-')[0];

        this.showLineEcharts(
          keys.filter(item => item.includes(year)),
          values_1,
          values_2,
          values_3,
          year + '年各终端数据趋势月度分析'
        )
      })  
    },
    showEcharts(data){
        console.log(data,222)
        var myChart=echarts.init(document.getElementById("pieChart"));
        myChart.setOption({
            title: {
                text: '终端数据占比',
                // subtext: '纯属虚构',
                left: 'center'
            },
            tooltip: {
                trigger: 'item',
                formatter: '{a} <br/>{b} : {c} ({d}%)'
            },
            // legend: {
            //     orient: 'vertical',
            //     left: 'left',
            //     data: ['直接访问', '邮件营销', '联盟广告', '视频广告', '搜索引擎']
            // },
            series: [
                {
                    name: '终端数据占比',
                    type: 'pie',
                    radius: '65%',
                    center: ['50%', '60%'],
                    data: data,
                    emphasis: {
                        itemStyle: {
                            shadowBlur: 10,
                            shadowOffsetX: 0,
                            shadowColor: 'rgba(0, 0, 0, 0.5)'
                        }
                    }
                }
            ]
        });
    },
    showLineEcharts(keys,values_1,values_2,values_3, title){
      var myChart_1=echarts.init(document.getElementById("lineChart"));
      myChart_1.setOption({
          title: {
              text: title
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data: ['涉外APP', '安防硬件', '其他']
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          toolbox: {
              feature: {
                  saveAsImage: {}
              }
          },
          xAxis: {
              type: 'category',
              boundaryGap: false,
              data: keys
          },
          yAxis: {
              type: 'value'
          },
          series: [
              {
                  name: '涉外APP',
                  type: 'line',
                  // stack: '总量',
                  data: values_1
              },
              {
                  name: '安防硬件',
                  type: 'line',
                  // stack: '总量',
                  data: values_2
              },
              {
                  name: '其他',
                  type: 'line',
                  // stack: '总量',
                  data: values_3
              },
          ]
      })
    },
    // handleListener() {
    //   bus.$on("collapse", this.handleBus);
    //   // 调用renderChart方法对图表进行重新渲染
    //   window.addEventListener("resize", this.renderChart);
    // },
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
      var key_1 =  '';
      for(var key in echartsData){
        keys.push(key);
        values_1.push(echartsData[key][0].value);//取得value   
        values_2.push(echartsData[key][1].value);//取得value
        values_3.push(echartsData[key][2].value);//取得value
      } 
      this.data = echartsData[time]
      this.$nextTick(()=>{
        this.showEcharts(this.data)
        const year = time.split('-')[0];

        this.showLineEcharts(
          keys.filter(item => item.includes(year)),
          values_1,
          values_2,
          values_3,
          year + '年各终端数据趋势月度分析'
        )
      })  
    }
  },
  mounted(){
      this.getData(this.value2)
      // this.showLineEcharts(keys,values_1,values_2,values_3)
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
