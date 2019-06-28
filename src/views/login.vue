<template>
  <el-row type="flex" class="row-bg" justify="center" align="middle">
    <el-col :xs="14" :sm="12" :md="10" :lg="8" :xl="6">
      <el-form
        :model="form"
        :rules="rules"
        ref="form"
        label-width="80px"
        label-position="top"
        class="login-form"
      >
        <el-form-item label="用户名" prop="username">
          <el-input v-model="form.username"></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input v-model="form.password"></el-input>
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="submitForm('form')">登录</el-button>
          <el-button @click="resetForm('form')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ]
      }
    };
  },

  methods: {
    submitForm(form) {
      this.$refs[form].validate(item => {
        if (item) {
          axios({
            url: "http://localhost:8888/api/private/v1/login",
            method: "post",
            data: this.form
          }).then(res => {
            //  console.log(res)
            if (res.data.meta.status === 200) {
              // 本地存储设置  存储名 值
              localStorage.setItem("token", res.data.token);
              this.$router.push("/home");
            } else {
              console.log("失败");
            }
          });
        } else {
          alert("输入账号或密码错误");
          return;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>

<style>
.row-bg {
  height: 100%;
  background-color: #2d434c;
}

.login-form {
  background-color: #fff;
  padding: 30px 20px;
  border-radius: 10px;
  min-width: 400px;
}
</style>
