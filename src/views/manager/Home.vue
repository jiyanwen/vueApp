<template>
  <div class="home">
    <!-- 头部图片 -->
    <header class="header">
      <img src="../../assets/home.jpg" alt="">
    </header>
    <!--内容区域-->
    <div>
      <!-- 分类6个 -->
    <van-grid :column-num="3" >
      <van-grid-item
        v-for="value in categories"
        :key="value.id"
        :icon="value.icon"
        :text="value.name"
      />
    </van-grid>
    <!-- 产品n个 -->
     <briup-product-item
     @click="toBuyHandler(p)"
        v-for="p in products"
        :key="p.id"
        :data="p">
     </briup-product-item>
    <!--产品-->
    </div>
  </div>
</template>
<script>

import {get, post} from '../../http/axios';
export default {
  data(){
    return {
      categories:[],
      products:[]
    }
  },
  created(){
    this.loadCategories();
    //查询产品
    this.loadProducts();
  },
  methods:{
    toBuyHandler(p){
      //跳转到订单确认页面并携带数据p
      this.$router.push({
        path:"/manager/order_confirm",
        query:p
      })
    },
    loadCategories(){
      let url = "/category/findAll";
      get(url).then((response)=>{
        //将查询结果，数组中的前6个元素获取到
        this.categories=response.data.slice(0,6);
      })
    },
    loadProducts(){
      let url="/product/query"
      let params = {
        page:0,
        pageSize:10
      }
      post(url,params).then((response)=>{
        this.products = response.data.list;
      })
    }
  }
}
</script>
<style scoped>
.home {
  padding-bottom: 50px;
}
.header {
  height: 300px;
  overflow: hidden;
}
.header img {
  width: 100%;
}
</style>