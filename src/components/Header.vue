<template>
  <div style="font-size: 12px;line-height: 60px;display: flex ">

  <div style="flex: 1;">
     <span :class="collapseBtnClass" style="cursor: pointer;font-size: 15px " @click="collapse">
     </span>
    <el-breadcrumb separator="/" style="display: inline-block; margin-left: 10px">
      <el-breadcrumb-item :to="{ path: '/' }" >首页</el-breadcrumb-item>
      <el-breadcrumb-item >{{ currentPathName }}</el-breadcrumb-item>
    </el-breadcrumb>
  </div>

  <el-dropdown style="width: 70px; cursor: pointer">
    <span >王小虎</span><i class="el-icon-arrow-down" style="margin-left: 5px"></i>
    <el-dropdown-menu slot="dropdown" style="width: 100px; text-align: center">
      <el-dropdown-item style="font-size: 14px;padding: 5px 0">个人信息</el-dropdown-item>
      <el-dropdown-item style="font-size: 14px;padding: 5px 0">退出</el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
  </div>
</template>

<script>
export default {
  name: "Header",
  props:{
    collapseBtnClass:String
    
  },
  computed:{
    currentPathName(){
      return this.$store.state.currentPathName;
    }
  },
  watch:{
    currentPathName(newVal,oldVal){
      console.log(newVal)
    }
  },

  data(){
    return{
      //此处的组件有问题,需要之后改变这个按钮
      isCollapse:false
    }
  },
  methods:{
    collapse(){
      this.isCollapse=!this.isCollapse
      if(this.isCollapse){
        this.sideWidth=64
        this.collapseBtnClass='el-icon-s-unfold'
        this.logoTextShow=false
      }else{
        this.sideWidth=200
        this.collapseBtnClass='el-icon-s-fold'
        this.logoTextShow=true
      }
    }

  }
  

}
</script>

<style scoped>

</style>