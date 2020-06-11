<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="logo" />
      </div>
      <!-- 登录表单区 -->
      <el-form
        ref="loginFormRef"
        :rules="rules"
        :model="loginForm"
        class="login_form"
      >
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-user"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input
            type="password"
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
          ></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginFrom">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      /* 登录表单的数据绑定对象 */
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      /* 表单验证规则 */
      rules: {
        /* 验证用户名 */
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          {
            min: 3,
            max: 5,
            message: '请输入正确的用户名（3~10）',
            trigger: 'blur'
          }
        ],
        /* 验证密码 */
        password: { required: true, message: '请输入登录密码', trigger: 'blur' }
      }
    }
  },
  methods: {
    /* 点击重置按钮重置表单 */
    resetLoginFrom() {
      /*  console.log(this) */
      this.$refs.loginFormRef.resetFields()
      this.name = ''
      this.password = ''
    },
    /* 登录验证 ：valid预验证、发起请求（解构接收）、登录状态验证 */
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        /* console.log(res) */
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登陆成功')
        /* 登录成功后将返回的数据中的token保存在sessionStorage中 */
        window.sessionStorage.setItem('token', res.data.token)
        /* 编程式导航跳转到后台主页，路由地址是/home */
        this.$router.push('/home')
      })
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
  position: absolute;
  left: 50%;
  top: 50%;
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  transform: translate(-50%, -50%);
  .avatar_box {
    position: absolute;
    left: 50%;
    padding: 10px;
    height: 130px;
    width: 130px;
    background-color: #fff;
    border: 1px solid #eee;
    border-radius: 50%;
    box-shadow: 0 0 10px #ddd;
    transform: translate(-50%, -50%);
    img {
      width: 100%;
      height: 100%;
      background-color: #eee;
      border-radius: 50%;
    }
  }
}
.btns {
  display: flex;
  justify-content: flex-end;
}
.login_form {
  position: absolute;
  bottom: 0;
  padding: 0 20px;
  width: 100%;
  box-sizing: border-box;
}
</style>
