
<template>
<div class="login-container">
<el-form :model="loginForm" 
  ref="loginForm" 
  label-width="100px" 
  class="demo-ruleForm login-page">

  <el-form-item label="用户名" prop="username">
    <el-input v-model="loginForm.username" auto-complete="off" ></el-input>
  </el-form-item>    
  
  <el-form-item label="密码" prop="password">
    <el-input type="password" v-model="loginForm.password" auto-complete="off" ></el-input>
  </el-form-item>
  
  <el-form-item>
    <el-button type="primary" @click="submitForm('loginForm')">登录</el-button>
    <el-button @click="resetForm('loginForm')">注册</el-button>
  </el-form-item>
</el-form>
</div>
</template>
<script>
import axios from 'axios'
  export default {
    name: 'Login',
    data() {
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        }
      };

      return {
        loginForm: {
          username: '',
          password: ''
        }
      };
    },
    methods: {

      async submitForm (formName) {

      try {
        // 表单验证
        await this.$refs[formName].validate()

        //发送登录验证请求
        const res = await axios.post(
          'user/login/',
          {
            'userName': this.loginForm.username,
            'passWord': this.loginForm.password
          })

        //控制跳转
        if(res.data[0] == null){
          this.$message.error('用户名或密码错误');
        }else{
          console.log(res)
          this.$router.push({path: '/register'})
        }
        
      } catch (err) {
        console.log(err)
      }
    },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>
<style scoped>
.login-container {
    width: 100%;
    height: 100%;
}
.login-page {
    -webkit-border-radius: 5px;
    border-radius: 5px;
    margin: 180px auto;
    width: 350px;
    padding: 35px 35px 15px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
}
</style>