<template>
  <div class="pos">
    <el-row class="order1">
        <el-col :span="7" class="pos-order" id="order-list">
            <el-tabs>
                <el-tab-pane label="点餐">
                    <el-table :data="tableData" border style="width:100%">
                        <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                        <el-table-column prop="count" label="数量"></el-table-column>
                        <el-table-column prop="price" label="金额"></el-table-column>
                        <el-table-column label="操作" fixed='right'>
                        <template slot-scope="scope">
                            <el-button type='text' size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                            <el-button type='text' size="small" @click="addOrderList(scope.row)">增加</el-button>
                        </template>   
                        </el-table-column>
                       
                    </el-table>

                    <div class="totalDiv">
                        <small>数量：</small>{{totalCount}}   &nbsp;&nbsp;&nbsp; <small>金额：</small> {{totalPrice}}元
                    </div>

                    <div class="but">
                        <el-button type="">挂单</el-button>
                        <el-button type="" @click="delAllGoods()">清空</el-button>
                        <el-button type="">结账</el-button>
                    </div>
                </el-tab-pane>
                <el-tab-pane label="挂单"></el-tab-pane>
                <el-tab-pane label="外卖"></el-tab-pane>
            </el-tabs>
        </el-col>
        <el-col :span="17">
            <div class="often-goods">
                <div class="title">常用商品</div>
                <div class="often-goods-list">
                    <ul>
                        <li v-for="goods in oftenGoods" :key="goods.id" @click="addOrderList(goods)">
                            <span>{{goods.goodsName}}</span>
                            <span class="o-price">{{goods.price}}元</span>
                        </li>
                    </ul>
                </div>
            </div>
            
            <div class="goods-type">
                <el-tabs>
                    <el-tab-pane label="汉堡">
                        <div>
                            <ul class="cookList">
                                <li v-for="goods in type0Goods" :key="goods.id" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" alt="图片" class="foodsImg"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                
                    <el-tab-pane label="小吃">
                        <div>
                            <ul class="cookList">
                                <li v-for="goods in type1Goods" :key="goods.id" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" alt="图片" class="foodsImg"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                
                    <el-tab-pane label="饮料">
                        <div>
                            <ul class="cookList">
                                <li v-for="goods in type2Goods" :key="goods.id" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" alt="图片" class="foodsImg"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                
                    <el-tab-pane label="套餐">
                        <div>
                            <ul class="cookList">
                                <li v-for="goods in type3Goods" :key="goods.id" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" alt="图片" class="foodsImg"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </div>
                    </el-tab-pane>
                </el-tabs>
            </div>

        </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';

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
        }
        
    },
    created:function(){
        axios.get('https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/oftenGoods')
        .then(response=>{
            this.oftenGoods=response.data.oftenGoods;
        })
        .catch(error=>{
            alert('网络错误，请联系管理员');
        });
        axios.get('https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/typeGoods')
        .then(response=>{
            this.type0Goods=response.data.data[0];
            this.type1Goods=response.data.data[1];
            this.type2Goods=response.data.data[2];
            this.type3Goods=response.data.data[3];
        })
        .catch(error=>{
            alert('网络错误，请联系管理员');
        })

    },
    mounted:function(){
        var orderHeight=document.body.clientHeight;
        document.getElementById('order-list').style.height=orderHeight + 'px';
    },
    computed: {
        //计算商品总数量
      totalCount() {
        return this.tableData.reduce((pre, goods) => {
          return pre + goods.count
          } , 0)
      },
        //计算商品总价格
      totalPrice() {
        return this.tableData.reduce((pre, goods) => {
          return pre + goods.count * goods.price;
        }, 0)
      }
    },
    methods:{
        addOrderList(goods){


            //判断商品是否已经存在订单列表中
            let isHave=false;
            for(let i=0;i<this.tableData.length;i++){
                if(this.tableData[i].goodsId==goods.goodsId){
                    isHave=true;
                }
            }

            //根据判断的值编写业务逻辑
            if(isHave){
                //改变列表中商品的数量
                let arr = this.tableData.filter(o=>o.goodsId==goods.goodsId);
                arr[0].count++;
            }else{
                let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
                this.tableData.push(newGoods)
            }
        },
        delSingleGoods(goods){
            this.tableData=this.tableData.filter(o=>o.goodsId !=goods.goodsId)
        },
        delAllGoods(){
            this.tableData=[];
        }
    }
}
</script>

<style scoped>
.pos-order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
}
.order1{
    height: 100%;
}
.but{
    display: flex;
    justify-content: center;
}
.title{
    height: 20px;
    border-bottom: 1px solid #D3dce6;
    background-color: #F9fafc;
    padding: 10px;
    text-align: center;
}
.often-goods-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #F9FAFC;
    padding: 10px;
    margin: 10px;
    background-color: #fff;
    cursor: pointer;
}
.o-price{
    color: #58B7FF;
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
       cursor: pointer;

   }
   .cookList li span{

        display: block;
        float:left;
   }
   .foodImg{
       width: 100%;
       height: auto;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       margin-left: 10px;
   }
   .foodsImg{
       width: auto;
       height: 200px;
   }
   .totalDiv{
       background-color: #fff;
       padding: 10px;
       border-bottom: 1px solid #D3dce6;
   }
</style>