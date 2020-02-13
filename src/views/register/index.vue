<template>
  <div class="register">
    <div class="register-container">
      <el-form ref="ruleForm" :model="ruleForm" status-icon :rules="rules" label-width="100px" class="demo-ruleForm">
        <el-form-item label="用户名" prop="name" class="el-form-item">
          <el-input v-model="ruleForm.name" />
        </el-form-item>
        <el-form-item label="密码" prop="pass" class="el-form-item">
          <el-input v-model="ruleForm.pass" type="password" autocomplete="off" />
        </el-form-item>
        <el-form-item label="确认密码" prop="checkPass" class="el-form-item">
          <el-input v-model="ruleForm.checkPass" type="password" autocomplete="off" />
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">创建</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
          <el-button @click="backLogin">返回</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
// import { register } from '@/api/user'
export default {
  name: 'Register',
  data() {
    var checkName = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('用户名不能为空'))
      } else {
        if (this.ruleForm.name !== '') {
          this.$refs.ruleForm.validateField('checkName')
        }
        callback()
      }
    }
    var validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'))
      } else {
        if (this.ruleForm.checkPass !== '') {
          this.$refs.ruleForm.validateField('checkPass')
        }
        callback()
      }
    }
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'))
      } else if (value !== this.ruleForm.pass) {
        callback(new Error('两次输入密码不一致!'))
      } else {
        callback()
      }
    }
    return {
      ruleForm: {
        pass: '',
        checkPass: '',
        name: ''
      },
      rules: {
        pass: [
          { validator: validatePass, trigger: 'blur' }
        ],
        checkPass: [
          { validator: validatePass2, trigger: 'blur' }
        ],
        name: [
          { validator: checkName, trigger: 'blur' }
        ]
      }
      // roleForm: null,
      // rules: null,
      // registerLoading: true
    }
  },
  mounted() {
    this.getRegisterInfo()
  },
  methods: {
    // fetchData() {
    //   this.registerLoading = true
    //   register().then(response => {
    //     this.roleForm = response.data.items
    //     this.rules = response.data.items
    //     this.registerLoading = false
    //   })
    // },
    // getRegisterInfo() {
    //   axios.get('/api/register.json').then(this.getRegisterInfoSucc)
    // },
    // getRegisterInfoSucc(res) {
    //   res = res.data
    //   if (res.ret && res.data) {
    //     const data = res.data
    //     this.roleForm = data.roleForm
    //     this.rules = data.rules
    //   }
    //   console.log(res)
    // },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('创建成功，请点击“确定”，并点击“返回”按钮进入登录页面。')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    },
    backLogin() {
      this.$confirm('此操作将退出至登录页, 是否已创建好新账户?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '请登录!'
        })
        this.$router.push({ path: this.redirect || '/' })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '请创建新账户！'
        })
      })
    }
  }
}
</script>

<style>
.register {
  width: 100%;
  height: 100%;
  background: url('./../../assets/login_images/bg.jpg') no-repeat 0px 0px;
  background-size: 100% 100%;
}
.register-container {
  position: relative;
  top: 28%;
  left: 32%;
  width: 480px;
  height: 300px;
  border: 1px solid rgb(255, 252, 252);
  border-radius: 20px;
  /* background: #faf6f6; */
}
.demo-ruleForm {
  position: absolute;
  padding: 40px 0px 20px 20px;
  width: 400px;
  height: 30px;
}
/* .el-form-item {
  color:rgb(255, 252, 252)
} */
</style>
