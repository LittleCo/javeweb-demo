<template>
  <section class="login-container">
    <section class="login-box">
      <h2 class="login-header">学生交流平台</h2>
      <p class="project-details">课表查询，作业通知，文档下载，发布活动等功能等你体验</p>
      <el-input placeholder="输入学号" v-model="username"></el-input>
      <el-input placeholder="输入密码" v-model="password" type="password"></el-input>
      <div class="login-option">
        <el-checkbox v-model="checked">记住密码</el-checkbox>
        <a href="#">忘记密码？</a>
      </div>
      <div class="login-event">
        <el-button type="info" @click.stop="loginIn">登录</el-button>
        <el-button @click.stop="handleRegister">注册</el-button>
      </div>
    </section>
    <section class="menu-option">
      <nav class="navigate">
        <ul class="nav-list">
          <li class="nav-item" v-for="(item, index) in navlist" :key="index">
            <a :href="item.href">{{item.title}}</a>
          </li>
        </ul>
      </nav>
    </section>
  </section>
</template>

<script>
import { login, register } from "../api/users";
export default {
  data() {
    return {
      username: "",
      password: "",
      checked: false,
      navlist: [
        { href: "#", title: "关于我们" },
        { href: "#", title: "联系我们" },
        { href: "#", title: "报个bug" },
        { href: "#", title: "欢迎评价" }
      ]
    };
  },
  methods: {
    handleNotify(message, type) {
      this.$message({
        message: `${message}`,
        type: `${type}`
      });
    },
    loginIn() {
      login({
        u_id: this.username,
        u_password: this.password
      })
        .then(data => {
          if (data.state) {
            localStorage.setItem("username", this.username);
            this.handleNotify(`${data.message}`, "success");
            this.$router.push("/home");
          } else {
            this.handleNotify(`${data.message}`, "error");
          }
        })
        .catch(err => {
          console.log(err);
          this.handleNotify("登录时出错", "error");
        });
    },
    handleRegister() {
      register({
        u_id: this.username,
        u_password: this.password,
        type: "graduate"
      })
        .then(res => {
          if (res.state) {
            this.handleNotify(`${res.message}`, "success");
          } else {
            this.handleNotify(`该用户已注册`, "error");
          }
        })
        .catch(err => {
          console.log(err);
          this.handleNotify(`服务器出错`, "error");
        });
    }
  }
};
</script>
<style>
.el-input {
  text-align: left;
  height: 50px;
  padding-bottom: 5px;
}
.el-input__inner {
  padding: 10px 0;
  width: 80%;
  height: 100%;
  padding-left: 10px;
  box-shadow: 5px 5px 5px rgb(184, 182, 182);
}
.el-checkbox {
  color: #666;
}
</style>
<style lang="scss" scoped>
.login-container {
  display: flex;
  width: 100%;
  height: 100vh;
}
.login-box {
  width: 40%;
  background: #fff;
  padding: 120px 80px;
  .login-header {
    color: #666;
    font-size: 30px;
    text-align: left;
  }
  .project-details {
    font-size: 14px;
    width: 50%;
    color: #777;
    text-align: left;
    padding: 5px 0;
    margin-bottom: 95px;
  }
}
.login-option {
  display: flex;
  justify-content: space-between;
  margin-top: 100px;
  a {
    text-decoration: none;
    color: #333;
  }
}
.login-event {
  text-align: left;
  margin-top: 80px;
  .el-button {
    width: 140px;
    height: 40px;
  }
}
.menu-option {
  width: 60%;
  background: url("./../assets/images/login_7.jpg");
  background-size: cover;
}
.nav-list {
  display: flex;
  .nav-item {
    min-width: 22%;
    padding-top: 30px;
    list-style: none;
    a {
      text-decoration: none;
      color: rgb(231, 228, 228);
      font-size: 18px;
    }
  }
}
</style>
