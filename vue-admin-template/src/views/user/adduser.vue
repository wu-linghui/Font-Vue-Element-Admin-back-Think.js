<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="用户名">
        <el-input v-model="form.username"  />
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="form.password"  />
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="form.email" />
      </el-form-item>
      <el-form-item label="电话号码">
        <el-input v-model="form.mobile" />
      </el-form-item>
      <el-form-item label="管理员角色选择">
        <el-select v-model="form.role_id" placeholder="please select your zone">
          <el-option v-for="(item,index) in rolelist" :key="index" :label="item.desc" :value="item.id" />
        </el-select>
      </el-form-item>
     
      <el-form-item>
        <el-button type="primary" @click="onSubmit">添加</el-button>
        <el-button @click="onCancel">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import {adduser,userinfo,rolelist,updateuser} from '@/api/admin'
export default {
  data() {
    return {
      form: {
        username:"",
        password:'',
        email:"",
        mobile:"",
        role_id:1
      },
      role_id:"",
      rolelist:[]
    }
  },
  async created(){
    

  },
  methods: {
    onSubmit() {
      this.$message('submit!')
      adduser(this.form)
      this.$router.push('/user/userlist');
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  },
  async beforeMount(){
      let res = await rolelist()
      this.rolelist = res.rolelist;
  }
}
</script>

<style scoped>
.line{
  text-align: center;
}
</style>

