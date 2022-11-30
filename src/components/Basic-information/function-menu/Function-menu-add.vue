<template>
  <div class="bullet-frame">
    <div class="menu-addbox">
      <div class="title">
       <h3> {{ shows ? "编辑" : "添加" }}</h3>
        <i class="el-icon-circle-close iconcal" @click="menuadd"></i>
      </div>
      <el-form
        :model="ruleForm"
        :rules="rules"
        ref="ruleForm"
        label-width="150px"
        class="demo-ruleForm"
      >
        <el-form-item label="菜单名称" prop="menu_name">
          <el-input v-model="ruleForm.menu_name"></el-input>
        </el-form-item>
        <el-form-item label="菜单URL" prop="url">
          <el-input v-model="ruleForm.url"></el-input>
        </el-form-item>
         <el-form-item label="重定向">
          <el-input v-model="ruleForm.redirect"></el-input>
        </el-form-item>
        <el-form-item label="路由name" prop="route_url">
          <el-input v-model="ruleForm.route_url"></el-input>
        </el-form-item>
        <el-form-item label="路由组件路径" prop="route_path">
          <el-input v-model="ruleForm.route_path"></el-input>
        </el-form-item>
        <el-form-item label="上级菜单" prop="cascader">
          <el-cascader
            :options="routeListOpstion"
            v-model="ruleForm.cascader"
            :clearable="true"
            :props="Props"
            class="cascader-box"
            @change="changecal"
          ></el-cascader>
        </el-form-item>
        <el-form-item label="显示顺序" prop="menu_order">
          <el-input type="number" v-model="ruleForm.menu_order"></el-input>
        </el-form-item>
        <el-form-item label="菜单图标" prop="menu_ico">
          <el-input v-model="ruleForm.menu_ico"></el-input>
        </el-form-item>
        <el-form-item label="是否加入侧边栏" prop="broadside_status">
          <el-radio-group v-model="ruleForm.broadside_status">
            <el-radio label="1"> 否 </el-radio>
            <el-radio label="2"> 是 </el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="是否缓存组件" prop="keepAlive">
          <el-radio-group v-model="ruleForm.keepAlive">
            <el-radio label="1"> 否 </el-radio>
            <el-radio label="2"> 是 </el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item class="btn-box">
          <el-button type="primary" @click="submitForm('ruleForm')"
            >保存</el-button
          >
          <el-button @click="menuadd">取消</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import { mapState } from "vuex";
import { postAddrouterpage,postUpdateRouterListPage } from "../../../api/user";
export default {
  props: ["menuadd", "rowlist"],
  data() {
    return {
      shows: false,
      Props: {
        value: "member_menu_id",
        label: "title",
        checkStrictly: true,
      },
      ruleForm: {
        menu_name: "",
        url: "",
        redirect: "",
        route_url: "",
        route_path: "",
        cascader:[0],
        pid:"0",
        menu_order: "",
        menu_ico: '',
        keepAlive:'0',
      },
      rules: {
        menu_name: [{ required: true, message: "请输入菜单名称", trigger: "blur" }],
        url: [{ required: true, message: "请输入菜单URL", trigger: "blur" }],
        route_url: [{ required: true, message: "请输入路由name", trigger: "blur" }],
        route_path: [
          { required: true, message: "请输入路由组件路径", trigger: "blur" },
        ],
        cascader:[{ required: true, message: "请输入菜单名称", trigger: "blur" }],
        menu_order:[{ required: true, message: "请输设置显示顺序", trigger: "blur" }],
        broadside_status: [
          {
            required: true,
            message: "请选择是否加入侧边栏",
            trigger: "change",
          },
        ],
      },
    };
  },
  computed: {
    ...mapState({
      routeListOpstion:state=>state.user.routeListOpstion,
    }),
  },
  mounted() {
    // 修改
    this.shows = Object.keys(this.rowlist).length == 0 ? false : true;
    if (this.shows) {
      this.ruleForm = this.rowlist;
    }
  },
  methods: {
    changecal(e){
      console.log(e)
    },
    // 添加接口
    async postAddrouterpages(row) {
      let data = await postAddrouterpage(row);
      if (data.status == 200) {
        this.$message.success({
          message: data.message,
          type: "success",
        });
        this.menuadd("addshou");
      } else {
        this.$message.error(data.message);
      }
    },
    // 修改
    async postUpdateRouterListPages(row){
     let data = await postUpdateRouterListPage(row)
      if (data.status == 200) {
        this.$message.success({
          message: data.message,
          type: "success",
        });
        this.menuadd("addshou");
      } else {
        this.$message.error(data.message);
      }
    },
    // 保存
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          console.log(this.ruleForm.cascader)
          if (this.ruleForm.cascader.length) {
            let ids = this.ruleForm.cascader.length - 1;
            this.ruleForm.pid =JSON.parse(JSON.stringify(this.ruleForm.pid[ids]));
          }
          if (!this.shows) {
            this.postAddrouterpages(this.ruleForm);
          } else {
            this.postUpdateRouterListPages(this.ruleForm)
          }
        } else {
          return false;
        }
      });
    },
  },
};
</script>
<style lang="less" scoped>
.menu-addbox {
  width: 55vw;
  margin: auto;
  background: #fff;
  box-sizing: border-box;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border-radius: 5px;
}
.title{
  padding: 10px 30px;
  display: flex;
  justify-content:space-between;
   color: #606266;
}
.btn-box {
  text-align: center;
}
.demo-ruleForm{
  height: 70vh;
  overflow: hidden;
  overflow-y: scroll;
  padding: 20px 30px;
}
.iconcal{
  font-size: 23px;
  color: #C0C4CC;
}
.iconcal:hover{
  color: #F56C6C;
}
</style>