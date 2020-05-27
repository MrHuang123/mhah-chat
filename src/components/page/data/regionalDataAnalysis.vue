<template>
  <div>
    <el-date-picker
      v-model="month"
      type="month"
      placeholder="选择月"
      value-format="yyyy-MM"
      style="margin-bottom:10px"
      @change="changeMonth"
    >
    </el-date-picker>
    <div
      :id="chinaData.id"
      :style="{ width: '100%', height: chinaData.height + 'px' }"
    ></div>
  </div>
</template>

<script>
import echarts from "echarts";
import cityIdList from "../../../../static/map/city/china-main-city-map.js";
import { mapData } from "../../../mock/data/map.js";

export default {
  data() {
    return {
      chinaData: {
        id: "china",
        height: "500",
        title: "china 地图下钻"
      },
      month: "2020-04",
      myChart: null
    };
  },
  mounted() {
    this.setOptions();
  },
  methods: {
    changeMonth(val) {
      this.renderMap("guangxi", mapData[val]);
    },
    setOptions() {
      this.myChart = echarts.init(document.getElementById(this.chinaData.id));

      const guangxiJson = require("../../../../static/map/province/guangxi.json");
      //注册地图
      echarts.registerMap("guangxi", guangxiJson);
      //绘制地图
      this.renderMap("guangxi", mapData["2020-04"]);
    },
    //初始化绘制全国地图配置
    renderMap(map, data) {
      let option = {
        backgroundColor: "#fff",
        title: {
          text: "Echarts3 中国地图下钻至县级",
          subtext: "三级下钻",
          link: "",
          left: "center",
          textStyle: {
            color: "#fff",
            fontSize: 16,
            fontWeight: "normal",
            fontFamily: "Microsoft YaHei"
          },
          subtextStyle: {
            color: "#333",
            fontSize: 14,
            fontWeight: "normal",
            fontFamily: "Microsoft YaHei"
          }
        },
        tooltip: {
          trigger: "item"
          // formatter: "{b}{c}"
        },
        toolbox: {
          show: false,
          orient: "vertical",
          left: "right",
          top: "center",
          feature: {
            dataView: { readOnly: false },
            restore: {},
            saveAsImage: {}
          },
          iconStyle: {
            normal: {
              color: "#fff"
            }
          }
        },
        visualMap: {
          left: 20,
          bottom: 20,
          min: 0,
          max: 50000,
          text: ["High", "Low"],
          realtime: false,
          calculable: true,
          inRange: {
            color: ["#009999", "#00CC99", "#FFDD22", "#7700AA", "#99CC00", "#FFCC66", "#FFDD22", "#772244", "#FFDD22", "#330066", "#0000BB", "#EEDDAA", "#BB55FF", "#FF8822"]
          }
        },
        animationDuration: 1000,
        animationEasing: "cubicOut",
        animationDurationUpdate: 1000
      };

      option.title.subtext = "广西省";
      option.series = [
        {
          name: "广西省",
          type: "map",
          mapType: "guangxi",
          roam: false,
          nameMap: {
            china: "中国"
          },
          label: {
            normal: {
              show: true,
              textStyle: {
                color: "#666",
                fontSize: 13
              }
            },
            emphasis: {
              show: true,
              textStyle: {
                color: "#fff",
                fontSize: 13
              }
            }
          },
          itemStyle: {
            normal: {
              areaColor: "#323c48",
              borderColor: "dodgerblue"
            },
            emphasis: {
              areaColor: "#009688"
            }
          },
          data: data
        }
      ];
      //渲染地图
      this.myChart.setOption(option);
    }
  }
};
</script>