<template>
<!-- template里面没有组件会导致项目启动卡住 -->
  <div>
    <el-button type="info" @click="logout">退出</el-button>
  </div>
</template>

<script>
  export default {
    name: 'Home',
    methods: {
      logout() {
        window.sessionStorage.clear();
        this.$router.push("/login");
      }

    }
  }
</script>

<style scoped>

</style>
