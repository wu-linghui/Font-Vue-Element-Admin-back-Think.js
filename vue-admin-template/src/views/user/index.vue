<template>
  <div class="app-container">
      <div style="padding-bottom:20px;">
          <el-button @click="add" type="primary" icon="el-icon-edit">添加用户</el-button>
      </div>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>
      <el-table-column label="用户名" width="150" align="center">
        <template slot-scope="scope">
          {{ scope.row.username }}
        </template>
      </el-table-column>
      <el-table-column label="邮箱"  align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.email }}</span>
        </template>
      </el-table-column>
      <el-table-column label="手机" width="110" align="center">
        <template slot-scope="scope">
          {{ scope.row.mobile }}
        </template>
      </el-table-column>
      <el-table-column class-name="status-col" label="状态" width="110" align="center">
        <template slot-scope="scope">
          <el-tag :type="scope.row.status | statusFilter">{{ scope.row.status==1?"正常":"禁用" }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="操作" width="200">
        <template slot-scope="scope">
          <el-button @click="edit(scope.row.id)" type="primary" icon="el-icon-edit" circle></el-button>
          <el-button @click="del(scope.row.id)" type="danger" icon="el-icon-delete" circle></el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="height:100px;display:flex;justify-content: center;align-items:center;">
    <el-pagination
    background
    layout="prev, pager, next"
    :total="total" @current-change="getPageUsers">
    </el-pagination>
    </div>
  </div>
</template>

<script>
import { userlist,deluser} from '@/api/admin'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        '1': 'success',
        '0': 'gray',
        '-1': 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true,
      total:0,
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
      add(){
          this.$router.push({
           path:"/user/adduser",
       })
      },
    fetchData() {
      this.listLoading = true
      userlist().then(response => {

        this.list = response.userlist;
        this.total = response.total;
        console.log(response)
        this.listLoading = false
      })
    },
    getPageUsers(page){
        console.log(page);
        userlist({page:page}).then(response => {

        this.list = response.userlist;
        this.total = response.total;
        console.log(response)
        this.listLoading = false
      })
    },
   edit(id){
       this.$router.push({
           path:"/user/edituser",
           query:{id}
       })
    },
    async del(id){
        this.$confirm(`是否需要删除${id}的用户？`, '确认删除', {
          distinguishCancelAndClose: true,
          confirmButtonText: '删除',
          cancelButtonText: '取消'
        })
          .then(async () => {
            this.listLoading = true;
            let res = await deluser({id})
            this.listLoading = false;
            this.fetchData()
          })
          
      
        
    }
  }
}
</script>
