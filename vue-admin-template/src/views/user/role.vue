<template>
  <div class="app-container">
      <div style="padding-bottom:20px;">
          <el-button @click="add" type="primary" icon="el-icon-edit">添加角色</el-button>
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
      <el-table-column label="角色名称" width="150" align="center">
        <template slot-scope="scope">
          {{ scope.row.desc }}
        </template>
      </el-table-column>
      <el-table-column label="权限规则编号"  align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.rule_ids }}</span>
        </template>
      </el-table-column>
      
      <el-table-column class-name="status-col" label="状态" width="110" align="center">
        <template slot-scope="scope">
          <el-tag :type="scope.row.status | statusFilter">{{ scope.row.status==1?"正常":"禁用" }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="操作" width="200">
        <template slot-scope="scope">
          <el-button @click="edit(scope.row)" type="primary" icon="el-icon-edit" circle></el-button>
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
import { rolelist,delrole} from '@/api/admin'

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
           path:"/user/addrole",
       })
      },
    fetchData() {
      this.listLoading = true
      rolelist().then(response => {

        this.list = response.rolelist;
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
   edit(role){
       this.$router.push({
           path:"/user/editrole",
           query:role
       })
    },
    async del(id){
        this.$confirm(`是否需要删除${id}的角色？`, '确认删除', {
          distinguishCancelAndClose: true,
          confirmButtonText: '删除',
          cancelButtonText: '取消'
        })
          .then(async () => {
            this.listLoading = true;
            let res = await delrole({id})
            this.listLoading = false;
            this.fetchData()
          })
          
      
        
    }
  }
}
</script>
