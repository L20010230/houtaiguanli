<template>
    <div>
        <div class="question">
          <el-tag>问</el-tag>
          <div class="title">{{newlist.title}}</div>
        </div>
        <!-- 职业期待 -->
        <div class="await" style="padding:15px" v-show="testType == 1">
          <div class="components-container board">
            <Kanban :list="list1" :group="group1" class="kanbanList" headerText='很不期待'/>
            <Kanban :list="list2" :group="group2" class="kanbanList" headerText='不太期待'/>
            <Kanban :list="list3" :group="group3" class="kanbanList" headerText='有点期待'/>
            <Kanban :list="list4" :group="group4" class="kanbanList" headerText='完全期待' style="margin-right:0px"/>
          </div>
            <Kanban :list="list5" :group="group5" class="kanbanSelect" headerText='所有职业'/>
        </div>
        <!-- 调查问卷 自评-->
        <div class="tableList" v-show="testType == 2">
          <div v-for="(item,index) in tableList" :key="index" class="tableList_item">
            <div class="item_left">{{index + 1}}.{{item.title}}</div>
            <div class="item_right" >
              <el-radio-group v-model="item.num" style="display:flex;width:100%" @change='clickAnswer(item)' >
                <el-radio :label="item1.num" :key="index1" v-for="(item1,index1) in tableType"></el-radio>
              </el-radio-group>
            </div>
          </div>
        </div> 
        <!-- 穿梭框题型 -->
        <div class="answer" v-show="testType == 3">
          <el-transfer style="text-align: left; width:100%;display:flex" v-model="value" filterable :titles="['所有同学', '已选同学']"
              :button-texts="['取消', '确认']"
              ref="myTransfer"
              :format="{
              noChecked: '${total}',
              hasChecked: '${checked}/${total}',
              }" @change="handleChange" :data="newlist.data">
              <!-- <span slot-scope="{ option }">{{ option.label }}</span> -->
          </el-transfer>
        </div>
        <div class="nextQues">
          <el-button type="primary" plain v-if="allSubject > quesId" @click="nextQusetion">下一题</el-button>
          <el-button type="primary" v-if="allSubject == quesId" @click="nextQusetion">提交</el-button>
        </div>
    </div>
