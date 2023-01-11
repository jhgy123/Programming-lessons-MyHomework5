<template>
  <div>
    <img alt="Vue logo" src="../assets/logo.png" />
    <!--插入图片-->
    <div class="update-wrap" v-show="showUpd">
      <!--      绑定显示事件，登录时显示以下内容-->
      <h3>用户修改页面</h3>
      <!--      标题-->
      <p v-show="showTips">{{ Tips }}</p>
      <!--      绑定显示事件，显示提示信息-->
      <input type="text" placeholder="请输入修改用户ID" v-model="user.id" />
      <!--      输入框-->
      <input type="text" placeholder="请输入修改后的姓名" v-model="user.name" />
      <!--      输入框-->
      <input type="password" placeholder="请输入修改后的密码" v-model="user.password" />
      <button v-on:click="update">修改</button>
      <!--      按钮-->
      <span v-on:click="ToDelete">删除用户</span>
      <!--      绑定显示事件，转入删除页面-->
    </div>

    <div class="delete-wrap" v-show="showDel">
      <!--      绑定显示事件，注册时显示以下内容-->
      <h3>用户删除页面</h3>
      <p v-show="showTips">{{ Tips }}</p>
      <!--      绑定显示事件-->
      <input type="text" placeholder="请输入要删除的用户ID" v-model="user.id" />
      <button v-on:click="delet">删除</button>
      <!--      按钮-->
      <span v-on:click="ToUpdate">修改用户</span>
    </div>
    <button v-on:click="display">用户信息展示</button>
  </div>
</template>

<script>
export default {
  name: 'DeleteAndUpdate',
  data: function () {
    return {
      // 是否展示修改页面
      showUpd: true,
      // 是否展示删除页面
      showDel: false,
      // 是否展示提示内容
      showTips: true,
      Tips: '',
      users: '',
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
    ToUpdate () {
      this.showUpd = true
      this.showDel = false
    },
    // 跳转到注册页面的方法
    ToDelete () {
      this.showUpd = false
      this.showDel = true
    },
    // 点击删除后的处理方法
    delet: function () {
      if (this.user.id === '') { // 用户Id为空，显示提示信息
        this.Tips = '必须输入用户Id'
      } else {
        this.$http // 完成对服务层的访问
          .put('http://localhost:8000/homework/api/User/delete', this.user.id)
          .then((res) => { // 响应成功时调用
            if (res.status === 200) {
              this.Tips = '删除成功！'
            } else { // 响应失败时调用
              this.Tips = '注册失败，请联系管理员！'
            }
          })
      }
    },
    // 点击修改后的处理方法
    update: function () {
      if (this.user.id === '') { // 用户Id为空，显示提示信息
        this.Tips = '必须输入用户Id'
      } else {
        this.$http // 完成对服务层的访问
          .put('http://localhost:8000/homework/api/User/update', this.user)
          .then((res) => { // 响应成功时调用
            if (res.status === 200) {
              this.Tips = '修改成功！'
            } else { // 响应失败时调用
              this.Tips = '登陆失败！请检查用户名和密码！'
            }
          })
      }
    },
    display: function () {
      this.$http // 完成对服务层的访问
        .post('http://localhost:8000/homework/api/User/display')
        .then((res) => { // 响应成功时调用
          if (res.status === 200) {
            this.$router.push({ name: 'Display', params: { users: res.data } })
          } else { // 响应失败时调用
            this.Tips = '用户信息展示失败！'
          }
        })
    }
  }
}
</script>

<style scoped>
.update-wrap {
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
