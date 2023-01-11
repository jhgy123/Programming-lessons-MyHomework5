<template >
  <div>
  <h1>用户信息展示</h1>
  <p v-for="(user,i) in this.$route.params.users" :key="i">姓名：{{user.name}} id:{{user.id}} 密码：{{user.password}}</p>

  </div>
</template>

<script>
export default {
  name: 'Display'
}
</script>

<style scoped>

</style>
