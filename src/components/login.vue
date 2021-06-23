
<template>
    <div class="login-container">
      <div class="login-box">
        <div class="avatar-box">
          <img src="../assets/img/鹿.jpeg" alt="">
        </div>
        <!-- 表单-->
        <el-form :model="loginForm" :rules="loginFormRules" ref="loginFormRef" class="login-form">
          <!--用户名-->
          <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user" placeholder="用户名"></el-input>
          </el-form-item>
          <!--密码-->
          <el-form-item prop="password">
            <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" placeholder="密码" type="password"></el-input>
          </el-form-item>
          <el-form-item class="btns">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="resetLoginForm">清除</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      //表单数据绑定
      loginForm: {
        username:"admin",
        password:"123456"
      },
      //表单数据验证
      loginFormRules: {
        username: [
          {required: true, message: "请输入用户名", trigger: "blur" },
          {min: 1, max: 10, message: "长度在1到10个字符", trigger: "blur"}
        ],
        password: [
          {required: true, message: "请输入密码", trigger: "blur" },
          {min: 3, max: 10, message: "长度在3到10个字符", trigger: "blur"}
        ]
      }
    }
  },

  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },

    // 输出Promise，在前面加await修饰，await只修饰async
    login() {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return;
        const {data: res} = await this.$http.post("login", this.loginForm);
        if (res.meta.status !== 200)  return this.$message.error("登录失败,账号密码错误！");
        this.$message.success("登录成功！");
        //1.将登陆成功的token，保存到客户端的sessionStorage中
        //     1.1 项目中除了登陆之外的API接口，必须在登陆之后才能访问
        //     1.2 token只在当前网页打开期间生效，所以token保存在sessionStorage中
        window.sessionStorage.setItem("token", res.data.token);
        // 2.路由跳转
        this.$router.push('/home');

      })
    }

  }
}
</script>

<style lang="less" scoped>
  .login-container {
    height: 100%;
    background: #006400;
  }
  .login-box {
    width: 450px;
    height: 300px;
    background-color: white;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    .avatar-box {
      width: 150px;
      height: 150px;
      background-color: white;
      border: 1px solid #eee;
      border-radius: 50%;
      position: absolute;
      left: 50%;
      transform: translate(-50%,-50%);
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      img {
        width: 100%;
        height: 100%;
        border-radius: 50%;
      }
    }
  }

  .login-form{
    position: absolute;
    bottom: 0px;
    width: 100%;
    padding:  0 20px;
    box-sizing: border-box;
  }

  .btns {
    display: flex;
    justify-content: center;
    /*display: flex;*/
    /*justify-content: flex-end;*/
  }
</style>
