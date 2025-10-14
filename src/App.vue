<template>
  <div id="app">
    <el-dialog  title="飞书项目 数据同步配置"
                :visible.sync="dialogVisible"
                width="60%">
      <div v-if="one">
        <div class="check">
          <div class="left">
            <span>选择授权账号</span>
          </div>
          <div class="right">
            <el-button type="primary" plain icon="el-icon-plus" @click="addphone">添加授权账号</el-button>
          </div>
        </div>
        <div class="box" v-if="userDisposition.length < 1">
          <el-empty description="暂未授权任何账号"></el-empty>
        </div>
        <div class="box" v-if="userDisposition.length > 0 ">
          <el-table height="200"  ref="singleTable" :data="userDisposition" highlight-current-row @current-change="handleCurrentChange"
           style="width: 100%">
         <el-table-column type="index" width="120" label="序号"></el-table-column>
        <el-table-column property="userName" label="名称" width="120"></el-table-column>
        <el-table-column fixed="right" label="操作" width="100">
        <template slot-scope="scope">
        <!--<el-button @click="updateClick(scope.row)" type="text" size="small">修改</el-button>-->
        <el-button @click="deleteClick(scope.row)" type="text" size="small">删除</el-button>
        </template>
        </el-table-column>
        </el-table>
        </div>
      </div>
      <div v-if="two" style="height: 180px">
        <el-form>
          <el-form-item label="名称">

            <el-input placeholder="请输入名称" v-model="userInfo.name"></el-input>
          </el-form-item>
          <div style="text-align: left">
            <span style="font-weight: bold;">User Key</span>
          </div>
          <div style="margin-top: 12px">
            <el-input placeholder="请输入User Key" v-model="userInfo.userKey"></el-input>
          </div>
        </el-form>
      </div>
      <div v-if="three" style="height: 200px">
        <el-form label-position="left" label-width="100px">
          <el-form-item label="类型" required style="display: flex; flex-wrap: nowrap;">
            <el-radio v-model="radio" label="1">工作项列表</el-radio>
            <!--<el-radio v-model="radio" label="2">插件工时</el-radio>-->
            <el-radio v-model="radio" label="3">任务工时</el-radio>
          </el-form-item>
          <el-form-item label="项目空间" required>
            <el-select  v-model="projectName" 
                       placeholder="请选择飞书项目空间" 
                       style="width: 180px;"
                       @change="handleParentChange">
              <el-option
                v-for="item in projectSpace"
                :key="item.projectKey"
                :label="item.name"
                :value="item.projectKey">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="工作项  " required>
            <el-select v-model="projectInfo" placeholder="请选择工作项" style="width: 180px;">
              <el-option
                v-for="item in childOptions"
                :key="item.type_key"
                :label="item.name"
                :value="item.type_key">
              </el-option>
            </el-select>
          </el-form-item>
          <!--<el-form-item label="按视图筛选" required>
            <el-select  placeholder="当前版本不可用" disabled style="width: 320px;"></el-select>
          </el-form-item>-->
        </el-form>
      </div>
      <span slot="footer">
        <el-button  v-if="two" @click="cancel">取消</el-button>
        <el-button  v-if="three" @click="up">上一步</el-button>
        <el-button type="primary" :disabled="!two" @click="checkUserInfo(2)" v-if="two">下一步</el-button>
        <el-button type="primary" :disabled="!two" @click="synchronousData()" v-if="three">下一步</el-button>
        <el-button type="primary" :disabled="!one" @click="synchronous()" v-if="aaa">下一步</el-button>
        <!--<div style="font-size: 13px;margin-top: 10px;text-align: center">
          <span>免费版本仅支持部分字段及最多1000行数据的同步，<span style="color: #5487fd">升级版本</span>获取全部功能</span>
        </div>-->
      </span>

    </el-dialog>
  </div>
</template>

<script>

import axios from 'axios';
import { bitable } from '@lark-base-open/connector-api';

