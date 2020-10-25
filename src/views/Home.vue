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
        <el-aside :width="isCollapse ? '64px' : '200px'">
          <!--菜单折叠-->
          <div class="toggle-button" @click="toggleCollapse">|||</div>
          <el-menu
            background-color="#2E2E2E"
            text-color="#fff"
            active-text-color="#ccc" unique-opened :collapse="isCollapse" :collapse-transition="false">
            <!--1级菜单-->
            <el-submenu :index="menu.id + ''" v-for="menu in menuList" :key="menu.id">
              <!--1级菜单的模板-->
                <template slot="title">
                  <i :class="menu.icon"></i><span>{{menu.menuName}}</span>
                </template>
                <!--2级菜单-->
                <el-menu-item :index="menu.id + '-' + submenu.id + ''" v-for="submenu in menu.children" :key="submenu.id">
                  <template slot="title"><i :class="submenu.icon"></i>
                    <span>{{submenu.menuName}}</span>
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
  data() {
    return {
      menuList: [
        {
          id: 1,
          menuName: '系统管理',
          path: null,
          order: 1,
          icon: 'el-icon-menu',
          children: [
            {
              id: 104,
              menuName: '用户管理',
              path: null,
              order: 1,
              icon: 'el-icon-service',
              children: []
            },
            {
              id: 105,
              menuName: '权限管理',
              path: null,
              order: 2,
              icon: 'el-icon-bell',
              children: []
            }
          ]
        },
        {
            id: 2,
            menuName: '系统管理2',
            path: null,
            order: 1,
            icon: 'el-icon-menu',
            children: [
              {
                id: 204,
                menuName: '用户管理2',
                path: null,
                order: 1,
                icon: 'el-icon-service',
                children: []
              },
              {
                id: 205,
                menuName: '权限管理2',
                path: null,
                order: 2,
                icon: 'el-icon-bell',
                children: []
              }
            ]
        }
      ],
      meta: {
        msg: '获取菜单列表成功',
        status: 200
      }
      ,
      isCollapse: false
    }
  },
  created () {
    this.getMenuList()
  },
  methods: {
    logout () {
      alert('退出')
    },
    getMenuList () {

    },
    toggleCollapse() {
      this.isCollapse = !this.isCollapse;
    }
  }
}
</script>

<style lang="less" scoped>
  .logo {
    width: 40px;
  }
  .el-header{
    background-color: #2E2E2E;
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
    background-color: #424242;
    .el-menu{
      border-right: none;
    }
  }
  .el-main{
    background-color: #fff;
  }

  .toggle-button{
    background-color: gray;
    font-size: 10px;
    color: #fff;
    line-height: 25px;
    text-align: center;
    letter-spacing: 0.3em;
    cursor: pointer;
  }

</style>
