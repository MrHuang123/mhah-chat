<template>
  <div>
    <h3 style="margin-bottom:20px">语音数据专项分析-数据明细表</h3>
    <el-table :data="list" style="width: 100%">
      <el-table-column prop="sourceText" label="音频识别文本">
      </el-table-column>
      <el-table-column prop="sourceLanguage" label="音频原始语种" width="120">
      </el-table-column>
      <el-table-column prop="targetLanguage" label="翻译目标语种" width="120">
      </el-table-column>
      <el-table-column prop="targetText" label="译文转写文本">
      </el-table-column>
      <el-table-column prop="date" label="日期" width="200"> </el-table-column>
    </el-table>
    <el-pagination
      style="float:left;padding-right:0"
      layout="pager"
      :total="20"
      @current-change="handleCurrentChange"
    ></el-pagination>
    <ul class="el-pager disabled" style="padding: 2px 0px;" :disabled="true">
      <li class="">3</li>
      <li class="">4</li>
      <li class="">5</li>
      <li class="">6</li>
      <li class="">···</li>
    </ul>
    <span class="total">总计: 56121</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      tableData1: [
        {
          sourceText: "Excuse me, where is the nearest hotel nearby?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "请问，附近最近的酒店在哪里？",
          date: "2020/05/19"
        },
        {
          sourceText:
            "I lost my passport, I want to call the police, what should I do?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "护照遗失，我想报警，怎么处理？",
          date: "2020/05/19"
        },
        {
          sourceText: "How much is alcohol and lighter?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "酒精和打火机一共多少钱？",
          date: "2020/05/19"
        },
        {
          sourceText:
            "I want to find a guide tomorrow, take me to visit Nanning",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "我明天想找一名导游，带我游览南宁市",
          date: "2020/05/19"
        },
        {
          sourceText: "Which attractions in Nanning are the most fun",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "南宁哪个景点最好玩",
          date: "2020/05/19"
        },
        {
          sourceText: "Xin lỗi, khách sạn gần nhất ở đâu?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "请问，附近最近的酒店在哪里？",
          date: "2020/05/19"
        },
        {
          sourceText:
            "Tôi bị mất hộ chiếu, tôi muốn gọi cảnh sát, tôi phải làm sao?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "护照遗失，我想报警，怎么处理？",
          date: "2020/05/19"
        },
        {
          sourceText: "Bao nhiêu là rượu và nhẹ hơn?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "酒精和打火机一共多少钱？",
          date: "2020/05/19"
        },
        {
          sourceText:
            "Tôi muốn tìm một hướng dẫn vào ngày mai, đưa tôi đến thăm Nam Ninh",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "我明天想找一名导游，带我游览南宁市",
          date: "2020/05/19"
        },
        {
          sourceText: "Những điểm thu hút ở Nam Ninh là thú vị nhất",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "南宁哪个景点最好玩",
          date: "2020/05/19"
        },
        {
          sourceText: "Huyện nào của Nam Ninh có cư dân thường trú nhất?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "南宁哪个区常驻人口最多",
          date: "2020/05/19"
        },
        {
          sourceText: "Excuse me, where is the nearest hotel nearby?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "请问，附近最近的酒店在哪里？",
          date: "2020/05/18"
        },
        {
          sourceText:
            "I lost my passport, I want to call the police, what should I do?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "护照遗失，我想报警，怎么处理？",
          date: "2020/05/18"
        },
        {
          sourceText: "How much is alcohol and lighter?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "酒精和打火机一共多少钱？",
          date: "2020/05/18"
        },
        {
          sourceText:
            "I want to find a guide tomorrow, take me to visit Nanning",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "我明天想找一名导游，带我游览南宁市",
          date: "2020/05/18"
        },
        {
          sourceText: "Which attractions in Nanning are the most fun",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "南宁哪个景点最好玩",
          date: "2020/05/18"
        },
        {
          sourceText:
            "Which district of Nanning has the most permanent residents?",
          sourceLanguage: "英文",
          targetLanguage: "中文",
          targetText: "南宁哪个区常驻人口最多",
          date: "2020/05/18"
        }
      ],
      tableData2: [
        {
          sourceText: "先生，加油站里不允使用手机",
          sourceLanguage: "中文",
          targetLanguage: "英文",
          targetText: "Sir, mobile phones are not allowed in gas stations",
          date: "2020/05/18"
        },
        {
          sourceText: "过了这个服务区，马上就可以上高速了",
          sourceLanguage: "中文",
          targetLanguage: "英文",
          targetText:
            "After this service area, you can get on the high speed immediately",
          date: "2020/05/18"
        },
        {
          sourceText: "这属于违禁物品，不允许随身携带",
          sourceLanguage: "中文",
          targetLanguage: "英文",
          targetText:
            "This is a prohibited item and is not allowed to be carried around",
          date: "2020/05/18"
        },
        {
          sourceText: "如果可以，我带您去最近的警察局吧",
          sourceLanguage: "中文",
          targetLanguage: "英文",
          targetText:
            "If you can, I will take you to the nearest police station",
          date: "2020/05/18"
        },
        {
          sourceText: "Xin lỗi, khách sạn gần nhất ở đâu?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "请问，附近最近的酒店在哪里？",
          date: "2020/05/17"
        },
        {
          sourceText:
            "Tôi bị mất hộ chiếu, tôi muốn gọi cảnh sát, tôi phải làm sao?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "护照遗失，我想报警，怎么处理？",
          date: "2020/05/17"
        },
        {
          sourceText: "Bao nhiêu là rượu và nhẹ hơn?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "酒精和打火机一共多少钱？",
          date: "2020/05/17"
        },
        {
          sourceText:
            "Tôi muốn tìm một hướng dẫn vào ngày mai, đưa tôi đến thăm Nam Ninh",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "我明天想找一名导游，带我游览南宁市",
          date: "2020/05/17"
        },
        {
          sourceText: "Những điểm thu hút ở Nam Ninh là thú vị nhất",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "南宁哪个景点最好玩",
          date: "2020/05/17"
        },
        {
          sourceText: "Huyện nào của Nam Ninh có cư dân thường trú nhất?",
          sourceLanguage: "越南语",
          targetLanguage: "中文",
          targetText: "南宁哪个区常驻人口最多",
          date: "2020/05/17"
        },
        {
          sourceText: "Entschuldigung, wo ist das nächste Hotel in der Nähe?",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "请问，附近最近的酒店在哪里？",
          date: "2020/05/16"
        },
        {
          sourceText:
            "Ich habe meinen Pass verloren, ich möchte die Polizei rufen, was soll ich tun?",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "护照遗失，我想报警，怎么处理？",
          date: "2020/05/16"
        },
        {
          sourceText: "Wie viel ist Alkohol und leichter?",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "酒精和打火机一共多少钱？",
          date: "2020/05/16"
        },
        {
          sourceText:
            "Ich möchte morgen einen Führer finden und mich zu Nanning bringen",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "我明天想找一名导游，带我游览南宁市",
          date: "2020/05/16"
        },
        {
          sourceText: "Welche Attraktionen in Nanning machen am meisten Spaß?",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "南宁哪个景点最好玩",
          date: "2020/05/16"
        },
        {
          sourceText:
            "Welcher Bezirk von Nanning hat die dauerhaftesten Einwohner?",
          sourceLanguage: "德语",
          targetLanguage: "中文",
          targetText: "南宁哪个区常驻人口最多",
          date: "2020/05/16"
        }
      ]
    };
  },
  mounted() {
    this.list = this.tableData1;
  },
  methods: {
    handleCurrentChange(val) {
      if (val == 1) {
        this.list = this.tableData1;
      } else if (val == 2) {
        this.list = this.tableData2;
      }
    }
  }
};
</script>
<style scoped>
/deep/ .total {
  margin-left: 20px;
  display: inline-block;
  height: 32px;
  line-height: 32px;
  font-size: 14px;
}
.disabled li {
  color: #999;
  cursor: not-allowed;
}
.disabled li:hover {
  color: #999;
}
</style>