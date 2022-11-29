<template>
    <div style="padding:15px">
        <div class='search'>
          <el-input v-model="searchName" placeholder="请输入要查询的提交人" class='input'></el-input>
          <el-date-picker v-model="searchDate" type="daterange" @change="changeTime" range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期"> </el-date-picker>
          <el-button type="primary" style="margin-left:15px" :disabled='searchName == "" && searchDate == "" ' plain class='button' @click="search">查询</el-button>
        </div>
        <el-table :data="tableData" border style="width: 100%">
            <el-table-column v-for="item in data" :key="item.key" :prop="item.porp" :label="item.label"></el-table-column>
            <el-table-column label="操作" align='center'>
              <template #default="scope">
                <el-button size='mini' type="primary" @click="changeImport(scope.$index, scope.row)">查看</el-button>
              </template>
            </el-table-column>
        </el-table>
    </div>
</template>
<script>
export default {
  data(){
      return{
        searchName:'',
        searchDate:'',
        data: [
            { label: "提交人", porp: "className"},
            { label: "提交时间", porp: "students"},
            { label: "问卷名称", porp: "title"},
        ],
        tableData: [
            {
            className: '李子',
            students: '2022-07-22',
            title:'心理测试问卷'
            }, 
            {
            className: '李子',
            students: '2022-07-22',
            title:'心理测试问卷'
            }, 
            {
            className: '李子',
            students: '2022-07-22',
            title:'心理测试问卷'
            }, 
            {
            className: '李子',
            students: '2022-07-22',
            title:'心理测试问卷'
            }
        ],
        startTime:'',
        endTime:''
      }
  },
  methods:{
    // 查看修改记录
    changeImport:function(index,item){
      console.log(index,item)
    },
    changeTime(e){
      function formatTen(num) { 
        return num > 9 ? (num + "") : ("0" + num); 
      }
      function formatDate(date) { 
        var year = date.getFullYear(); 
        var month = date.getMonth() + 1; 
        var day = date.getDate(); 
        return year + "-" + formatTen(month) + "-" + formatTen(day); 
      }
      this.startTime = formatDate(this.searchDate[0])
      this.endTime = formatDate(this.searchDate[1])
    },  
    search(){
      if(this.input == '' && this.startTime == ''){
        this.$message.error('请填写你要查询的条件哦~');
      }else{
        this.$message('开始查询~');
      }
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