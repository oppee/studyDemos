<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" id="order-list">
          <div class="grid-content bg-purple">
            <template>
              <el-tabs v-model="activeName_auto">
                <el-tab-pane label="结账" name="first">
                  <template>
                    <el-table :data="tableData" border style="width: 100%">
                      <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                      <el-table-column prop="price" label="金额"></el-table-column>
                      <el-table-column prop="count" label="数量"></el-table-column>
                      <el-table-column fixed="right" label="操作">
                        <template slot-scope="scope">
                            <el-button  type="text" size="small" @click="deSingleGoods(scope.row)">删除</el-button>
                            <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                        </template>
                      </el-table-column>
                    </el-table>
                  </template>
                  <p style="text-align:center"><small>数量：</small>{{totalCount}}    <small>总额：</small>{{totalMoney}}</p>
                  <template>
                    <el-row class="tableBtn">
                      <el-button type="primary" round @click="checkout">结账</el-button>
                      <el-button type="success" round>挂单</el-button>
                      <el-button type="danger" round @click="delAllGoods">删除</el-button>
                    </el-row>
                  </template>
                </el-tab-pane>
                <el-tab-pane label="挂单" name="second">挂单</el-tab-pane>
                <el-tab-pane label="外卖" name="third">套餐</el-tab-pane>
              </el-tabs>
            </template>
          </div>
          </el-col>
        <el-col :span="17">
          <div class="grid-content bg-purple" style="padding:0 12px;">
            <div class="oftenGoods">
              <h3>高频率主食</h3>
              <ul>
                <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                  <span>{{goods.goodsName}}</span>
                  <span>{{goods.price}}</span>
                </li>
              </ul>
            </div>
            <hr>
            <template>
              <el-tabs v-model="activeName">
                <el-tab-pane label="主食" name="first">
                  <ul class='cookList'>
                      <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <span class="foodName">{{goods.goodsName}}</span><br>
                          <span class="foodPrice">{{goods.price}}</span>
                      </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="小食" name="second">
                  <ul class='cookList'>
                      <li v-for="goods in type1Goods">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <span class="foodName">{{goods.goodsName}}</span><br>
                          <span class="foodPrice">{{goods.price}}</span>
                      </li>
                  </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐" name="third">
                 <ul class='cookList'>
                      <li v-for="goods in type2Goods">
                          <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                          <span class="foodName">{{goods.goodsName}}</span><br>
                          <span class="foodPrice">{{goods.price}}</span>
                      </li>
                  </ul>
                </el-tab-pane>
              </el-tabs>
            </template>
          </div>
          </el-col>
      </el-row>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: "Pos",
    data () {
      return {
      tableData: [],
      activeName_auto: 'first',
      activeName: 'first',
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      totalCount:0,
      totalMoney:0
      }
    },
      methods:{
        addOrderList(goods){
//每次添加之前需要清零
        this.totalCount=0;
        this.totalMoney=0;

          let isHave = false;
          for (let i = 0; i < this.tableData.length; i++) {
            //console.log(this.tableData[i].goodsId)
            if(this.tableData[i].goodsId == goods.goodsId){
              isHave = true;
            }
          }

          if(isHave){
            let arr = this.tableData.filter(a =>a.goodsId == goods.goodsId);
            arr[0].count++;
          } else {
            let newGoods = {goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
            this.tableData.push(newGoods);
          }

          //进行数量和价格的汇总计算
          this.tableData.forEach((element) => {
            this.totalCount+=element.count;
            this.totalMoney=this.totalMoney+(element.price*element.count)
          })
        },
        //删除单个商品
        deSingleGoods(goods){
          this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
          this.getAllMoney();
        },
        //汇总数量和金额
        getAllMoney(){
          this.totalCount=0;
          this.totalMoney=0;
          if(this.tableData){
            this.tableData.forEach((element) => {
              this.totalCount+=element.count;
              this.totalMoney=this.totalMoney+(element.price*element.count)
            })
          }
        },
        //删除所有商品
        delAllGoods(){
          this.tableData=[];
          this.totalCount=0;
          this.totalMoney=0;
        },
        //结账
        checkout(){
          if(this.tableData != 0){
            this.tableData = [];
            this.totalCount = 0;
            this.totalMoney = 0;
            this.$message({
              message:'结账成功',
              type: 'success'
            });
          } else {
            this.$message.error('不能空结');
          }
        }
      },
      created:function(){
        //菜单
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
        .then(response=>{
          this.type0Goods=response.data[0];
          this.type1Goods=response.data[1];
          this.type2Goods=response.data[2];
        })
        .catch(error=>{
          console.log('网络错误，不能访问')
        })
        //常用商品
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(response=>{
          this.oftenGoods=response.data;
        })
        .catch(error=>{
          console.log('网络错误，不能访问')
        })
      },
      mounted:function(){
      //获取左侧栏高度
      var orderHeight=document.body.clientHeight;
      console.log(orderHeight)
      document.getElementById("order-list").style.height=orderHeight+'px';
  },
}
</script>

<style scoped>
.pos {
  background-color: #e9eef3;
  width: 100%;
  height: 100vh;
}
#order-list {
  background: #fff;
  border-right: 1px solid #dadada;
}
.el-tabs--border-card {
  box-shadow: none;
  border: none;
}
.el-tabs--top .el-tabs__item.is-top:nth-child(2){
  padding-left: 20px;
}
.el-button+.el-button {
  margin-left: 0;
}
.tableBtn {
  display: flex;
  justify-content: space-between;
  margin: 20px;
}
.oftenGoods ul{
  display: flex;
  flex-wrap: wrap;
  justify-content:flex-start;
}
.oftenGoods ul li {
  padding: 10px;
  border: 1px solid #000;
  border-radius: 4px;
  margin: 5px;
}
.cookList {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
}
.cookList li{
  width: 23%;
  height: auto;
  background: #fff;
  border-radius: 4px;
  overflow: hidden;
  margin: 5px;
  box-shadow: 0 2px 2px 0 rgba(0,0,0,0.14), 0 3px 1px -2px rgba(0,0,0,0.12), 0 1px 5px 0 rgba(0,0,0,0.2);
}
.cookList li .foodImg {
  width: 40%;
  height: 70px;
  display: inline-block;
  vertical-align: middle;
  float: left;
}
.foodName,.foodPrice{
  display: block;
  float: left;
}
.foodName {
  padding: 10px 10px 5px;
  width: 48%;
}
.foodPrice {
  padding: 0 10px;
  color: red;
}
</style>
