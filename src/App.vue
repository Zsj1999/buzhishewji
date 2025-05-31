<template>
  <div id="app">
    <el-container>
      <el-header>
        <nav class="nav-menu">
          <div class="logo">不止设计</div>
          <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            router
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
            <el-menu-item index="/">首页</el-menu-item>
            <el-menu-item index="/works">作品展示</el-menu-item>
            <el-menu-item index="/about">关于我们</el-menu-item>
            <el-menu-item index="/contact">联系我们</el-menu-item>
          </el-menu>
        </nav>
      </el-header>
      <el-main>
        <router-view/>
      </el-main>
      <el-footer>
        <div class="footer-content">
          <p>© 2024 不止设计 版权所有</p>
        </div>
      </el-footer>
    </el-container>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      activeIndex: '/'
    }
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft YaHei', Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.el-header {
  padding: 0;
  background-color: #545c64;
  color: white;
  position: fixed;
  width: 100%;
  top: 0;
  z-index: 1000;
}

.nav-menu {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  height: 60px;
  max-width: 1200px;
  margin: 0 auto;
}

.logo {
  font-size: 24px;
  font-weight: bold;
  color: #ffd04b;
}

.el-menu {
  border: none;
}

.el-main {
  padding: 0;
  margin-top: 60px;
  min-height: calc(100vh - 120px);
}

.el-footer {
  background-color: #545c64;
  color: white;
  text-align: center;
  padding: 20px 0;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

@media screen and (max-width: 768px) {
  .nav-menu {
    flex-direction: column;
    height: auto;
    padding: 10px;
  }
  
  .logo {
    margin-bottom: 10px;
  }
  
  .el-menu {
    width: 100%;
  }

  .el-menu-item {
    padding: 0 10px;
    font-size: 14px;
  }

  .el-main {
    margin-top: 100px;
  }
}
</style>
