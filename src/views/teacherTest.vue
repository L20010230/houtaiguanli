<template>
    <div style="padding:15px">
        <div class='search'>
          <el-cascader v-model="value" :options="options" @change="handleChange"></el-cascader>
        </div>
        <el-table :data="tableData" border style="width: 100%">
            <el-table-column v-for="item in data" :key="item.key" :prop="item.porp" :label="item.label"></el-table-column>
            <el-table-column label="操作" align='center'>
              <template #default="scope">
                <el-button size='mini' v-if="tableData[scope.$index].isState == true" type="primary" @click="changeTest(scope.$index,scope.row)">提名同学</el-button>
                <el-button size='mini' v-if="tableData[scope.$index].isState == false" type="success" disabled>已经提名</el-button>
                <el-button size='mini' @click="exportTest(scope.$index,scope.row)">报告</el-button>
              </template>
            </el-table-column>
        </el-table>
    </div>
</template>
<script>
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
                  label:'上学期'
                },
                {
                  value:'下学期',
                  label:'下学期'
                }
              ]
            },
            {
              value:'2023年',
              label:'2023年',
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
          data: [
            { label: "班级", porp: "className"},
            { label: "学生人数", porp: "students"},
          ],
          tableData: [
            {
              className: '三年四班',
              students: 44,
              isState:true
            }, 
            {
              className: '三年六班',
              students: 13,
              isState:true
            }, 
            {
              className: '三年二班',
              students: 34,
              isState:false
            }, 
            {
              className: '三年七班',
              students: 55,
              isState:true
            }
          ]
        }
    },
    mounted(){
      this.getTime()
    },
    methods:{
      // 获取当前学期
      getTime(){
        var timestamp = Date.parse(new Date());
        var date = new Date(timestamp);
        //获取年份  
        var Y =date.getFullYear();
        //获取月份  
        var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1);
        //获取当日日期 
        var D = date.getDate() < 10 ? '0' + date.getDate() : date.getDate(); 
        console.log(Y)
        if(M<6){
          this.value = [Y + '年','上学期']
        }else{
          this.value = [Y + '年','下学期']
        }
      },
      // change date
      handleChange(e){
        let years = e[0]  // year
        let terms = e[1]  // xueqi
      },
      // 提名同学
      changeTest(index,item){
        console.log(index,item)
        // 1 parent 2 students 3 otherStudent 4 teacher
        // this.$router.push({name:'test1',query: {id:'4'}})
      },
      // 查看年级报告
      exportTest(index,item){
        // this.$router.push({name:'result',query: {id:'1'}})
      }
    }
}
</script>
<style>
.search{
    display:flex;
    margin-bottom:15px;
}
.import{
  margin-right:15px;
}
.input{
  width:200px;
  margin-right:15px;
}
</style>