export default {
  name: 'App',
  data(){
    return {
      dialogVisible:true,
      one:true,
      two:false,
      three:false,
      aaa:false,
      bbb:true,
      radio:'1',
      userInfo:{
        name:'',
        userKey:''
      },
      projectSpace:[],
      projectName:'',
      childOptions:[],
      projectInfo:'',
      config:'',
      userId:'',
      tenantKey:'',
      configData: {
        userKey: '',
        projectName: '',
        projectInfo: '',
        tenantKey:'',
        userId:''
      },
      userDisposition:[],
      currentRow:'',
    }
  },
  created() {
    // 在实例创建完成后立即调用
    this.getUser()
  },
  methods:{
    synchronous(){
      this.userInfo.name = this.currentRow.userName
      this.userInfo.userKey = this.currentRow.userKey
      this.checkUserInfo(1);
      this.one = false
      this.aaa = false
    },
    handleCurrentChange(val) {
        this.currentRow = val;
        this.aaa = true
        this.two = false
      },
    addphone(){
      this.one = false
      this.two = true
    },
    next(){
      this.two = false
      this.three = true
    },
    async synchronousData(){
      if(this.projectName=='' || this.projectInfo==''){
        this.$message({
          message: '参数不可为空',
          type: 'warning'
        });
        return;
      }
      console.log("100000");
      this.configData.userKey = this.userInfo.userKey;
      this.configData.projectName = this.projectName;
      this.configData.projectInfo = this.projectInfo;
      this.configData.type = this.radio;
     
      //告诉后台进行数据同步拉取
      //用户id
      console.log("000001");
      //console.log(bitable);
      //const config = await bitable.getConfig();
      //console.log('获取配置成功:', config);
      //const userId = await bitable.getBaseUserId();
      console.log(11111)
      //console.log(22222+userId)
      console.log(33333)
      //this.userId = userId;
      //租户id
      //const tenantKey = await bitable.getTenantKey();
      //console.log(22222+tenantKey)
      //this.tenantKey = tenantKey;
      ///////
      this.configData.userId = this.userId;
      this.configData.tenantKey = this.tenantKey;
      try {
        const response = await axios.post('https://field.hykjit.com/prod-api/feishu/synchronousData', {
          // 请求体数据
          userKey:this.userInfo.userKey,
          projectId:this.projectName,
          workItem:this.projectInfo,
          userId:this.userId,
          tenantKey:this.tenantKey,
          type:this.radio
        }, {
          // 请求配置
          headers: {
            'Content-Type': 'application/json'
          }
        });
        //console.log('响应数据:', response.data);
        if(response.data.code != 200){
          this.$message({
          message: '用户不可用',
          type: 'warning'
        });
        return;
        }
        await bitable.saveConfigAndGoNext(this.configData);
        return;
      } catch (error) {
        console.error('请求失败:', error.response ? error.response.data : error.message);
        throw error;
      }
    },
    async checkUserInfo(type1){
      if(this.userInfo.name=='' || this.userInfo.userKey==''){
        this.$message({
          message: '参数不可为空',
          type: 'warning'
        });
        return;
      }
      var list = [];
      list.push(this.userInfo.userKey)
      var typea = type1;
      console.log(type1)
      //发送get请求校验参数是否正确
      try {
        const response = await axios.post('https://field.hykjit.com/prod-api/feishu/getUserInfo', {
          // 请求体数据
          userkey:list,
          type:typea,
          userName:this.userInfo.name,
          tenantId:this.tenantKey,
          userId:this.userId
        }, {
          // 请求配置
          headers: {
            'Content-Type': 'application/json'
          }
        });
        //console.log('响应数据:', response.data);
        if(response.data.code != 200){
          this.$message({
          message: '用户不可用',
          type: 'warning'
        });
        return;
        }
        this.projectSpace = response.data.data;
        this.two = false
        this.three = true
        return;
      } catch (error) {
        console.error('请求失败:', error.response ? error.response.data : error.message);
        throw error;
      }
    },
    // 父级选择变化时触发
    async handleParentChange(projectName) {
      this.childValue = ''; // 清空子级选择
      if (projectName) {
        await this.fetchChildOptions(projectName);
      } else {
        this.childOptions = [];
      }
    },
    // 根据父级ID获取子级选项
    async fetchChildOptions(projectName) {
      try {
        const response = await axios.post('https://field.hykjit.com/prod-api/feishu/getWorkItem', {
          // 请求体数据
          projectId:projectName,
          userKey1:this.userInfo.userKey
        }, {
          // 请求配置
          headers: {
            'Content-Type': 'application/json'
          }
        });
        //console.log('响应数据:', response.data);
        if(response.data.code != 200){
          this.$message({
          message: '数据不存在',
          type: 'warning'
        });
        return;
        }
        this.childOptions = response.data.data
        return;
      } catch (error) {
        console.error('请求失败:', error.response ? error.response.data : error.message);
        throw error;
      }
    },
    up(){
      this.one = false
      this.two = true
      this.three = false
    },
    cancel(){
      this.one = true
      this.two = false
      this.three = false
    },
    //修改信息
    async updateClick(){

    },
    //删除信息
    async deleteClick(row){
      try {
        const response = await axios.post('https://field.hykjit.com/prod-api/feishu/deleteUserInfo', {
          // 请求体数据
          tenantId:row.tenantId,
          id:row.id
        }, {
          // 请求配置
          headers: {
            'Content-Type': 'application/json'
          }
        });
        //console.log('响应数据:', response.data);
        if(response.data.code != 200){
          this.$message({
          message: '操作有误',
          type: 'warning'
        });
        return;
        }
        console.log(12123132123)
        this.getUser();
        return;
      } catch (error) {
        console.error('请求失败:', error.response ? error.response.data : error.message);
        throw error;
      }
    },
    //获取用户配置信息
    async getUser() {
      const config = await bitable.getConfig();
      console.log('获取配置成功:', config);
      //用户ID
      const userId = await bitable.getUserId();
      this.userId = userId;
      //租户id
      const tenantKey = await bitable.getTenantKey();
      this.tenantKey = tenantKey;
      try {
        const response = await axios.post('https://field.hykjit.com/prod-api/feishu/getUser', {
          // 请求体数据
          tenantId:this.tenantKey,
          userId:this.userId
        }, {
          // 请求配置
          headers: {
            'Content-Type': 'application/json'
          }
        });
        //console.log('响应数据:', response.data);
        if(response.data.code != 200){
          this.$message({
          message: '数据不存在',
          type: 'warning'
        });
        return;
        }
        this.userDisposition = response.data.data
        console.log(this.userDisposition)
        return;
      } catch (error) {
        console.error('请求失败:', error.response ? error.response.data : error.message);
        throw error;
      }
    }
  }
}
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
::v-deep .el-dialog__header{
  text-align: left;
}
.check{

  display: flex;
  justify-content:space-between;
  align-items: center;
}
.box{
  height: 200px;
}
::v-deep .el-input__inner{
  background-color: #f2f3f5;
  color: #99a1ac;
}
::v-deep .el-form-item__label{
  font-weight: bold;
  font-size: 14px;
}
</style>
