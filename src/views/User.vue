<template>
<div>

  <div class="pd-10" >
    <el-input style="width: 200px" suffix-icon="el-icon-search" placeholder="请输入名称" v-model="username">
    </el-input>
    <el-input style="width: 200px" suffix-icon="el-icon-message" placeholder="请输入邮箱" class="ml-5" v-model="email">
    </el-input>
    <el-input style="width: 200px" suffix-icon="el-icon-office-building" placeholder="请输入地址" class="ml-5" v-model="address">
    </el-input>
    <el-button class="ml-5" type="primary" @click="loadData">
      搜索
    </el-button>
    <el-button class="ml-5" type="warning" @click="reset">
      重置
    </el-button>

    <div style="margin: 10px 0" >
      <el-button type="primary" @click="handleAdd">新增<i class="el-icon-circle-plus-outline"></i></el-button>
      <el-popconfirm
          class="ml-5"
          confirm-button-text='确认删除'
          cancel-button-text='不用了'
          icon="el-icon-info"
          icon-color="red"
          title="您确定删除这些数用户吗？"
          @confirm="delBatch"
      >
        <el-button type="danger" slot="reference">批量删除<i class="el-icon-remove-outline"></i></el-button>
      </el-popconfirm>
      <el-upload  action="http://localhost:9090/user/import" style="display: inline-block" :show-file-list="false"
      accept=".xlsx" :on-success="importSuccess">
      <el-button type="primary" class="ml-5">导入<i class="el-icon-bottom-left"></i></el-button>
      </el-upload>
      <el-button type="primary" @click="exp" class="ml-5" >导出<i class="el-icon-bottom-right"></i></el-button>
    </div>
  </div>

  <el-table :data="tableData" border stripe  @selection-change="handleSelectionChange">>
    <el-table-column
        type="selection"
        width="55">
    </el-table-column>
    <el-table-column prop="id" label="id" width="140">
    </el-table-column>
    <el-table-column prop="username" label="用户名" width="140">
    </el-table-column>
    <el-table-column prop="nickname" label="别名" width="120">
    </el-table-column>
    <el-table-column prop="address" label="地址">
    </el-table-column>
    <el-table-column prop="email" label="邮箱">
    </el-table-column>
    <el-table-column prop="phone" label="电话">
    </el-table-column>
    <el-table-column label="操作">
      <template v-slot="scope" >
        <el-button type="success" @click="handleEdit(scope.row)" >编辑<i class="el-icon-edit"></i></el-button>
        <el-popconfirm
            class="ml-5"
            confirm-button-text='确认删除'
            cancel-button-text='不用了'
            icon="el-icon-info"
            icon-color="red"
            title="您确定删除吗？"
            @confirm="handleDel(scope.row.id)"
        >
          <el-button type="danger" slot="reference">删除<i class="el-icon-delete-solid"></i></el-button>
        </el-popconfirm>
      </template>
    </el-table-column>
  </el-table>

  <div style="padding: 10px 0" >
    <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageNum"
        :page-sizes="[2, 5, 10, 20]"
        :page-size="pageSize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
    </el-pagination>
  </div>
  <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%">
    <el-form label-width="100px">
      <el-form-item label="用户名" >
        <el-input v-model="form.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="昵称" >
        <el-input v-model="form.nickname" autocomplete="off"></el-input>
      </el-form-item>

      <el-form-item label="地址" >
        <el-input v-model="form.address" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮箱" >
        <el-input v-model="form.email" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="电话" >
        <el-input v-model="form.phone" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="save">确 定</el-button>
    </div>
  </el-dialog>
</div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "User",
  data(){
    return{
      tableData: [],
      total:0,
      pageNum:1,
      pageSize:5,
      username:'',
      form:{},
      email:'',
      address:'',
      dialogFormVisible:false,
      multipleSelection:[],
      headerBg:'headerBg'
    }
  },
  created() {
    this.loadData()
  },
  methods:{
    loadData(){
      request.get("/user/page",{
        params:{
          pageNum: this.pageNum,
          pageSize:this.pageSize,
          username:this.username,
          email:this.email,
          address:this.address
        }
      })
          .then(res=>{
            console.log(res)
            this.tableData=res.records
            this.total=res.total
          })
      // fetch("http://localhost:9090/user/page?pageNum="+this.pageNum+"&pageSize="+this.pageSize+"&username="+this.username+"&email="+this.email+"&address="+this.address).then(res=>res.json()).then(res=>{
      //   console.log(res)
      //   this.tableData=res.data
      //   this.total=res.total
      // })
    },
    handleSizeChange(val){
      this.pageSize=val
      this.loadData()
    },
    handleCurrentChange(val){
      this.pageNum=val
      this.loadData()
    },
    reset(){
      this.username=''
      this.address=''
      this.email=''
      this.loadData()
    },
    handleAdd(){
      this.dialogFormVisible =true
    },
    save(){
      request.post("/user",this.form).then(res=>{
        if(res){
          this.$message.success("保存成功!")
          this.dialogFormVisible=false
          this.loadData()
        }else{
          this.$message.error("保存失败")
        }
      })
    },
    handleEdit(row){
      this.form=row
      this.dialogFormVisible=true
    },
    handleDel(id){
      request.delete("/user/"+id).then(res=>{
        if(res){
          this.$message.success("删除成功!")
          this.loadData()
        }else{
          this.$message.error("删除失败!")
        }
      })
    },
    delBatch(){
      let ids=this.multipleSelection.map(v=>v.id)
      request.post("/user/delBatch",ids).then(res=> {
        if (res) {
          this.$message.success("批量删除成功!")
          this.loadData()
        } else {
          this.$message.error("删除失败!")
        }
      })

    },
    handleSelectionChange(val){
      this.multipleSelection=val
    },
    exp(){
        window.open("http://localhost:9090/user/export")
    },
    importSuccess(){
        this.$message.success("导入成功!")
        this.loadData()
    }
  
  }
}
</script>

<style>
.headerBg{
  background:#eee !important;
}
</style>
