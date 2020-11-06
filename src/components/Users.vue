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
              <template slot-scope="scope">
                <!--修改按钮-->
                <el-tooltip class="item" effect="light" content="修改用户" placement="top" :enterable="false">
                  <el-button type="primary" icon="el-icon-edit" size="mini" @click="openEditDialog(scope.row.id)"></el-button>
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
      <el-dialog title="添加用户" :visible.sync="addDialogVisible" width="50%" @close="handleClose">
        <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="70px">
          <el-form-item label="登陆名" prop="username" required>
            <el-input type="text" v-model="ruleForm.username" auto-complete="off" placeholder="请输入登陆账号"></el-input>
          </el-form-item>
          <el-form-item label="用户名" prop="name" required>
            <el-input type="text" v-model="ruleForm.name" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password" required>
            <el-input type="password" v-model="ruleForm.password" auto-complete="off" placeholder="密码应为5-20个以字母开头、可带数字、“_”、“.”的字串"></el-input>
          </el-form-item>
          <el-form-item label="性别" prop="sex" required>
            <el-radio-group v-model="ruleForm.sex">
              <el-radio label="男"></el-radio>
              <el-radio label="女"></el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="邮箱" prop="email" required>
            <el-input type="email" v-model="ruleForm.email" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="手机" prop="phone" required>
            <el-input type="text" v-model="ruleForm.phone" auto-complete="off"></el-input>
          </el-form-item>
          <el-form-item label="生日" required>
            <el-form-item prop="birthday">
              <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.birthday" style="width: 100%;"></el-date-picker>
            </el-form-item>
          </el-form-item>

        </el-form>
        <span slot="footer" class="dialog-footer">
          <el-button @click="addDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="submitForm('ruleForm')">确 定</el-button>
        </span>
      </el-dialog>

      <el-dialog title="修改用户信息" :visible.sync="editDialogVisible" width="70%">
        <span>这是一段信息</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="editDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="editDialogVisible = false">确 定</el-button>
        </span>
      </el-dialog>
    </div>
</template>

<script>
export default {
  data() {
    // 校验邮箱
    var checkEmail = (rule, value, callback) => {
      const regEmail = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/
      if(regEmail.test(value)){
        return callback()
      }
      callback(new Error('请输入合法的邮箱'))
    }
    // 校验密码 （5-20个以字母开头、可带数字、“_”、“.”的字串）
    var checkPassword = (rule, value, callback) => {
      if(value.length > 20 || value.length < 6){
          return callback(new Error('密码长度不合法（长度应为5-20）'))
      }
      const regPassword = /^[a-zA-Z]{1}([a-zA-Z0-9]|[._]){4,19}$/
      if(regPassword.test(value)){
        return callback()
      }
      callback(new Error('请输入严谨的密码（5-20个以字母开头、可带数字、“_”、“.”的字串）'))
    }
    // 校验手机号
    var checkPhone = (rule, value ,callback) => {
      const regPhone = /^[1][3,4,5,7,8,9][0-9]{9}$/;
      if(regPhone.test(value)){
        return callback()
      }
      callback(new Error('请输入合法的手机号'))
    }
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
      addDialogVisible: false,
      editDialogVisible: false,
      // 表单数据
      ruleForm: {
        username: '',
        name: '',
        password: '',
        email: '',
        birthday: '',
        sex: '',
        phone: ''
      },
      // 表单校验规则
      rules: {
        username: [
          {required: true, message: '请输入登陆名', trigger: 'blur'},
          {min: 3, max: 20, message: '用户名的长度在3 ~ 20个字符之间'}
        ],
        name: [
          {required: true, message: '请输入用户名', trigger: 'blur'},
          {min: 3, max: 20, message: '用户名的长度在3 ~ 20个字符之间'}
        ],
        password: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {validator: checkPassword, trigger: 'blur'}
        ],
        email: [
          {required: true, message: '请输入邮箱', trigger: 'blur'},
          {validator: checkEmail, trigger: 'blur'}
        ],
        phone: [
          {required: true, message: '请输入手机号', trigger: 'blur'},
          {validator: checkPhone, trigger: 'blur'}
        ],
        sex: [
          {required: true, message: '请选择性别', trigger: 'blur'}
        ],
        birthday: [
          {required: true, message: '请选择生日', trigger: 'blur'}
        ]
      }
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
    },
    // 对话框关闭前
    handleClose(){
      this.$refs.ruleForm.resetFields()
    },
    // 提交表单
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // 网路请求
          /*const {data: res} =  this.$http.post('user', this.ruleForm)
          if(res.meta.status != 201){
            this.$message.error("添加用户失败");
          }*/
          this.$message.success("添加用户成功");
          // 隐藏对话框
          this.addDialogVisible = false
          // 重新获取用户列表
          this.getUserList()
        } else {
          console.log('校验信息未通过')
          return false;
        }
      });
    },
    // 编辑用户信息
    openEditDialog(id){
      this.editDialogVisible = true;
      
    }
  }
}
</script>

<style scoped>
@import "../assets/global.css";
</style>
