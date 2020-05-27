<template>
  <div>
    <el-row :gutter="20">
      <el-col :span="24">
        <el-date-picker
          v-model="month"
          type="month"
          placeholder="选择月"
          style="margin-bottom:10px"
          @change="changeMonth"
        >
        </el-date-picker>
        <el-card shadow="hover">
          <div id="pieChart" style="width:100%;height:300px;"></div>
        </el-card>
      </el-col>
    </el-row>
    <br />
    <el-row :gutter="20">
      <el-col :span="24">
        <div style="margin-bottom:20px">
          <el-select v-model="year" placeholder="请选择" @change="changeTime">
            <el-option label="2019年" value="2019"></el-option>
            <el-option label="2020年" value="2020"></el-option>
          </el-select>
        </div>
        <el-card shadow="hover">
          <div id="lineChart" style="width:100%;height:300px;"></div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import echarts from "echarts";
import json from "../../../json/dataTypeStatistics.js";
export default {
  data() {
    return {
      month: "2020-04",
      year: "2020"
    };
  },
  mounted() {
    this.showPieEcharts();
    this.showLineEcharts();
  },
  methods: {
    showPieEcharts(data) {
      var myEcharts = echarts.init(document.getElementById("pieChart"));
      myEcharts.setOption({
        title: {
          text: "数据类型统计"
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)"
        },
        series: [
          {
            name: "访问来源",
            type: "pie",
            selectedMode: "single",
            radius: [0, "80%"],
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
            data: data || [
              { value: 8762, name: "文本" },
              { value: 6400, name: "音频" },
              { value: 4600, name: "视频" }
            ]
          }
        ]
      });
    },
    showLineEcharts(data, title) {
      var myEcharts = echarts.init(document.getElementById("lineChart"));
      myEcharts.setOption({
        title: {
          text: title || "2020年各终端数据趋势月度分析"
        },
        tooltip: {
          trigger: "axis"
        },
        legend: {
          data: ["涉外APP", "安防硬件", "其他"]
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: [
            "一月",
            "二月",
            "三月",
            "四月",
            "五月",
            "六月",
            "七月",
            "八月",
            "九月",
            "十月",
            "十一月",
            "十二月"
          ]
        },
        yAxis: {
          type: "value"
        },
        series: data || json[2020]
      });
    },
    changeMonth(e) {
      let data = [
        { value: json[2019][0].data[e.getMonth()], name: "文本" },
        { value: json[2019][1].data[e.getMonth()], name: "音频" },
        { value: json[2019][2].data[e.getMonth()], name: "视频" }
      ];
      this.showPieEcharts(data);
    },
    changeTime(value) {
      this.showLineEcharts(json[value], value + "年各终端数据趋势月度分析");
    }
  }
};
</script>