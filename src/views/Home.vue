<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <!--插入图片-->
    <div class="login-wrap" v-show="showLog">
<!--      绑定显示事件，登录时显示以下内容-->
      <h3>用户登录页面</h3>
<!--      标题-->
      <p v-show="showTips">{{ Tips }}</p>
<!--      绑定显示事件，显示提示信息-->
      <input type="text" placeholder="请输入用户ID" v-model="user.id" />
<!--      输入框-->
      <input type="password" placeholder="请输入密码" v-model="user.password" />
<!--      输入框-->
      <button v-on:click="login">登录</button>
<!--      按钮-->
      <span v-on:click="ToRegister">没有账号？马上注册</span>
<!--      绑定显示事件，转入注册页面-->
    </div>

    <div class="register-wrap" v-show="showReg">
<!--      绑定显示事件，注册时显示以下内容-->
      <h3>用户注册页面</h3>
      <p v-show="showTips">{{ Tips }}</p>
<!--      绑定显示事件-->
      <input type="text" placeholder="请输入用户ID" v-model="user.id" />
<!--      输入框-->
      <input type="text" placeholder="请输入用户姓名" v-model="user.name" />
<!--      输入框-->
      <input type="number" placeholder="请输入用户年龄" v-model="user.age" />
<!--      输入框-->
      <input type="radio" id="first" value="true" v-model="user.sex" />
<!--      输入框-->
      <label for="first">男</label>
<!--      标签-->
      <input type="radio" id="second" value="false" v-model="user.sex" />
      <label for="second">女</label>
<!--      标签-->
      <input type="password" placeholder="请输入密码" v-model="user.password" />
      <input
        type="password"
        placeholder="请再次输入密码"
        v-model="user.password2"
      />
      <button v-on:click="register">注册</button>
      <span v-on:click="ToLogin">已有账号？马上登录</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data: function () {
    return {
      // 是否展示登录页面
      showLog: true,
      // 是否展示注册页面
      showReg: false,
      // 是否展示提示内容
      showTips: true,
      Tips: '',
      user: {
        // 年龄默认值为18
        age: 18,
        id: '',
        name: '',
        password: '',
        password2: '',
        // 性别默认值为男
        sex: true
      }
    }
  },
  // 处理事件的方法定义
  methods: {
    // 跳转到注册页面的方法
    ToRegister () {
      this.showReg = true
      this.showLog = false
    },
    // 跳转到注册页面的方法
    ToLogin () {
      this.showLog = true
      this.showReg = false
    },
    // 点击注册后的处理方法
    register: function () {
      if (this.user.id === '') { // 用户Id为空，显示提示信息
        this.Tips = '必须输入用户Id'
      } else if (this.user.name === '') { // 用户名为空，显示提示信息
        this.Tips = '必须输入用户名'
      } else if (this.user.password === '' || this.user.password2 === '') { // 密码为空，显示提示信息
        this.Tips = '必须输入密码'
      } else if (this.user.password !== this.user.password2) { // 两次密码输入不一致，显示提示信息
        this.Tips = '两次密码输入不一致，请检查'
      } else {
        this.$http // 完成对服务层的访问
          .post('http://localhost:8000/homework/api/User/register2', this.user)
          .then((res) => { // 响应成功时调用
            if (res.status === 200) {
              // $router完成路由切换，传递内容
              this.$router.push({ name: 'Main', params: { user: this.user } })
              // this.$router.push({ name: 'Main', params: { user: 1 } })
            } else { // 响应失败时调用
              this.Tips = '注册失败，请联系管理员！'
            }
          })
      }
    },
    // 点击登录后的处理方法
    login: function () {
      if (this.user.id === '') { // 用户Id为空，显示提示信息
        this.Tips = '必须输入用户Id'
      } else if (this.user.password === '') { // 密码为空，显示提示信息
        this.Tips = '必须输入密码'
      } else {
        this.$http // 完成对服务层的访问
          .post('http://localhost:8000/homework/api/User/login2', this.user)
          .then((res) => { // 响应成功时调用
            if (res.status === 200) {
              // $router完成路由切换，传递内容
              this.$router.push({ name: 'Main', params: { user: res.data } })
            } else { // 响应失败时调用
              this.Tips = '登陆失败！请检查用户名和密码！'
            }
          })
      }
    }
  }
}
</script>
<style>
.login-wrap {
  /*居中对齐*/
  text-align: center;
}
/*输入框样式*/
input {
  display: block;
  width: 250px;
  height: 40px;
  line-height: 40px;
  margin: 0 auto 10px;
  outline: none;
  border: 1px solid #888;
  padding: 10px;
  box-sizing: border-box;
}

p {
  color: red;
}
/*按钮样式*/
button {
  display: block;
  width: 250px;
  height: 40px;
  line-height: 40px;
  border: none;
  background-color: #41b883;
  color: #fff;
  font-size: 16px;
  margin: 0 auto 5px;
}

span {
  cursor: pointer;
}

span:hover {
  color: #41b883;
}
</style>
