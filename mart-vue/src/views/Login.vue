<!-- 登陆文件 -->
<template>
  <div class="login_section">
    <HeadBarLight2></HeadBarLight2>

    <div class="login_content">
      <p>
        登录
      </p>
      <el-input placeholder="请输入用户名" v-model="account"></el-input>
      <el-input placeholder="请输入密码" v-model="pwd" type="password"></el-input>
      <div style="padding-top: 5px">
        <el-checkbox style="margin-left: 5px; float: left">记住我</el-checkbox>
        <div class="no_account">
          <router-link to='/register'> 找回密码</router-link>
        </div>
      </div>
      <el-button class="login_button" type="primary" :loading=this.button_disabled @click="doLogin">提交</el-button>
      <div class="no_account">
        <router-link to='/register'> 没有账号</router-link>
      </div>
    </div>
  </div>
</template>

<script>
  import HeadBarLight2 from "../components/head/HeadBarLight2.vue";
  import server from '../../config/index';
  import axios from '../axios/http';
  import {login} from '../axios/api'

  export default {
    name: "Login",
    components: {HeadBarLight2},
    data() {
      return {
        account: '',
        pwd: '',
        error: '',
        url: server.url + '/api/session',
        button_disabled: false,
      }
    },
    methods: {
      doLogin: function () {
        this.button_disabled = true;
        var filterRule= /[^0-9a-zA-Z_]/g; 
        if (this.account == "") {
          // this.$message({
          //   message: '请输入用户名',
          //   type: 'warning'
          // });
           alert('请输入用户名');
        }
        else if (this.pwd == "") {
          alert('请输入密码');
        }
        else if(this.account.length < 3){
          swal("Warning", "用户名少于三个字符！", "warning");
        }
        else if(this.account.length > 15){
          swal("Warning", "用户名多于十五个字符！", "warning");
        }
        else if(filterRule.test(this.account) == false){
          swal("Warning", "用户名包含非法字符！", "warning");
        }
        else {
          var self = this;
          login(this.url, {"username": this.account, "password": this.pwd}, this);
        }
        this.button_disabled = false;
      }
    }
  };
</script>

<style>
  .header {
    position: fixed;
    top: 0px;
    width: 100%;
    color: #323a45;
  }

  .login_content {
    text-align: center;
    margin-top: 200px;
    width: 20%;
    margin-left: auto;
    margin-right: auto;
  }

  .login_content > .el-input {
    margin: 5px;
  }

  .login_content > .el-button {
    margin: 8px 5px;
  }

  .login_button {
    width: 100%;
  }

  .no_account {
    float: right;
    cursor: pointer;
    font-size: 12px;
  }

  .no_account > a {
    color: #323a45;
  }
</style>

