<template>
  <div
    style="
      width: 100%;
      height: 100vh;
      background-image: linear-gradient(to bottom right, lightblue, #fff);
      overflow: hidden;
    "
  >
    <div style="width: 400px; margin: 150px auto">
      <div style="font-size: 30px; text-align: center; padding: 30px">欢迎登录</div>
      <el-form ref="form" :model="form" size="normal" :rules="rules">
        <el-form-item prop="username">
          <el-input v-model="form.userName" placeholder="用户名" size="large">
            <template #prefix>
              <el-icon><User /></el-icon>
            </template>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="form.password" show-password placeholder="密码" size="large">
            <template #prefix>
              <el-icon><Lock /></el-icon>
            </template>
          </el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="login" style="width: 100%" size="large"
            >登 录</el-button
          >
        </el-form-item>
        <el-form-item>
          <!-- <span>已有账号？</span> -->
          <el-button
            type="primary"
            @click="$router.push('/register')"
            link
            text
            style="width: 100%"
            >去注册</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
import request from "@/utils/request";
export default {
  name: "Login",
  data() {
    return {
      form: {},
      rules: {
        userName: [{ required: true, message: "请输入用户名", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
    };
  },
  methods: {
    login() {
      this.$refs["form"].validate((valid) => {
        if (valid) {
          request.post("/user/login", this.form).then((res) => {
            if (res.code === "0") {
              this.$message.success("登录成功");
              localStorage.setItem("user", JSON.stringify(res.data));
              request.get("/menu/usermenus/" + res.data.id).then((res) => {
                localStorage.setItem("menus", JSON.stringify(res.data));
              });
              this.$router.push("/");
            } else {
              this.$message.error(res.msg);
            }
          });
        } else {
          return false;
        }
      });
      // request.post("/user/login", this.form).then((res) => {
      //   if (res.code === "0") {
      //     this.$message.success("登录成功");
      //     this.$router.push("/"); //跳转主页
      //   } else {
      //     this.$message.error(res.msg);
      //   }
      // });
    },
  },
};
</script>
<style scoped></style>
