<template>
    <div class="pos">
        <el-row>
            <el-col :span='7' class="pos-order" id="order-list">
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" style="width:100%;" border>
                            <el-table-column prop="goodsName" label="商品"></el-table-column>
                            <el-table-column prop="count" label="数量" width="50"></el-table-column>
                            <el-table-column prop="price" label="金额" width="70"></el-table-column>
                            <el-table-column label="操作" width="100" fixed="right">
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="delSingelGood(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="totalDiv">
                            <small>总计:</small>{{totalCount}}件&nbsp;&nbsp;<small>金额：</small>{{totalMoney}}元
                        </div>
                        <div class="div-btn">
                            <el-button type="warning">挂单</el-button>
                            <el-button type="danger" @click="delAllGoods">删除</el-button>
                            <el-button type="success" @click="checkout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span='17'>
                <!-- 常用商品开始 -->
               <div class="often-goods">
                    <div class="title">常用商品</div>
                    <div class="often-goods-list">
                        <ul>
                            <li v-for="goods in oftenGoods" :key="goods.id" @click="addOrderList(goods)">
                                <span>{{goods.goodsName}}</span>
                                <span class="o-price">￥{{goods.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </div>
                <!-- 常用商品结束 -->
                <!-- 商品分类开始 -->
                <div class="goods-type">
                    <el-tabs>
                        <el-tab-pane label="汉堡">
                            <ul class='cookList'>
                                <li v-for="foods in type0Goods" :key="foods.goodsId" @click="addOrderList(foods)">
                                    <span class="foodImg"><img :src="foods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{foods.goodsName}}</span>
                                    <span class="foodPrice">￥{{foods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="小食">
                            <ul class='cookList'>
                                <li v-for="foods in type1Goods" :key="foods.goodsId" @click="addOrderList(foods)">
                                    <span class="foodImg"><img :src="foods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{foods.goodsName}}</span>
                                    <span class="foodPrice">￥{{foods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                            <ul class='cookList'>
                                <li v-for="foods in type2Goods" :key="foods.goodsId" @click="addOrderList(foods)">
                                    <span class="foodImg"><img :src="foods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{foods.goodsName}}</span>
                                    <span class="foodPrice">￥{{foods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                            <ul class='cookList'>
                                <li v-for="foods in type3Goods" :key="foods.goodsId" @click="addOrderList(foods)">
                                    <span class="foodImg"><img :src="foods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{foods.goodsName}}</span>
                                    <span class="foodPrice">￥{{foods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                    </el-tabs>
                </div>
                <!-- 商品分类结束 -->
            </el-col>
        </el-row>
    </div>
</template>
<script>
import axios from 'axios'
export default {
    name:'pos',
    data(){
        return{
            tableData: [],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalCount:0,
            totalMoney:0,
        }
    },
    created:function(){
        axios.get("http://rap2.taobao.org:38080/app/mock/236577/oftenGoods")
            .then(res=>{
                //console.log(res)
                this.oftenGoods=res.data.data
            })
            .catch(error=>{
                alert('网络错误，不能访问');
            })

        axios.get("http://rap2.taobao.org:38080/app/mock/236577/type0Goods")
            .then(res=>{
                //console.log(res)
                this.type0Goods=res.data.data
            })
            .catch(error=>{
                alert('网络错误，不能访问');
            })
        axios.get("http://rap2.taobao.org:38080/app/mock/236577/type1Goods")
            .then(res=>{
                //console.log(res)
                this.type1Goods=res.data.data
            })
            .catch(error=>{
                alert('网络错误，不能访问');
            })
        axios.get("http://rap2.taobao.org:38080/app/mock/236577/type2Goods")
            .then(res=>{
            // console.log(res)
                this.type2Goods=res.data.data
            })
            .catch(error=>{
                alert('网络错误，不能访问');
            })
        axios.get("http://rap2.taobao.org:38080/app/mock/236577/type3Goods")
            .then(res=>{
                //console.log(res)
                this.type3Goods=res.data.data
            })
            .catch(error=>{
                alert('网络错误，不能访问');
            })
    },
    methods:{
        //选择商品
        addOrderList(goods){
            this.totalCount=0;
            this.totalMoney=0;
            let isHave=false;
            for(let i=0;i<this.tableData.length;i++){
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true
                }
            }
            if(isHave){
                 //存在就进行数量添加
                 let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId)
                 arr[0].count++;
                 console.log(arr);
            }else{
                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
                this.tableData.push(newGoods)

            }
            this.getTotal()
            
        },
        //删除单个商品
        delSingelGood(goods){
            this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId)
            this.getTotal()
        },
        //删除所有商品
        delAllGoods(){
            this.tableData=[];
            this.totalCount=0;
            this.totalMoney=0;
        },
        //模拟结账
        checkout(){
            if(this.totalCount!=0){
                this.tableData=[];
                this.totalCount=0;
                this.totalMoney=0;
                this.$message({
                    message:'结账成功！',
                    type:'success'
                })
            }else{
                this.$message.error('不能空结')
            }
        },
        //汇总数量和金额
        getTotal(){
            this.totalCount=0;
            this.totalMoney=0;
            if(this.tableData){
                this.tableData.forEach(element=>{
                    this.totalCount+=element.count;
                    this.totalMoney=this.totalMoney+(element.count*element.price)
                })
            }
            
        }
     },
    mounted:function(){
        var orderHeight=document.body.clientHeight;
        document.getElementById('order-list').style.height=orderHeight+'px';
    }
}
</script>
<style scoped>
.pos-order{
    background: #f9fafc;
    border-right:1px solid #c0ccda;
}
.totalDiv{
    padding: 10px;
    border-bottom:1px solid #c0ccda;
}
.div-btn{
    margin-top:10px;
}
.title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
    text-align: left;
}
.often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding:10px;
    margin:5px;
    background-color:#fff;
}
.o-price{
    color:#58B7FF; 
}
.goods-type{
    clear: both;
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;

   }
   .cookList li span{

        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
</style>