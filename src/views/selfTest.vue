<template>
<!-- 自我测评 -->
  <div>
    <div v-if="show == 0" style="padding:15px">
      <div class="test_name">问卷名称</div>
      <div class="test_point">这是一段关于问卷的内容介绍</div>
       <el-button type="primary" class="test_button" plain @click='beginTest'>开始测评</el-button>
    </div>
    <div class="empty" v-if="show == 1">
      <!-- <img class="img" src="../public/success.png"/> -->
      <div>您已经成功填写过此次报告，无需重复提交！</div>
    </div>
    <div class="container">
      <div>
        <div class="question">
          <el-tag>问</el-tag>
          <div class="title">{{newlist.title}}</div>
        </div>
        <!-- 穿梭框题型 -->
        <div class="answer" v-if="show == 2">
          <div style="text-align: center">
            <el-transfer style="text-align: left; width:100%;display:flex" v-model="value" filterable :titles="['所有同学', '已选同学']"
              :button-texts="['取消', '确认']"
              ref="myTransfer"
              :format="{
                noChecked: '${total}',
                hasChecked: '${checked}/${total}',
              }" @change="handleChange" :data="newlist.data">
              <span slot-scope="{ option }">{{ option.label }}</span>
            </el-transfer>
          </div>
        </div>
        <!-- 文字题型 -->
        <div class="option" v-if="show == 3">
          <div v-for="(item,index) in fontList" :key='index' @click="checkText(index)" :class="checkOption == index ? 'checkOptioned' : 'checkOption' ">{{item.title}}</div>
        </div>
        <div class="nextQues">
          <el-button type="primary" plain v-if="allSubject > quesId" @click="nextQusetion">下一题</el-button>
          <el-button type="primary" v-if="allSubject == quesId" @click="nextQusetion">提交</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      show:2,
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
          checked:true,
          top:false
        },
        {
          label:'李二号',
          key:2,
          checked:false,
          top:false
        },
        {
          label:'李三号',
          key:3,
          checked:false,
          top:false
        }
      ],
      students1:[
        {
          label:'李一号',
          key:1,
          checked:true,
          top:false
        },
        {
          label:'李二号',
          key:2,
          checked:false,
          top:false
        },
        {
          label:'李三号',
          key:3,
          checked:false,
          top:false
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
      answerList:[],
      fontList:[
        {
          id:1,
          title:'A选项'
        },
        {
          id:2,
          title:'b选项'
        },
        {
          id:3,
          title:'c选项'
        },
        {
          id:4,
          title:'d选项'
        },
      ],
      checkOption:99,
    };
  },
  mounted(){
    this.newlist = this.list[this.quesId]
    this.newlist['data'] = this.students
    this.leftArr1[this.quesId]['data'] = this.students1
  },
  methods: {
    // 切换测评状态 0 测评前 1 已测评 2 开始测评
    beginTest(){
      this.show = 1
    },
    checkText(idx){
      this.checkOption = idx
    },
    // 选中穿梭框
    handleChange(value,direction, movedKeys) {
      if(direction == 'right'){
        for (let i = 0 ; i < movedKeys.length ; i++) {
          this.leftArr1[this.quesId].data.forEach((item,index) => {
             if(item.key === movedKeys[i]){
               console.log(item.key,movedKeys[i])
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
    // 下一题 提交
    nextQusetion(){
      if(this.rightArr == ''){
        this.$message('请对问题做出选择后在确认！');
      }else if(this.rightArr != ''){
        this.quesId = this.quesId + 1
        this.answerList.push({
          id:this.quesId,
          data:this.rightArr
        })
        this.value = []
        this.rightArr = []
        if(this.list.length > this.quesId){
          this.newlist = this.list[this.quesId]
          this.newlist['data'] = this.students
          this.leftArr1[this.quesId]['data'] = this.students1
        }
      }
    },
  },
};
</script>
<style scoped>


.test_name{
  font-size: 20px;
  font-weight: 500;
}
.test_point{
  font-size: 14px;
  margin-top: 30px;
}
.test_button{
  position: fixed;
  bottom: 40px;
  width: 40%;
  left: 30%;
}
.empty{
  height: 100vh;
  margin: 0 auto;
}
.empty>.img{
  display: block;
  padding: 140px 0px 60px 0px;
  margin: auto;
  width: 300px;
}
.empty>div{
  font-size: 20px;
  text-align: center;
}
.option{
  margin-top: 30px;
}
.option>div{
  height: 40px;
  text-align: center;
  border-radius: 4px;
  margin-bottom: 15px;
  line-height: 40px;
}
.checkOption{
  background-color: #ffffff;
  border: 1px solid #e6ebf5;
}
.checkOptioned{
  background-color: #1890ff;
  border: 1px solid #8cc8ff;
  color: white;
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
.container {
  margin: 15px;
}
.question {
  font-size: 18px;
  display: flex;
  line-height: 28px;
}
.title {
  margin-left: 10px;
}
.answer {
  padding: 15px 0px;
}
.button {
  width: 100%;
}
.checkButton {
  display: inline-block;
  width: 100%;
  border-radius: 4px;
  height: 35px;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  -webkit-appearance: none;
  text-align: center;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  outline: none;
  margin: 0;
}
.checkButtonBe {
  background: #ffffff;
  border: 1px solid #dcdfe6;
  color: #606266;
}
.checkButtonEd {
  border: 1px solid #d1e9ff;
  background: #e8f4ff;
  color: #1890ff;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
/* 穿梭框 */
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
</style>
