<template>
  <body id="login">
    <img :src="logoTitleUrl" class="logo-title">
    <el-form :model="loginForm" status-icon :rules="rule2" class="login-container" ref="loginForm">
      <el-form-item prop="username">
        <el-input v-model="loginForm.username" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" v-model="loginForm.password" auto-complete="off"></el-input>
      </el-form-item>
      <el-checkbox class="login_remember" v-model="checked" >
        <span style="color: #1b1a1a">记住密码</span>
      </el-checkbox>
      <el-form-item>
        <el-button @click="submitForm('loginForm')">登录</el-button>
        <el-button @click="resetForm('loginForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </body>
</template>

<script>
export default{
  data () {
    var validatePassword = (rule, value, callback) => {
      if (!value) {
        return callback(new Error('请输入密码'));
      }
      setTimeout(() => {callback();}, 1000);
    };
    var validateUsername = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入用户名'));
      } else {
        callback();
      }
    };
    return {
      loginForm: {
        username: '',
        password: ''
      },
      checked: false,
      rule2: {
        username: [
          { validator: validateUsername, trigger: 'blur' }
        ],
        password: [
          { validator: validatePassword, trigger: 'blur' }
        ]
      },
      logoTitleUrl: "/static/logo_title.png"
    };
  },
  methods: {
    login () {
      var _this = this;
      this.$axios
        .post('/login', {
          username: this.loginForm.username,
          password: this.loginForm.password
        })
        .then(resp => {
          if (resp.data.status === 200) {
            //var data = resp.data.data
            //_this.$store.commit('login', data)
            var path = _this.$route.query.redirect
            _this.$router.replace({path: path === '/' || path === undefined ? '/home' : path})
          } else {
            this.$nextTick(()=>{
              this.$message.error(resp.data.msg);
              this.$options.methods.resetForm.bind(this)('loginForm');
            });
          }
        })
        .catch(resp => {
          this.$message.error('请求失败,请重试');
        })
    },
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$options.methods.login.bind(this)();
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm (formName) {
      var _this = this;
      this.$refs[formName].resetFields();
    }
  }
}
</script>
<style>
  #login {
    background:url("/static/login_bg.jpg") no-repeat top center;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
    color: #ecd2d2;
  }
  body{
     margin: -5px 0px;
  }
  .login-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin-top: -50px;
    margin-left: -150px;
    width: 300px;
    top: 50%;
    left: 50%;
    padding: 35px ;
    border: 1px solid #908484;
    box-shadow: 0 0 50px #cac6c6;
    position: absolute;/*绝对定位*/
  }
  .el-button {
    opacity: 0.6;
    background: rgba(255, 255, 255, 0.5); 
    color:  #1b1a1a;
    text-shadow: 1px 2px 3px #ecb4c1;
    font-size: 20px;
    border: 1px  rgb(255, 255, 255);
    width: 140px;
  }
  .el-input__inner {
    opacity: 0.6;
  }
  
  .login_remember {
    padding: 0 25px 20px 100px;
    font-size: 18px;
    text-shadow: 1px 2px 3px #c7abab;
  }
  .el-form-item__content {
    text-align: center;
  }
  .logo-title {
    width: 500px;
    margin-top: -250px;
    margin-left: -250px;
    top:50%;
    left: 50%;
    padding: 35px ;
    position: absolute;/*绝对定位*/
  }
</style>