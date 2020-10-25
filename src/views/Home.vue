<template>
    <el-container style="height: 100%">
      <!-- 头部区域 -->
      <el-header>
        <div>
          <img class="logo" src="../assets/logo.png" alt="后台管理系统">
          <span>后台管理系统</span>
        </div>
        <el-button type="info" @click="logout">退出</el-button>
      </el-header>
      <!-- 页面主体区域 -->
      <el-container>
        <!-- 左侧区域 -->
        <el-aside width="200px">
          <el-menu
            background-color="dodgerblue"
            text-color="#fff"
            active-text-color="#ccc">
            <!--1级菜单-->
            <el-submenu index="1">
              <!--1级菜单的模板-->
                <template slot="title">
                  <i class="el-icon-menu"></i>系统管理
                </template>
                <!--2级菜单-->
                <el-menu-item index="1-1-1">
                  <template slot="title"><i class="el-icon-service"></i>
                    <span>用户管理</span>
                  </template>
                </el-menu-item>
              </el-submenu>
          </el-menu>
        </el-aside>
        <!-- 右侧内容主体 -->
        <el-main>Main</el-main>
      </el-container>
    </el-container>
</template>

<script>
export default {
  created () {
    this.getMenuList()
  },
  methods: {
    logout () {
      alert('退出')
    },
    getMenuList () {

    }
  }
}
</script>

<style lang="less" scoped>
  .logo {
    width: 40px;
  }
  .el-header{
    background-color: dodgerblue;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #fff;
    font-size: 20px;
    > div {
      display: flex;
      align-items: center;
      span{
        margin-left: 15px;
      }
    }
  }
  .el-aside{
    background-color: dodgerblue;
  }
  .el-main{
    background-color: antiquewhite;
  }
</style>
