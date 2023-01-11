<template>
<!--  显示用户姓名-->
  <div class="main">
    <h1>欢迎登录！</h1>
    <h1>{{ this.$route.params.user.name }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Main'
}
</script>

<style scoped></style>
