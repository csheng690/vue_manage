<template>
  <div class="login_container">
    <div class="login_box">
      <!--头像-->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <!--登录表单-->
      <el-form
        ref="loginFormRef"
        :model="loginForm"
        :rules="loginFormRules"
        label-width="0px"
        class="login_form"
      >
        <!--用户名-->
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="el-icon-user"
          ></el-input>
        </el-form-item>
        <!--密码-->
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="el-icon-lock"
            type="password"
          ></el-input>
        </el-form-item>
        <!--按钮-->
        <el-form-item class="btns">
          <el-button type="primary" @click="login" round>登录</el-button>
          <el-button type="info" @click="resetLoginForm" round>重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 登录表单的数据绑定对象
      loginForm: {
        username: "admin",
        password: "123456",
      },
      //   表单的验证规则对象
      loginFormRules: {
        username: [
          { required: true, message: "请输入用户名", trgger: "blur" },
          {
            min: 3,
            max: 11,
            message: "长度为3~11位的用户名",
            trigger: "blur",
          },
        ],
        password: [{ required: true, message: "请输入密码", trgger: "blur" }],
      },
    };
  },
  methods: {
    //   点击重置按钮，重置表单数据
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return;
        const { data: res } = await this.$http.post("login", this.loginForm);
        if (res.meta.status !== 200) return this.$message.error("登录失败");
        this.$message.success("登录成功");

        window.sessionStorage.setItem("token", res.data.token);
        this.$router.push("/home");
      });
    },
  },
};
</script>

<style Lang="less" scoped>
.login_container {
  height: 100%;
  background-color: #92f6f2;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 10px 10px #999;
}
.avatar_box {
  width: 130px;
  height: 130px;
  border-radius: 50%;
  padding: 4px;
  box-shadow: 0 10px 10px #999;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
.avatar_box img {
  widows: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 40px;
  box-sizing: border-box;
}

.btns {
  display: flex;
  justify-content: flex-end;
}
</style>
