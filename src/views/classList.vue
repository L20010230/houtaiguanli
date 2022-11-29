<template>
    <div>
      <div class='search'>
        <upload-excel-component :on-success="handleSuccess" :before-upload="beforeUpload" />
        <el-button :loading="downloadLoading" style="margin-left:15px;" type="primary" @click="handleDownload">导出</el-button>
        <el-button style="position:absolute;right:110px" type="primary" @click="addTeacher">绑定教师</el-button>
        <el-tag style="position:absolute;right:15px;height:36px;line-height:34px">共{{tableData.length}}名学生</el-tag>
      </div>
      <div class='box'>
        <div class='left'>
          <el-tree :data="classList" :props="defaultProps" @node-click="handleNodeClick"></el-tree>
        </div>
        <div class='right'>
          <el-table size='mini' :data="tableData" border highlight-current-row style="width: 100%;">
              <el-table-column v-for="(item,index) of tableHeader" :key="index" :prop="item.id" :label="item.name" />
              <el-table-column prop="chanaddStudentge" label="操作" align='center'>
                <template #default="scope">
                  <el-button size='mini' @click="changePass(scope.$index, scope.row)">修改密码</el-button>
                  <el-button size='mini' type="success" @click="exportTest(scope.$index, scope.row)">报告</el-button>
                </template>
                <!-- {{ $t('login.logIn') }} -->
              </el-table-column>
          </el-table>
          <!-- 
            <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" 
                :current-page="currentPage4" :page-sizes="[100, 200, 300, 400]"
                :page-size="100" layout="total, sizes, prev, pager, next, jumper" :total="400" class='page'>
            </el-pagination>
          -->
        </div>
      </div>
      <el-dialog v-model="bindTeacherType" title='绑定教师' width="40%">
        <div class="newList"><div>姓名：</div><el-input class="newlist_input" v-model="teacherName" placeholder="请输入要绑定的教师"></el-input></div>
        <div class="newList">
          <div>选择：</div>
          <el-select v-model="teacherName" placeholder="请选择" class='newlist_input'>
            <el-option v-for="item in option" :key="item.value" :label="item.label" :value="item.label"></el-option>
          </el-select>
        </div>
        <div class="dialog-footer">
          <el-button @click="bindTeacherType = false">取 消</el-button>
          <el-button type="primary" @click="bindTeacher">确 定</el-button>
        </div>
      </el-dialog>
      <el-dialog v-model="changePassword" title='修改密码' width="40%">
        <div class="newList">
          <div>身份证号：</div><el-input class="newlist_input" maxlength="18" type="number" v-model="idCard" placeholder="请输入学生的身份证号"></el-input>
        </div>
        <div class="newList">
          <div>新密码：</div><el-input class="newlist_input" type="number" v-model="passWord" show-password placeholder="请输入修改的密码"></el-input>
        </div>
        <div class="dialog-footer">
          <el-button @click="changePassword = false">取 消</el-button>
          <el-button type="primary" @click="confirmPass">确 定</el-button>
        </div>
      </el-dialog>
    </div>
</template>

