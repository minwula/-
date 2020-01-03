<template>
    <div>
        <el-button round icon="el-icon-edit" size="small" type="success" @click="toAddHandler">添加</el-button>
        <el-button round icon="el-icon-delete" size="small" type="danger">批量删除</el-button>
        <el-table :data="employee">
            <el-table-column type="selection"> </el-table-column>
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="realname" label="真实姓名"></el-table-column>
            <el-table-column prop="gender" label="性别"></el-table-column>
            <el-table-column prop="telephone" label="联系方式"></el-table-column>
            <el-table-column prop="idCard" label="身份证号"></el-table-column>
            <el-table-column prop="bankCard" label="银行卡号"></el-table-column>
            <el-table-column label="操作">
                <template v-slot="slot">
                    <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                    <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
                </template>
            </el-table-column>
        </el-table>
<el-pagination background layout="prev, pager, next"  :total="100"></el-pagination>
        <el-dialog
      title="添加员工信息"
      :visible.sync="visible"
      width="60%">
      <el-form :model="form" label-width="80px">
          <el-form-item label="用户名">
          <el-input v-model="form.username"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input type="password" v-model="form.password"></el-input>
        </el-form-item>
           <el-form-item label="姓名">
          <el-input v-model="form.realname"></el-input>
        </el-form-item>
           <el-form-item label="性别">
            <el-radio-group v-model="form.gender">
               <el-radio label="男">男</el-radio>
               <el-radio label="女">女</el-radio>
            </el-radio-group>
           </el-form-item>
        <el-form-item label="联系方式">
          <el-input v-model="form.telephone"></el-input>
        </el-form-item>
          <el-form-item label="身份证号">
          <el-input v-model="form.idCard"></el-input>
        </el-form-item>
          <el-form-item label="银行卡号">
          <el-input v-model="form.bankCard"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button size="small" @click="closeModalHandler">取 消</el-button>
        <el-button size="small" type="primary" @click="closeModalHandler">确 定</el-button>
      </span>
    </el-dialog>
    </div>
</template>
<script>
import request from'@/utils/request'
import querystring from 'querystring'
    export default{
        methods:{
             loadData(){
 let url="http://localhost:6677/waiter/findAll"
    request.get(url).then((response)=>{
      //将查询结果设置到employee中
      this.employee=response.data;
    })
    },
             toDeleteHandler(id){
      this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$message({
          type: 'success',
          message: '删除成功!'
        });
      })
      
    },
            toAddHandler(){
                this.visible=true;
            },
            closeModalHandler(){
                this.visible=false;
            },
            toUpdateHandler(row){
                this.title="修改员工信息";
                this.visible=true;
            }
        },
    data(){
        return{
            visible:false,
            employee:[],
            form:{
                type:"employee"
            }
        }
    },
    created(){
        this.loadData();
    }

}
</script>
<style scoped>

</style>