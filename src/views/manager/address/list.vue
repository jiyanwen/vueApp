<template>
        <briup-fulllayout  title="常用地址">

        <van-list>
            <van-cell
                v-for="item in addresses"
                :key="item.id"
                :title="item.province+'  '+item.city+'  '
                +item.area+'  '+item.address"
            >
            <van-button type="danger" size="small" 
            @click="toDeleteAddress(item.id)" >删除</van-button>
            </van-cell>
        </van-list>


        <br>
        <van-button block type="default" @click="toAddressEditHandler" >
            添加
        </van-button>
        </briup-fulllayout>
</template>

<script>
import {mapState} from 'vuex' 
import {get} from '../../../http/axios'
export default {
    data(){
        return{
        addresses:[]
        }
    },
    computed:{//计算属性
        //将状态机中的user对象中的info对象获取到
       ...mapState("user",["info"])
    },
    created(){
        this.loadAddress();
    },

    methods:{
        
        toDeleteAddress(id){
            let url = "/address/deleteById?id="+id;
            get(url).then((response)=>{
                this.$toast.success("删除成功");
                //刷新数据
                this.loadAddress();
            })
            

        },
        loadAddress(){
            let id=this.info.id;
            let url="/address/findByCustomerId?id="+id;
            get(url).then((response)=>{
                this.addresses=response.data;
            })
        },
        //跳转到地址编辑的函数
        toAddressEditHandler(){
            this.$router.push("/manager/address_edit")
        }
    }
}
</script>