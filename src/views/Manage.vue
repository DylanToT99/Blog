<template>
 <div>
   <el-container style="height: 100%; min-height: 100vh">
     <el-aside :width="sideWidth+'px'" style="background-color: rgb(238, 241, 246); height: 100vh;
                overflow-x: hidden;box-shadow: 2px 0 6px rgb(0,21,41) ">
      <Aside :is-collapse="isCollapse" :logoTextShow="logoTextShow">
      </Aside>
     </el-aside>

     <el-container>
       <el-header  style="border-bottom: 1px solid #ccc;">
        <Header :collapseBtnClass="collapseBtnClass" :collapse="collapse"></Header>
       </el-header>



       <el-main>
<!--         表示当前页面的子路由会在此展示-->
        <router-view></router-view>
       </el-main>
     </el-container>
   </el-container>
 </div>

</template>

<script>
// @ is an alias to /src
import Aside from "@/components/Aside";
import User from "@/views/User";

import request from "@/utils/request";
import Header from "@/components/Header";

export default {
  name: 'Home',


  data(){
    return {
      msg:"hello ,Dylan1",
      collapseBtnClass:'el-icon-s-fold',
      isCollapse:false,
      sideWidth: 200,
      logoTextShow:true
    }
  },
  created(){
    //请求分页查询数据

  },
  components: {
    Header,
    Aside,
    User
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
    },



  }
}
</script>

