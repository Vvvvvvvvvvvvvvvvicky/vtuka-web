<template>
  <body id="login">
      <div>
          用户名：<input type="text" v-model="loginInfoVo.username" placeholder="请输入用户名"/>
          <br/>
          密码：<input type="password" v-model="loginInfoVo.password" placeholder="请输入密码"/>
          <br/>
          <button v-on:click="login">登录</button>
      </div>
  </body>
</template>

<script>
export default{
  name: 'Login',
  data () {
    return {
      rules: {
        username: [{required: true, message: '用户名不能为空', trigger: 'blur'}],
        password: [{required: true, message: '密码不能为空', trigger: 'blur'}]
      },
      checked: true,
      loginInfoVo: {
        username: '',
        password: ''
      },
      loading: false
    }
  },
  methods: {
    login () {
      var _this = this
      this.$axios
        .post('/login', {
          username: this.loginInfoVo.username,
          password: this.loginInfoVo.password
        })
        .then(resp => {
          if (resp.data.code === 200) {
            var data = resp.data.data
            _this.$store.commit('login', data)
            var path = _this.$route.query.redirect
            _this.$router.replace({path: path === '/' || path === undefined ? '/home' : path})
          }
        })
        .catch(failResponse => {})
    }
  }
}
</script>
