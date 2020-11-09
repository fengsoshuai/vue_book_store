<template>
    <div id="rights">
       <!--面包屑导航-->
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
        <el-breadcrumb-item>权限列表</el-breadcrumb-item>
      </el-breadcrumb>

      <el-card>
          <div style="margin-top: 15px;">
            <!--权限列表-->
          <el-table :data="rightsList" style="width: 100%" border stripe>
            <el-table-column type="index" :index="indexMethod" label="#"></el-table-column>
            <el-table-column prop="name" label="权限名" width="180"></el-table-column>
            <el-table-column prop="path" label="路径" width="180"></el-table-column>
            <el-table-column prop="level" label="权限等级">
                <template slot-scope="scope">
                    <el-tag type="danger" v-if="scope.row.level == 1">1级</el-tag>
                    <el-tag type="warning" v-if="scope.row.level == 2">2级</el-tag>
                    <el-tag type="info" v-if="scope.row.level == 3">3级</el-tag>
                    <el-tag type="success" v-if="scope.row.level == 4">4级</el-tag>
                    <el-tag v-if="scope.row.level == 5">5级</el-tag>
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
    </div>
</template>

<script>
export default {
    data(){
        return{
            rightsList: [
                {
                    id: '1001',
                    name: '用户列表',
                    path: 'users',
                    level: 2
                }, 
                {
                    id: '1002',
                    name: '权限列表',
                    path: 'rights',
                    level: 1
                }
            ],
            queryInfo: {
                query: '',
                pageNum: 1,
                pageSize: 2
            },
            total: 4

        }
    },
    created() {
        this.getRightsList();
    },
    methods: {
        async getRightsList(){
            // const {data: res} = await this.$http.get('rights/list');
            // if(res.meta.status !== 200){
            //     return this.$message.error('获取权限列表失败！');
            // }
            // this.rightsList = res.data;
        },
        indexMethod(index){
            return index;
        },
        // 监听 pageSize 改变的事件
        handleSizeChange(newSize){
            this.queryInfo.pageSize = newSize;
            this.getRightsList();
        },
        handleCurrentChange(){},

    }
}
</script>
<style lang="less" scoped>

</style>