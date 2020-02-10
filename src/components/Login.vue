<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png" />
      </div>
      <div>
        <!--label-width="80px"用来占位-->
        <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form">
          <!--用户名-->
          <el-form-item prop="username">
            <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username"></el-input>
          </el-form-item>
          <!--密码-->
          <el-form-item prop="password">
            <el-input
              prefix-icon="iconfont icon-3702mima"
              type="password"
              v-model="loginForm.password"
            ></el-input>
          </el-form-item>
          <!--button-->
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登陆</el-button>
            <el-button type="info" @click="resetLoginForm">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 表单验证规则
      loginFormRules: {
        username: [
          { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        // es6的解构赋值
        const { data: result } = await this.$http.post('login', this.loginForm)
        if (result.meta.status !== 200) return this.$message('这是一条消息提示')
        console.log('登陆成功')
        // 1.将登陆成功之后的token,保存到客户端的sessionStorage中
        //   1.1 项目中除了登陆之外的其他api接口，必须在登陆之后才能访问
        //   1.2 token只应在当前网站打开期间生效，所以将token保存在sessionStorage中
        window.sessionStorage.setItem('token', result.data.token)
        // 2.通过编程是导航跳转到后台主页，路由地址是/home
        this.$router.push('/home')
      })
    },
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>
<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: (translate(-50%, -50%));
  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    padding: 10px;
    border-radius: 50%; //圆角
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
  .login_form {
    position: absolute;
    width: 100%;
    padding: 0 20px;
    bottom: 0;
    box-sizing: border-box;
  }
  .btns {
    display: flex;
    justify-content: flex-end;
  }
}
</style>
