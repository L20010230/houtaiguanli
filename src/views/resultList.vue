<template>
  <div class="container">
    <div class="list">
        <el-cascader v-model="value" :options="options" @change="handleChange"></el-cascader>
    </div>
    <div class="content">
      <div class="title">一、自评的学科能力及兴趣</div>
      <div style="display:flex">
        <div id="myChart" :style="{ width: '600px', height: '400px',margin:'15px 0px' }"></div>
        <div id="myChart1" :style="{ width: '600px', height: '400px',margin:'15px 0px' }"></div>
      </div>
      <div class="title">二、专业兴趣</div>
      <div id="lineChart" :style='{ float: "left", width: "100%", height: "400px" }'></div>
      <div class="title">三、同学评价的学习能力</div>
      <div class="pingjia">                                           
        <div v-for="(item,index) in pingjia" :key="index">
          {{item.tit}} : {{item.num}}
        </div>
      </div>
      <div class="title">四、教师评价的学习能力：</div>
      <div class="pingjia">                                           
        <div v-for="(item,index) in pingjia" :key="index">
          {{item.tit}} : {{item.num}}
        </div>
      </div>
      <div class="title">五、自我分析</div>
      <div class="fenxi">
        <div>1.你的能力、兴趣、未来你期望包括专业三者之间是否一致?<span class="bottom"></span>如不一致你打算怎么解决？<span class="bottom"></span>如果一致，你希望的选科是哪三科？<span class="bottom"></span>志愿填报是哪几个？<span class="bottom"></span>                                 </div>
        <div>2.你眼中的学科能力和老师、同学眼中对你学科能力的看法是否一致？<span class="bottom"></span></div>
        <div>3.你的高考专业志愿填报期望和父母是否一致？<span class="bottom"></span>如不一致请家庭内部沟通讨论。<span class="bottom"></span></div>
      </div>
    </div>
    <!-- <div class="empty">
      <img src="../public/empty.png" alt="">
      <div>暂无内容</div>
    </div> -->
  </div>
