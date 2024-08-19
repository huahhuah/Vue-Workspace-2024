<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div>
    <el-container>
      <el-header class="layout-header">
        <nav>
          <RouterLink to="/week01">
            <el-button color="#626aef" :dark="isDark" plain>week01</el-button>
          </RouterLink>
          <RouterLink to="/week02">
            <el-button color="#626aef" :dark="isDark" plain>week02</el-button>
          </RouterLink>
          <RouterLink to="/week03">
            <el-button color="#626aef" :dark="isDark" plain>week03</el-button>
          </RouterLink>
        </nav>
      </el-header>

      <el-main class="layout-main">
        <router-view />
      </el-main>
    </el-container>
  </div>
</template>
<style scoped>
.el-header {
  background-color: #000;
  height: 50px;
}
nav {
  text-align: center;
  line-height: 50px;
}
</style>
