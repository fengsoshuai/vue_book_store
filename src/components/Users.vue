<template>
    <div id="users">
      <!--面包屑导航-->
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户列表</el-breadcrumb-item>
      </el-breadcrumb>

      <!--卡片视图-->
      <el-card>
        <div style="margin-top: 15px;">
          <!--搜索框、添加用户-->
          <el-row :gutter="20">
            <el-col :span="7">
              <el-input placeholder="请输入内容">
                <el-button slot="append" icon="el-icon-search"></el-button>
              </el-input></el-col>
            <el-col :span="4">
              <el-button type="primary">添加</el-button>
            </el-col>
          </el-row>

          <!--用户列表-->
          <el-table
            :data="userList"
            style="width: 100%">
            <el-table-column
              type="index"
              :index="indexMethod">
            </el-table-column>
            <el-table-column
              prop="date"
              label="日期"
              width="180">
            </el-table-column>
            <el-table-column
              prop="name"
              label="姓名"
              width="180">
            </el-table-column>
            <el-table-column
              prop="address"
              label="地址">
            </el-table-column>
          </el-table>

        </div>
      </el-card>

    </div>
</template>

<script>
export default {
  data() {
    return {
      // 获取用户列表
      queryInfo: {
        query: '',
        pageNum: 1,
        pageSize: 2
      },
      userList: [{
        date: '2016-05-02',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333,
        tag: '家'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1517 弄',
        zip: 200333,
        tag: '公司'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1519 弄',
        zip: 200333,
        tag: '家'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1516 弄',
        zip: 200333,
        tag: '公司'
      }],
      total: 0
    }
  },
  created() {
    this.getUserList()
  },
  methods: {
    getUserList(){

    },
    indexMethod(index) {
      return index * 2;
    }
  }

}
</script>

<style scoped>
@import "../assets/global.css";
</style>