</template>
<script>
import * as echarts from 'echarts'
export default {
  data(){
    return{
        value:'',
        options:[
            {
            value:'2022年',
            label:'2022年',
            children:[
                {
                value:'上学期',
                label:'上学期',
                children:[
                    {
                    value:'第一问卷',
                    label:'第一问卷'
                    },
                    {
                    value:'第二问卷',
                    label:'第二问卷'
                    },
                ]
                },
                {
                value:'下学期',
                label:'下学期',
                }
            ]
            },
            {
            value:'2023年',
            label:'2023年',
            children:[
                {
                value:'上学期',
                label:'上学期',
                children:[
                    {
                    value:'第一问卷',
                    label:'第一问卷'
                    },
                    {
                    value:'第二问卷',
                    label:'第二问卷'
                    },
                ]
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2024年',
            label:'2024年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2025年',
            label:'2025年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2026年',
            label:'2026年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2027年',
            label:'2027年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2028年',
            label:'2028年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
            {
            value:'2029年',
            label:'2029年',
            children:[
                {
                value:'上学期',
                label:'上学期'
                },
                {
                value:'下学期',
                label:'下学期'
                }
            ]
            },
        ],
        pingjia:[
            {
            tit:'历史优势突出程度',
            num:'7/42'
            },
            {
            tit:'物理优势突出程度',
            num:'7/52'
            },
            {
            tit:'地理优势突出程度',
            num:'突出'
            },
            {
            tit:'化学优势突出程度',
            num:'7/42'
            },
            {
            tit:'政治优势突出程度',
            num:'正常'
            },
            {
            tit:'生物优势突出程度',
            num:'7/42'
            },
        ]
     }
  },
  mounted(){
    this.getTime()
    this.getLoadEcharts();
    this.initEcharts();
  },
  methods:{
    getTime(){
      var timestamp = Date.parse(new Date());
      var date = new Date(timestamp);
      //获取年份  
      var Y =date.getFullYear();
      //获取月份  
      var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1);
      //获取当日日期 
      var D = date.getDate() < 10 ? '0' + date.getDate() : date.getDate(); 
      if(M<6){
        this.value = [Y + '年','上学期']
      }else{
        this.value = [Y + '年','下学期']
      }
    },
    initEcharts() {
      const myChart1 = echarts.init(document.getElementById("lineChart"));
      const mulColumnZZTData = {
        xAxis: {
          data: ["理", "工", "农", "医", "文", "史", "哲",'艺','经','管','法','教']
        },
        // 图例
        legend: {
          data: ["家长期待", "学生期待"],
          top: "0%"
        },
        yAxis: {},
        series: [
          {
            type: "bar", //形状为柱状图
            data: [3, 4, 2, 1, 4, 5, 3,4,6,2,4,1],
            name: "家长期待", // legend属性
            label: {
              // 柱状图上方文本标签，默认展示数值信息
              show: true,
              position: "top"
            }
          },
          {
            type: "bar", //形状为柱状图
            data: [3, 4, 2, 1, 4, 5, 3,4,6,2,4,1],
            name: "学生期待", // legend属性
            label: {
              // 柱状图上方文本标签，默认展示数值信息
              show: true,
              position: "top"
            }
          }
        ]
      };
      myChart1.setOption(mulColumnZZTData);
    },
    getLoadEcharts() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = echarts.init(document.getElementById('myChart'))
      let myChart1 = echarts.init(document.getElementById('myChart1'))
      let dataMax = [
        { name: '历史', max: '100' },
        { name: '生物', max: '100' },
        { name: '化学', max: '100' },
        { name: '物理', max: '102' },
        { name: '地理', max: '100' },
        { name: '政治', max: '100' },
      ]
      let option1 = {
        color:'blue',
        legend: {
          data: ['自我眼中的兴趣结构'],
        },
        radar: {
          name: {
            show: true,
            color: 'black',
          },
          indicator: dataMax,
        },
        series: [
          {
            type: 'radar',
            label: {
              show: true, 
            },
            data: [
              {
                name: '自我眼中的兴趣结构',
                value: [70, 30, 50, 12, 60],
              },
            ],
          },
        ],
      }
      let option = {
        legend: {
          data: ['自我眼中的能力结构']
        },
        radar: {
          name: {
            show: true,
            color: 'black',
          },
          indicator: dataMax,
        },
        series: [
          {
            type: 'radar',
            label: {
              show: true, 
            },
            data: [
              {
                name: '自我眼中的能力结构',
                value: [80, 90, 80, 82, 90],
              },
            ],
          },
        ],
      }
      // 绘制图表
      myChart.setOption(option)
      myChart1.setOption(option1)
    },
    handleChange(e){
      // console.log(e[0],e[1])
    },
  }
}
</script>
<style scoped lang="less">
.container {
  padding: 15px;
}
#echart {
  width: 100%;
  height: 100%;
}
.bottom{
  border-bottom: 1px solid black;
  padding: 0px 15px;
}
.pingjia{
  display: flex;
  margin-top: 15px;
  flex-wrap: wrap;
  div{
    width: 50%;
    height: 40px;
    font-size: 14px;
    line-height: 40px;
  }
}
.fenxi div{
  padding: 10px 0px;
  font-size: 14px;
}

.content{
  width: 100%;
}
.empty {
  margin-top: 200px;
}
.empty img {
  display: block;
  margin: auto;
}
.empty div {
  height: 40px;
  line-height: 40px;
  text-align: center;
  font-size: 12px;
  color: #666;
}
.list {
  display: flex;
  margin-bottom: 30px;
}
.title {
  font-weight: 500;
  font-size: 20px;
  margin-top: 15px;
}
.tips {
  margin: 10px 0px;
}
.splitLine {
  height: 2px;
  margin: 15px 0px;
  background: #f4f4f5;
}
.line1 {
  font-size: 16px;
  text-indent: 32px;
  line-height: 24px;
}
</style>