<template>
    <div>
        
        <el-row :gutter="20">
            <el-col :span="24">
                <el-card shadow="hover">
                    <div id='lineChart' style="width:100%;height:500px;"></div>
                </el-card>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import echarts from 'echarts';
import json from '../../../json/speechAnalysis.js'
export default {
    data(){
        return {
            month:'2019-01',
        }
    },
    mounted(){
      this.showLineEcharts()
    },
    methods:{
        showLineEcharts(data){
            var myEcharts=echarts.init(document.getElementById("lineChart"));
            myEcharts.setOption({
                title: {
                    text: '语音转写分析-语种分类&语音总数'
                },
                tooltip: {
                    trigger: 'axis'
                },
                legend: {
                    data: ['缅甸语', '越南语','英语','法语','德语', '日语','韩语','其他']
                },
                toolbox: {
                    show: true,
                    feature: {
                        dataView: {show: true, readOnly: false},
                        magicType: {show: true, type: ['line', 'bar']},
                        restore: {show: true},
                        saveAsImage: {show: true}
                    }
                },
                calculable: true,
                xAxis: [
                    {
                        type: 'category',
                        data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月']
                    }
                ],
                yAxis: [
                    {
                        type: 'value'
                    }
                ],
                series: [
                    {
                        name: '缅甸语',
                        type: 'bar',
                        data: [ 
                            1138,
                            1370,
                            1204,
                            1223,
                            1121,
                            1116,
                            1158,
                            1413,
                            1059,
                            1280,
                            1318,
                            1261
                        ],
                    },
                    {
                        name: '越南语',
                        type: 'bar',
                        data: [ 
                            1588,
                            1932,
                            1601,
                            1743,
                            1785,
                            1988,
                            1682,
                            1569,
                            1811,
                            1794,
                            1961,
                            1996
                        ],
                        
                    },
                    {
                        name: '英语',
                        type: 'bar',
                        data: [ 
                            916,
                            924,
                            829,
                            911,
                            821,
                            939,
                            896,
                            955,
                            958,
                            965,
                            898,
                            816
                        ],
                        
                    },
                    {
                        name: '法语',
                        type: 'bar',
                        data: [
                            444,
                            229,
                            546,
                            352,
                            500,
                            755,
                            789,
                            522,
                            582,
                            280,
                            445,
                            727
                        ],
                        
                    },
                    {
                        name: '德语',
                        type: 'bar',
                        data: [
                            509,
                            303,
                            463,
                            417,
                            554,
                            294,
                            635,
                            559,
                            656,
                            352,
                            308,
                            553
                        ],
                        
                    },
                    {
                        name: '日语',
                        type: 'bar',
                        data: [
                            435,
                            673,
                            422,
                            717,
                            202,
                            605,
                            655,
                            524,
                            323,
                            373,
                            206,
                            549
                        ],
                        
                    },
                    {
                        name: '韩语',
                        type: 'bar',
                        data: [
                            421,
                            215,
                            744,
                            491,
                            401,
                            281,
                            570,
                            741,
                            426,
                            409,
                            489,
                            794
                        ],
                        
                    },
                    {
                        name: '其他',
                        type: 'bar',
                        data: [
                            355,
                            380,
                            373,
                            374,
                            360,
                            705,
                            316,
                            334,
                            604,
                            441,
                            562,
                            428
                        ],
                        
                    }

                ]
            })
        }
    }
}
</script>