</template>
<script>
import Kanban from '@/components/Kanban'
export default {
    components: { Kanban },
    data(){
        return{
            testType:1,  // 
            group1:{
                name: "itxst",
                pull: true, 
                put: () => {
                  if (this.list1.length == 3) {
                    return false
                  }
                },  
            },
            group2:{
                name: "itxst",
                pull: true, 
                put: () => {
                if (this.list2.length == 3) {
                    return false
                }
                },  
            },
            group3:{
                name: "itxst",
                pull: true, 
                put: () => {
                if (this.list3.length == 3) {
                    return false
                }
                },  
            },
            group4:{
                name: "itxst",
                pull: true, 
                put: () => {
                if (this.list4.length == 3) {
                    return false
                }
                },  
            },
            group5:{
                name: "itxst",
                pull: true, 
                put: true,
            },
            list1: [],
            list2: [],
            list3: [],
            list4: [],
            list5: [
                { name: '科学实验工作者、质量检测人员、食品/制药/化工/地质行业等',id:1},
                { name: '工程师、工艺设计、产品设计、技术员、软件开发员等',id:2},
                { name: '环保公司、污水处理、养殖业、兽医、林业、水产行业等',id:3},
                { name: '医生、护理、体检、药厂、药监部门等',id:4},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:5},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:6},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:7},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:8},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:9},
                { name: '翻译、外交、记者、出版、编辑、主持人、文秘、教师等',id:10}
            ],
            tableList:[
              {
              id:1,
              title:'在历史学科上，我取得了好成绩。',
              },
              {
              id:2,
              title:'我对自己在历史学科上的学习潜力有信心。',
              },
              {
              id:3,
              title:'我对自己在历史学科上的学习潜力有信心。',
              },
              {
              id:4,
              title:'我对自己在历史学科上的学习潜力有信心。',
              },
              {
              id:5,
              title:'我对自己在历史学科上的学习潜力有信心。',
              },
              {
              id:6,
              title:'我对自己在历史学科上的学习潜力有信心。',
              },
            ],
            tableType:[
                {
                    id:101,
                    num:'完全不符'
                },
                {
                    id:102,
                    num:'基本不符'
                },
                {
                    id:103,
                    num:'不确定'
                },
                {
                    id:104,
                    num:'基本符合'
                },
                {
                    id:105,
                    num:'完全符合'
                },
            ],
            rightArr:[],
            value:[],
            leftArr1:[
                {
                id:1,
                title:'这是第一个问题',
                },
                {
                id:2,
                title:'这是第二个问题',
                },
                {
                id:3,
                title:'这是第三个问题',
                },
            ],
            students:[
                {
                label:'李一号',
                key:1,
                },
                {
                label:'李二号',
                key:2,
                },
                {
                label:'李三号',
                key:3,
                }
            ],
            students1:[
                {
                label:'李一号',
                key:1,
                },
                {
                label:'李二号',
                key:2,
                },
                {
                label:'李三号',
                key:3,
                }
            ],
            quesId:0,
            allSubject:5,
            list:[
                {
                id:1,
                title:'你觉得班级里那个同学最顽皮?',
                },
                {
                id:2,
                title:'你觉得班级里那个同学最可爱?',
                },
                {
                id:3,
                title:'你觉得班级里那个同学最活泼?',
                },
            ],
            newlist:[],
        }
    },
    mounted(){
        this.newlist = this.list[this.quesId]
        this.newlist['data'] = this.students
        this.leftArr1[this.quesId]['data'] = this.students1
    },
    methods:{
      // 自评 调查问卷
      clickAnswer(e) {

      },
      // 选中穿梭框
      handleChange(value,direction, movedKeys) {
        if(direction == 'right'){
            for (let i = 0 ; i < movedKeys.length ; i++) {
            this.leftArr1[this.quesId].data.forEach((item,index) => {
                if(item.key === movedKeys[i]){
                  this.rightArr.push(this.leftArr1[this.quesId].data[index])
                  this.leftArr1[this.quesId].data.splice(index,1);
                }
            }); 
            }
        }else{
            for (let i = 0 ; i < movedKeys.length; i++) {
            this.rightArr.forEach((item,index) => {
                if(item.key === movedKeys[i]){
                  this.leftArr1[this.quesId].data.push(this.rightArr[index])
                  this.rightArr.splice(index,1);
                }
            }); 
            }
          }
      },
      // submit
      nextQusetion(){
        // if(this.tableList[0].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else if(this.tableList[1].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else if(this.tableList[2].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else if(this.tableList[3].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else if(this.tableList[4].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else if(this.tableList[5].num == undefined){
        //   this.$message.error('请检查是否选择~');
        // }else{
        //   console.log(this.tableList)
        // }
        console.log(this.rightArr)
      }
    }
}
</script>
<style scoped>
.question {
  font-size: 18px;
  display: flex;
  padding: 15px 15px 0px 15px;
  line-height: 28px;
}
.title {
  margin-left: 10px;
}
/* 职业期待 */
.kanbanList{
  width: 23.8%;
  margin-right: 1.3%;
}
/deep/.kanbanSelect .board-column-content{
  display: flex !important;
  flex-wrap: wrap;
  min-height: 40px;
}
/deep/.kanbanList .board-column-content{
  min-height: 200px;
}
.components-container{
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 15px;
}
/* 列表题型 */
.tableList{
    margin: 15px;
    margin-top: 15px;
    border-left: 1px solid #e6ebf5;
    border-top: 1px solid #e6ebf5;
    border-right: 1px solid #e6ebf5;
}
.tableList_item{
  display: flex;
  height: 40px;
  line-height: 40px;
  font-size: 14px;
  border-bottom: 1px solid #e6ebf5;
}
.tableList .item_left{
  width: 50%;
  padding-left:15px;
  border-right: 1px solid #e6ebf5;
}
.tableList .item_right{
  width: 50%;
  display: flex;
  padding-right:15px;
}
.tableList .item_right>div{
  text-align: center;
}
.el-radio{
  line-height: 40px;
  width: 20%;
  margin: 0px;
}
/* 穿梭框 */
.answer{
    margin: 15px;
}
/deep/.el-transfer-panel{
  width: 40%;
  height: 80vh;
}
/deep/.el-transfer-panel__body{
  padding: 10px;
}
/deep/.el-transfer-panel__filter{
  margin: 0px;
}
/deep/.el-transfer__buttons{
  width: 20%;
  padding: 200px 2.3%;
  text-align: center;
}
/deep/.el-transfer-panel__list.is-filterable{
  height: 95%;
}
/deep/.el-transfer-panel__body{
  height: 93%;
}
.nextQues{
  position: fixed;
  width: 30%;
  left: 40%;
  bottom: 30px;
}
.nextQues button{
  display: block;
  margin: auto;
  width: 100%;
}
</style>