<template>
 <h1>这是首页</h1>

</template>

<script>
// @ is an alias to /src


export default {
  name: 'Home',
  

  data(){
    return {
      msg:"hello ,Dylan1",
      dialogFormVisible:false,
      multipleSelection:[],
      collapseBtnClass:'el-icon-s-fold',
      isCollapse:false,
      sideWidth: 200,
      logoTextShow:true
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

<!--<style>-->
<!--.headerBg{-->
<!--  background:#eee !important;-->
<!--}-->
<!--</style>-->
