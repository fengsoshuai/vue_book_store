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
              <el-input placeholder="请输入用户名" v-model="queryInfo.query" clearable @clear="getUserList">
                <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
              </el-input></el-col>
            <el-col :span="4">
              <el-button type="primary" @click="addDialogVisible = true">添加</el-button>
            </el-col>
          </el-row>

          <!--用户列表-->
          <el-table :data="userList" style="width: 100%" border stripe>
            <el-table-column type="index" :index="indexMethod" label="#"></el-table-column>
            <el-table-column prop="name" label="姓名" width="180"></el-table-column>
            <el-table-column prop="date" label="注册日期" width="180"></el-table-column>
            <el-table-column label="状态" width="100">
              <template slot-scope="scope">
                <el-switch v-model="scope.row.status" active-color="#13ce66" inactive-color="#ff4949" @change="userStatuChange(scope.row)"></el-switch>
              </template>
            </el-table-column>
            <el-table-column prop="role" label="角色"></el-table-column>
            <el-table-column prop="address" label="地址"></el-table-column>
            <el-table-column label="操作" width="180">
              <template>
                <!--修改按钮-->
                <el-tooltip class="item" effect="light" content="修改用户" placement="top" :enterable="false">
                  <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
                </el-tooltip>
                <!--删除按钮-->
                <el-tooltip class="item" effect="light" content="删除用户" placement="top" :enterable="false">
                  <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
                </el-tooltip>
                <!--分配角色按钮-->
                <el-tooltip class="item" effect="light" content="分配角色" placement="top" :enterable="false">
                  <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
                </el-tooltip>
              </template>
            </el-table-column>
          </el-table>
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="queryInfo.pageNum"
            :page-sizes="[2, 4, 6, 8]"
            :page-size="queryInfo.pageSize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </div>
      </el-card>

      <!--添加用户对话框-->
      <el-dialog title="添加用户" :visible.sync="addDialogVisible" width="50%" :before-close="handleClose">
        <span>这是一段信息</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="addDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="addDialogVisible = false">确 定</el-button>
        </span>
      </el-dialog>
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
        id: '1001',
        date: '2016-05-02',
        name: '王小虎1',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1518 弄',
        zip: 200333,
        tag: '家',
        status: true,
        role: '超级管理员'
      }, {
        id: '1002',
        date: '2016-05-04',
        name: '王小虎2',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1517 弄',
        zip: 100333,
        tag: '公司',
        status: false,
        role: '超级管理员'
      }, {
        id: '1003',
        date: '2016-05-01',
        name: '王小虎3',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1519 弄',
        zip: 400333,
        tag: '家',
        status: true,
        role: '超级管理员'
      }, {
        id: '1004',
        date: '2016-05-03',
        name: '王小虎4',
        province: '上海',
        city: '普陀区',
        address: '上海市普陀区金沙江路 1516 弄',
        zip: 700333,
        tag: '公司',
        status: true,
        role: '超级管理员'
      }],
      total: 4,
      addDialogVisible: false
    }
  },
  created() {
    this.getUserList();
  },
  methods: {
    // 获取用户列表
    getUserList(){
    },
    indexMethod(index) {
      return index;
    },
    // 监听 pageSize 改变的事件
    handleSizeChange(newSize){
      this.queryInfo.pageSize = newSize;
      this.getUserList();
    },
    // 监听页码值改变的事件
    handleCurrentChange(newPage){
      this.queryInfo.pageNum = newPage;
      this.getUserList();
    },
    // 更改用户状态
    async userStatuChange(userInfo){
      return this.$message.success("更新用户状态成功");
    }
  }
}
</script>

<style scoped>
@import "../assets/global.css";
</style>
