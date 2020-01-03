<template>
    <div>
        <el-button round type="success" size="small" @click="toAddHandler">添加</el-button>
        <el-button round type="danger" size="small">批量删除</el-button>
        <el-table :data="category">
            <el-table-column type="selection"> </el-table-column>
            <el-table-column prop="id" label="编号"> </el-table-column>
            <el-table-column prop="name" label="栏目名称"></el-table-column>
            <el-table-column prop="price" label="序号"></el-table-column>
            <el-table-column prop="discriotion" label="父栏目"></el-table-column>
            <el-table-column label="操作">
                 <template v-slot="slot">
              <a href="" @click.prevent="toDeleteHandler(slot.row.id)"><i class="el-icon-delete"></i></a>
              <a href="" @click.prevent="toUpdateHandler"><i class="el-icon-edit"></i></a>
              <a href="" @click.prevent="toDetailHandler">详情</a>
        </template>
      </el-table-column>
        </el-table>
        <el-dialog
      title="添加产品信息"
      :visible.sync="visible"
      width="60%">
      <el-form :model="form" label-width="80px">
        <el-form-item label="名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="价格">
          <el-input v-model="form.price"></el-input>
        </el-form-item>
           <el-form-item label="所属栏目">
          <el-input v-model="form.realname"></el-input>
        </el-form-item>
          <el-form-item label="介绍">
          <el-input v-model="form.introduct"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button round size="small" @click="closeModalHandler">取 消</el-button>
        <el-button round size="small" type="primary" @click="submitHandler">确 定</el-button>
      </span>
    </el-dialog>
    </div>
</template>
<script>
import request from'@/utils/request'
import querystring from 'querystring'
export default {
    methods:{
        handleCheckAllChange(val) {
        this.isIndeterminate = false;
      },

         loadData(){
 let url="http://localhost:6677/category/findAll"
    request.get(url).then((response)=>{
      //将查询结果设置到product中
      this.category=response.data;
    })
    },
    submitHandler(){
      //this.form对象---字符串-->后台
      //通过request与后台进行交互,需携带参数
      let url="http://localhost:6677/category/saveOrUpdate"
      request({
        url,
        method:"POST",
        headers:{
          "Content-Type":"application/x-www-form-urlencoded"
        },
        data:querystring.stringify(this.form)

      }).then((response)=>{
        this.closeModalHandler();
        // 模态框关闭
        //刷新
        this.loadData();
        //提示消息
        this.$message({
          type:"success",
          message:response.message
        })
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
    toUpdateHandler(){
      this.visible = true;
    },
    closeModalHandler(){
      this.visible = false;
    },
    toAddHandler(){
      this.visible = true;
    }
  },
    data(){
    return {
  

      visible:false,
      category:[],
      form:{
        type:"category"
      }
    }
  },
  created(){
    //this为当前实例
    //vue实例创建完毕
   this.loadData();
  }
}
</script>
<style scoped>
 
</style>