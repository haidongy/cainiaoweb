
<template>
<div class="login-container">
<el-form :model="loginForm" 
  status-icon :rules="rules2" 
  ref="loginForm" 
  label-width="100px" 
  class="demo-ruleForm login-page">
  <el-form-item label="用户名" prop="username">
    <el-input v-model="loginForm.username" auto-complete="off" ></el-input>
  </el-form-item>  
  <el-form-item label="性别">
    <el-select v-model="loginForm.sex" placeholder="请选择性别">
      <el-option label="男" value="男"></el-option>
      <el-option label="女" value="女"></el-option>
    </el-select>
  </el-form-item>
  <el-form-item
    prop="email"
    label="邮箱"
  >
    <el-input v-model="loginForm.email"></el-input>
  </el-form-item>
  <el-form-item label="密码" prop="pass">
    <el-input type="password" v-model="loginForm.pass" auto-complete="off" ></el-input>
  </el-form-item>
  <el-form-item label="确认密码" prop="checkPass">
    <el-input type="password" v-model="loginForm.checkPass" auto-complete="off" ></el-input>
  </el-form-item>
  <el-form-item label="年龄" prop="age">
    <el-input v-model.number="loginForm.age" ></el-input>
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
      var checkAge = (rule, value, callback) => {
        if (!value) {
          return callback(new Error('年龄不能为空'));
        }
        setTimeout(() => {
          if (!Number.isInteger(value)) {
            callback(new Error('请输入数字值'));
          } else {
            if (value < 18) {
              callback(new Error('必须年满18岁'));
            } else {
              callback();
            }
          }
        }, 1000);
      };
      var validatePass = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入密码'));
        } else {
          if (this.loginForm.checkPass !== '') {
            this.$refs.loginForm.validateField('checkPass');
          }
          callback();
        }
      };
      var validatePass2 = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请再次输入密码'));
        } else if (value !== this.loginForm.pass) {
          callback(new Error('两次输入密码不一致!'));
        } else {
          callback();
        }
      };
      return {
        loginForm: {
          username: '',
          sex: '',
          email: '',
          pass: '',
          checkPass: '',
          age: ''
        },
        rules2: {
          pass: [
            { validator: validatePass, trigger: 'blur' }
          ],
          checkPass: [
            { validator: validatePass2, trigger: 'blur' }
          ],
          age: [
            { validator: checkAge, trigger: 'blur' }
          ],
          email:[
            { required: true, message: '请输入邮箱地址', trigger: 'blur' },
            { type: 'email', message: '请输入正确的邮箱地址', trigger: ['blur','change'] }
          ]
        }
      };
    },
    methods: {
      // submitForm(formName) {
      //   console.log(this.$refs[formName])
      //   this.$refs[formName].validate((valid) => {
      //     if (valid) {
      //       alert('submit!');
      //     } else {
      //       console.log('error submit!!');
      //       return false;
      //     }
      //   });
      // },
      async submitForm (formName) {
      try {
        // 表单验证
        await this.$refs[formName].validate()
        console.log(this.loginForm)
        const res = await axios.post(
          'user/save/',
          {
            'userName': this.loginForm.username,
            'sex': this.loginForm.sex,
            'email': this.loginForm.email,
            'age': this.loginForm.age,
            'passWord': this.loginForm.pass
          }
          
        )
        console.log('注册返回的数据', res)
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