<template>
    <div class='box'>
        <div class='search'>
          <el-button @click="editUser">新增</el-button>
          <el-button style="margin-left:15px;" :loading="downloadLoading" type="primary" @click="handleDownload">导出</el-button>
          <el-input style="margin-left:15px;" v-model="searchName" placeholder="请输入要查询的姓名" class='input'></el-input>
          <el-input v-model="searchPhone" placeholder="请输入要查询的手机号" class='input'></el-input>
          <el-button type="primary" plain class='button' @click='searchMess' :disabled='searchName == "" && searchPhone == "" '>查询</el-button>
        </div>
        <el-table size='mini' :data="tableData" border highlight-current-row style="width: 100%;">
            <el-table-column v-for="(item,index) of tableHeader" :key="index" :prop="item.id" :label="item.name" />
            <el-table-column prop="chanaddStudentge" label="操作" align='center'>
              <template #default="scope">
                <el-button size='mini' type="" @click="changeUser">修改</el-button>
                <el-button size='mini' type="danger" @click="deletUser(scope.$index, scope.row)">删除</el-button>
              </template>
            </el-table-column>
        </el-table>
          <el-dialog v-model="changeState" :title="dialogType==='edit'?'修改信息':'新增账号'" width="40%">
            <div class="newList"><div>姓名：</div><el-input class="newlist_input" v-model="user_name" placeholder="请输入姓名"></el-input></div>
            <div class="newList"><div>账号：</div><el-input type="number" maxlength="11" class="newlist_input" v-model="userNumber" placeholder="请输入账号"></el-input></div>
            <div class="newList"><div>密码：</div><el-input show-password class="newlist_input" v-model="userPassword" placeholder="请输入密码"></el-input></div>
            <div class="newList">
              <div>角色：</div>
              <el-select v-model="userRelo" placeholder="请选择" class='newlist_input'>
                <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"></el-option>
              </el-select>
            </div>
            <div class="dialog-footer">
              <el-button @click="changeState = false">取 消</el-button>
              <el-button type="primary" @click="confirm">确 定</el-button>
            </div>
          </el-dialog>
        <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" 
            :current-page="currentPage" :page-sizes="[10, 200, 300, 400]"
            :page-size="10" layout="total, sizes, prev, pager, next, jumper" :total="tableData.length" class='page'>
        </el-pagination>
    </div>
</template>

<script>
import UploadExcelComponent from '@/components/UploadExcel/index.vue'
  export default {
    components: { UploadExcelComponent },
    data() {
      return {
        searchName:'', // 查询的名字
        searchPhone:'', // 查询的手机号
        userNumber:'',
        user_name:'',
        userPassword:'',
        userRelo:'',
        dialogType:'',
        changeState: false,
        tableHeader:[
          {
            id:'name',
            name:'姓名'
          },
          {
            id:'phone',
            name:'联系方式'
          },
          {
            id:'role',
            name:'校长'
          },
        ],
        tableData: [
          {
            'name':'李美丽',
            'phone':13333333333,
            'role':'校长',
          },
          {
            'name':'李美丽',
            'phone':13333333333,
            'role':'校长',
          },
          {
            'name':'李美丽',
            'phone':13333333333,
            'role':'校长',
          },
          {
            'name':'李美丽',
            'phone':13333333333,
            'role':'校长',
          },
        ],
        currentPage:1,
        downloadLoading: false,
        options: [
          {
            value: 1,
            label: '管理员'
          }, 
          {
            value: 2,
            label: '校长'
          }, 
          {
            value: 3,
            label: '教师'
          }, 
          {
            value: 4,
            label: '学生'
          }
        ],
      }
    },
    methods: {
      // 添加账号
      editUser(){
        this.changeState = true
        this.dialogType = 'change'
      },
      // 修改账号
      changeUser(){
        this.changeState = true
        this.dialogType = 'edit'
      },
      // 查询
      searchMess(){
        console.log(this.searchName,this.searchPhone)
      },
      handleSizeChange(val) {
        console.log(val);
      },
      handleCurrentChange(val) {
        console.log(val);
      },
      // 确认修改信息
      confirm(){
        if(this.user_name == ''){
          this.$message.error('您还没有填写姓名哦~');
        }else if(this.userNumber == '' || this.userNumber.length < 11){
          this.$message.error('请您检查账号的正确性哦~');
        }else if(this.userPassword == ''){
          this.$message.error('您还没有填写密码哦~');
        }else if(this.userRelo == ''){
          this.$message.error('您还没有分配角色哦~');
        }
      },
      // 删除账号
      deletUser(e){
        this.$confirm('此操作将永久删除该教师, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this._data.tableData.splice(this._data.tableData.indexOf(e),1);
          console.log(this._data.tableData)
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      // 导入xlsx
      beforeUpload(file) {
        const isLt1M = file.size / 1024 / 1024 < 1
        if (isLt1M) {
          return true
        }
        this.$message({
          message: 'Please do not upload files larger than 1m in size.',
          type: 'warning'
        })
        return false
      },
      // 导入成功后
      handleSuccess({ results, header }) {
        this.tableData = this.tableData.concat(results)
        console.log(results,this.tableData)
        // this.tableData = results
        this.tableHeader = header
      },
      handleDownload() {
        this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['name', 'phone', 'role']
          const filterVal = ['name', 'phone', 'role']
          const list = this.tableData
          const data = this.formatJson(filterVal, list)
          excel.export_json_to_excel({
            header: tHeader,
            data,
          })
          this.downloadLoading = false
        })
      },
      formatJson(filterVal, jsonData) {
        return jsonData.map(v => filterVal.map(j => {
          if (j === 'timestamp') {
            return parseTime(v[j])
          } else {
            return v[j]
          }
        }))
      }
    },
  }
</script>
<style scoped>
.box{
  padding:15px;
}
.newList{
  display: flex;
  margin-bottom: 15px;
}
.newList>div{
  width: 20%;
  line-height: 36px;
}
.newList>.newlist_input{
  width: 80%;
}
.table{
  border:1px solid #dfe6ec;
  border-bottom:none;
}
.el-table__body,.el-table__header{
  width:100% !important;
}
.page{
  margin-top:20px;
}
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
.button{
  
}
</style>