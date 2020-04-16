<template>
  <div class="login_container">
    <div class="login_box">
      <div class="box-div">登录界面</div>
      
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormrules" label-width="0px" class="loginfrom">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="iconfont icon-yonghu3"
            placeholder="请输入用户名"
          ></el-input>
        </el-form-item>
        <!-- 密码 -->

        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-mima"
            placeholder="请输入密码"
            show-password
          ></el-input>
        </el-form-item>
        <!-- 按键 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script src="@/iconfont.js"></script>
<script>
export default {
  data() {
    return {
      // 登录表单的数据绑定
      loginForm: {
        username: 'tian7712',
        password: ''
      },
      // 表单的验证规则
      loginFormrules: {
        username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            { min: 3, max: 8, message: '长度在 3 到 8 个字符', trigger: 'blur' }
          ],
        password: [
            { required: true, message: '请输入密码', trigger: 'change' },
            { min: 6, max: 15, message: '长度在 6 到 15 个字符（密码123456）', trigger: 'blur' }
          ]
      }
    };
  },
  methods: {
    resetLoginForm() {
      // console.log(this)
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async valid =>{
        // console.log(valid);
        if(!valid) return;
        // const result = this.$http.post("login", this.loginForm);
        // console.log(result);
        const { data: res } = await this.$http.post("login", this.loginForm);
        if (res.meta.status !== 200) return this.$message.error("登录失败");
        this.$message.success("登陆成功");
        console.log(res);
        // 保存token
        window.sessionStorage.setItem("token", res.data.token);
        window.sessionStorage.setItem("username", res.data.username);
        // 跳转到后台主页
        this.$router.push("/home");
      });
    }

  }
};
</script>

<style lang="less" scoped>
.login_container {
  background-color: black;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  border-radius: 3px;
  background-color: white;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  .box-div{
    text-align: center;
    font-size: 18px;
    font-weight: bold;
    margin: 10px;
    height: 50px;
    line-height: 50px;
  }
}
.btns {
  display: flex;
  justify-content: flex-end;
}
.loginfrom {
  position: absolute;
  bottom: 0px;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
</style>
