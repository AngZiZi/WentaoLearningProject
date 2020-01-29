<template>
  <div class="login-container">
    <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">

      <div class="title-container">
        <h3 class="title">
          <div class="iconfont-wrapper">
            <span class="iconfont">&#xe602;</span>
          </div>
        </h3>
      </div>

      <el-form-item prop="username">
        <span class="svg-container">
          <svg-icon icon-class="user" />
        </span>
        <el-input
          ref="username"
          v-model="loginForm.username"
          placeholder="用户名"
          name="username"
          type="text"
          tabindex="1"
          auto-complete="on"
        />
      </el-form-item>

      <el-form-item prop="password">
        <span class="svg-container">
          <svg-icon icon-class="password" />
        </span>
        <el-input
          :key="passwordType"
          ref="password"
          v-model="loginForm.password"
          :type="passwordType"
          placeholder="密码"
          name="password"
          tabindex="2"
          auto-complete="on"
          @keyup.enter.native="handleLogin"
        />
        <span class="show-pwd" @click="showPwd">
          <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
        </span>
      </el-form-item>

      <el-button :loading="loading" type="primary" style="width:100%;margin-bottom:30px;" @click.native.prevent="handleLogin">登 录</el-button>

      <div class="tips">
        <span class="tips-left"><a>注册新用户</a></span>
        <span class="tips-right"><a>忘记密码？</a></span>
      </div>

    </el-form>
  </div>
</template>

<script>
import { validUsername } from '@/utils/validate'
import { validPassword } from '@/utils/validate'

export default {
  name: 'Login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('请输入正确的用户名'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('密码长度不能低于6位'))
      } else if (!validPassword(value)) {
        callback(new Error('请输入正确的密码'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'yudong',
        password: '123123'
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      loading: false,
      passwordType: 'password',
      redirect: undefined
    }
  },
  watch: {
    $route: {
      handler: function(route) {
        this.redirect = route.query && route.query.redirect
      },
      immediate: true
    }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        console.log(valid)
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm).then(() => {
            this.$router.push({ path: this.redirect || '/' })
            this.loading = false
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style lang="scss">
/* 修复input 背景不协调 和光标变色 */
/* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */
$bg:#ffffff;
$light_gray:rgb(46, 45, 45);
$cursor: #fff;

@supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
  .login-container .el-input input {
    color: $cursor;
  }
}

/* reset element-ui css */
.login-container {
  .el-input {
    display: inline-block;
    height: 47px;
    width: 85%;

    input {
      background: transparent;
      // background-color: rgb(218, 218, 218);
      border: 0px;
      -webkit-appearance: none;
      border-radius: 0px;
      padding: 12px 5px 12px 15px;
      color: $light_gray;
      height: 47px;
      caret-color: $cursor;

      &:-webkit-autofill {
        box-shadow: 0 0 0px 1000px $bg inset !important;
        -webkit-text-fill-color: $cursor !important;
      }
    }
  }

  .el-form-item {
    border: 1px solid rgba(0, 0, 0, 0.1);
    background: rgba(139, 139, 139, 0.1);
    border-radius: 10px;
    // color: #000000;
  }
}
</style>

<style lang="scss" scoped>
$bg:#ffffff;
$dark_gray:#424242;
$light_gray:#eee;

.login-container {
  min-height: 100%;
  width: 100%;
  background-color: $bg;
  background: url('./../../assets/login_images/bg.jpg') no-repeat 0px 0px;
  background-size: 1370px 657px;
  // background: no-repeat;
  overflow: hidden;

  .login-form {
    position: relative;
    width: 520px;
    max-width: 100%;
    padding: 160px 35px 0;
    margin: 0 auto;
    overflow: hidden;
  }

  .tips {
    position: relative;
    font-size: 14px;
    color: #fff;
    margin-bottom: 10px;
    // background: #424242;

    span {
     width: 60px;
     height: 14px;
    }

    .tips-left a{
      padding: 0 100px 0 50px;
      color: #fff
    }
    .tips-left:hover a{
      text-decoration: underline;
      color: red;
      cursor: pointer
    }
    .tips-right a{
      padding: 0 20px 0 100px;
      color: #fff
    }
    .tips-right:hover a{
      text-decoration: underline;
      color: red;
    }
  }

  .svg-container {
    padding: 6px 5px 6px 15px;
    color: $dark_gray;
    vertical-align: middle;
    width: 30px;
    display: inline-block;
  }

  .title-container {
    position: relative;

    .title {
      font-size: 26px;
      color: $light_gray;
      margin: 0px auto 40px auto;
      text-align: center;
      font-weight: bold;
    }
  }

  .show-pwd {
    position: absolute;
    right: 10px;
    top: 7px;
    font-size: 16px;
    color: $dark_gray;
    cursor: pointer;
    user-select: none;
  }
  .iconfont-wrapper {
    width: 80px;
    height: 80px;
    position: relative;
    left: 180px;
    background: #cecece;
    border-radius: 50px 50px 50px 50px;
  }
  .iconfont {
    position: absolute;
    top: 15px;
    left: 15px;
    line-height: 50px;
    text-align: center;
    font-size: 50px;
    color: rgb(255, 255, 255);
  }
}
</style>