<script>
import UploadExcelComponent from '@/components/UploadExcel/index.vue'
  export default {
    components: { UploadExcelComponent },
    data() {
      return {
        teacherName:'', // 教师的默认value
        bindTeacherType: false, // 绑定教师弹窗
        changePassword:false, // 修改密码弹窗
        tableHeader:[
          {
            id:'name',
            name:'姓名'
          },
          {
            id:'class',
            name:'班级'
          },
          {
            id:'idCard',
            name:'身份证号'
          },
          {
            id:'phone',
            name:'联系方式'
          },
        ],
        tableData: [
          {
            'name':'李美丽',
            'class':'三年四班',
            'idCard':220220200202222222,
            'phone':13333333333,
          },
          {
            'name':'李美丽',
            'class':'三年四班',
            'idCard':220220200202222222,
            'phone':13333333333,
          },
          {
            'name':'李美丽',
            'class':'三年四班',
            'idCard':220220200202222222,
            'phone':13333333333,
          },
          {
            'name':'李美丽',
            'class':'三年四班',
            'idCard':220220200202222222,
            'phone':13333333333,
          },
        ],
        classList: [
          {
            label: '2020级',
            id:1,
            children: [
              {
                label: '高一一班',
                id:101,
              },
              {
                label: '高一二班',
                id:102,
              },
              {
                label: '高一三班',
                id:103,
              },
              {
                label: '高一四班',
                id:104,
              },
              {
                label: '高一五班',
                id:105,
              },
              {
                label: '高一六班',
                id:106,
              },
              {
                label: '高一七班',
                id:107,
              },
              {
                label: '高二一班',
                id:108,
              },
              {
                label: '高二二班',
                id:109,
              },
              {
                label: '高二三班',
                id:110,
              },
              {
                label: '高二四班',
                id:111,
              },
              {
                label: '高二五班',
                id:112,
              },
              {
                label: '高二六班',
                id:113,
              },
              {
                label: '高二七班',
                id:114,
              },
              {
                label: '高三一班',
                id:115,
              },
              {
                label: '高三二班',
                id:116,
              },
              {
                label: '高三三班',
                id:117,
              },
              {
                label: '高三四班',
                id:118,
              },
              {
                label: '高三五班',
                id:119,
              },
              {
                label: '高三六班',
                id:120,
              },
              {
                label: '高三七班',
                id:121,
              }
            ]
          }, 
          {
            label: '2021级',
            id:2,
            children: [
              {
                label: '高一一班',
                id:101,
              },
              {
                label: '高一二班',
                id:102,
              },
              {
                label: '高一三班',
                id:103,
              },
              {
                label: '高一四班',
                id:104,
              },
              {
                label: '高一五班',
                id:105,
              },
              {
                label: '高一六班',
                id:106,
              },
              {
                label: '高一七班',
                id:107,
              },
              {
                label: '高二一班',
                id:108,
              },
              {
                label: '高二二班',
                id:109,
              },
              {
                label: '高二三班',
                id:110,
              },
              {
                label: '高二四班',
                id:111,
              },
              {
                label: '高二五班',
                id:112,
              },
              {
                label: '高二六班',
                id:113,
              },
              {
                label: '高二七班',
                id:114,
              },
              {
                label: '高三一班',
                id:115,
              },
              {
                label: '高三二班',
                id:116,
              },
              {
                label: '高三三班',
                id:117,
              },
              {
                label: '高三四班',
                id:118,
              },
              {
                label: '高三五班',
                id:119,
              },
              {
                label: '高三六班',
                id:120,
              },
              {
                label: '高三七班',
                id:121,
              }
            ]
          }, 
          {
            label: '2022级',
            id:3,
            children: [
              {
                label: '高一一班',
                id:101,
              },
              {
                label: '高一二班',
                id:102,
              },
              {
                label: '高一三班',
                id:103,
              },
              {
                label: '高一四班',
                id:104,
              },
              {
                label: '高一五班',
                id:105,
              },
              {
                label: '高一六班',
                id:106,
              },
              {
                label: '高一七班',
                id:107,
              },
              {
                label: '高二一班',
                id:108,
              },
              {
                label: '高二二班',
                id:109,
              },
              {
                label: '高二三班',
                id:110,
              },
              {
                label: '高二四班',
                id:111,
              },
              {
                label: '高二五班',
                id:112,
              },
              {
                label: '高二六班',
                id:113,
              },
              {
                label: '高二七班',
                id:114,
              },
              {
                label: '高三一班',
                id:115,
              },
              {
                label: '高三二班',
                id:116,
              },
              {
                label: '高三三班',
                id:117,
              },
              {
                label: '高三四班',
                id:118,
              },
              {
                label: '高三五班',
                id:119,
              },
              {
                label: '高三六班',
                id:120,
              },
              {
                label: '高三七班',
                id:121,
              }
            ]
          }, 
          
        ],
        defaultProps: {
          children: 'children',
          label: 'label',
        },
        currentPage4:200,
        downloadLoading: false,
        option:[
          {
            value: 1,
            label: '李优秀'
          }, 
          {
            value: 2,
            label: '李美丽'
          }, 
          {
            value: 3,
            label: '李聪明'
          }, 
        ],
        idCard:'',
        passWord:''
      }
    },
    methods: {
      // 修改密码
      changePass(item,index){
        this.changePassword = true
      },
      confirmPass(){
        let _IDRe18 = /^([1-6][1-9]|50)\d{4}(18|19|20)\d{2}((0[1-9])|10|11|12)(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/
        let myreg = /^(((13[0-9]{1})|(15[0-9]{1})|(18[0-9]{1})|(17[0-9]{1}))+\d{8})$/;
        if(this.idCard == '' || this.idCard.length < 18 || !_IDRe18.test(this.idCard)){
          this.$message.error('请检查您填写的身份证号是否有误！');
        }else if(this.passWord == '' || this.passWord.length < 6){
          this.$message.error('请检查您填写的密码长度是否有误！');
        }else{
          this.changePassword = false
        }
      },
      // 绑定教师
      addTeacher(){
        this.bindTeacherType = true
      },
      bindTeacher(){
        if(this.teacherName == ''){
         this.$message.error('请选择绑定该班级的教师！');
        }else{
          this.bindTeacherType = false
        }
      },
      // 查看报告
      exportTest(item,index){
        this.$router.push({name:'result',query: {id:'1'}})
      },
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      },
      // 筛选年级条件
      handleNodeClick(data){
        console.log(data)
      },
      // 查看报告
      handleImport(data){
        // console.log(data)
      },
      // 添加学生
      addStudent(){

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
        // this.tableData = results
        this.tableHeader['name'] = header
      },
      handleDownload() {
        this.downloadLoading = true
        import('@/vendor/Export2Excel').then(excel => {
          const tHeader = ['name', 'class', 'idCard', 'phone']
          const filterVal = ['name', 'class', 'idCard', 'phone']
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
.box{
    padding:15px;
    display:flex;
    height: 90vh;
}
.left{
  width:15%;
  border:1px solid #dfe6ec;
  overflow-y: auto;
}
.right{
  width:84%;
  margin-left:1%;
  overflow-y: auto;
}
.el-table__body,.el-table__header{
  width:100% !important;
}
.page{
  margin-top:20px;
}
.search{
  display:flex;
  margin: 15px 15px 0px 15px;
}
.input{
  width:200px;
}
/deep/ .el-tree-node__content{
  height: 40px;
}
</style>