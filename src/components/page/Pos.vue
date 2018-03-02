<template>
  <div class="pos">
    <el-row>
      <el-col :span = '7' class="order">
        <el-tabs>
          <el-tab-pane label = '点餐'>
            <el-table :data= 'tableData' border>
              <el-table-column prop='goodsName' label='商品名称'></el-table-column>
              <el-table-column prop='count' label='数量'></el-table-column>
              <el-table-column prop='price' label='金额'></el-table-column>
              <el-table-column label='操作' width='100' fixed='right'>
                <template scope="scope">
                  <el-button type='text' size='small' @click = 'delOrderList(scope.row)'>删除</el-button>
                  <el-button type='text' size='small' @click = 'addOrderList(scope.row)'>添加</el-button>
                </template>
              </el-table-column>

            </el-table>
            <div class="total">
              <p>总价:{{total}}元</p>
            </div>
            <div>
              <el-button>挂单</el-button>
              <el-button @click = 'delAll'>删除</el-button>
              <el-button @click = "pay">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label = '挂单'>挂单</el-tab-pane>
          <el-tab-pane label = '外卖'>外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span='17' class="goods_main">
        <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
                <ul>
                    <li v-for='goods in oftenGoods' @click = 'addOrderList(goods)'>
                        <span>{{goods.goodsName}}</span>
                        <span class="o-price">￥{{goods.price}}</span>
                    </li>
                </ul>
            </div>
        </div>
        <div class="goods-type">
            <el-tabs>
                <el-tab-pane label="汉堡">
                    <ul class='cookList'>
                        <li v-for='goods in type0Goods' @click = 'addOrderList(goods)'>
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                <el-tab-pane label="小食">
                    <ul class='cookList'>
                        <li v-for='goods in type1Goods' @click = 'addOrderList(goods)'>
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                <el-tab-pane label="饮料">
                    <ul class='cookList'>
                        <li v-for='goods in type2Goods' @click = 'addOrderList(goods)'>
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐">
                    <ul class='cookList'>
                        <li v-for='goods in type3Goods' @click = 'addOrderList(goods)'>
                            <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                            <span class="foodName">{{goods.goodsName}}</span>
                            <span class="foodPrice">￥{{goods.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
            </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      total: 0
    };
  },
  created: function() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(res => {
        //console.log(res)
        this.oftenGoods = res.data;
      })
      .catch(error => {
        console.log(error);
      });

    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(res => {
        console.log(res);
        this.type0Goods = res.data[0];
        this.type1Goods = res.data[1];
        this.type2Goods = res.data[2];
        this.type3Goods = res.data[3];
      })
      .catch(error => {
        console.log(error);
      });
  },
  mounted() {
    var orderH = document.body.clientHeight;
    document.querySelector(".order").style.height = orderH + "px";
    document.querySelector(".goods_main").style.height = orderH + "px";
  },
  methods: {
    addOrderList(goods) {
      this.total = 0;
      var isHave = false;
      for (var i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      if (isHave) {
        this.tableData.filter(function(o) {
          return o.goodsId == goods.goodsId;
        })[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.totalMoney();
      // for(var i = 0; i < this.tableData.length; i++){
      //   this.total += this.tableData[i].count * this.tableData[i].price
      // }
    },
    delOrderList(goods) {
      this.tableData = this.tableData.filter(e => e.goodsId != goods.goodsId);
      this.totalMoney();
    },
    delAll(){
      this.tableData = []
      this.total = 0;
    },
    totalMoney() {
      this.total = 0;
      this.tableData.forEach(el => {
        this.total += el.count * el.price;
      });
    },
    pay(){
      if(this.total){
        alert('结账成功')
        this.delAll()
      }else{
        alert('请选择商品')
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.order {
  background: #f9fafc;
  border-right: 1px solid #c0ccda;
}
li {
  cursor: pointer;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}

.cookList li {
  list-style: none;
  width: 22%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding: 10px;
  float: left;
  margin: 10px;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.goods_main {
  background: rgb(210, 229, 243);
}
</style>
