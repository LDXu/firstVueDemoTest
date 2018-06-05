<template>
    <div class="pos">
        <el-row>
            <el-col :span='7' class="order" id="order-list">订单栏

                <el-tabs class="tabs">
                    <el-tab-pane label='点餐'>
                        <el-table :data='tableData' border style="width:100%">
                            <el-table-column prop='goodsName' label='商品'></el-table-column>
                            <el-table-column prop='count' label='量' width='50'></el-table-column>
                            <el-table-column prop='price' label='金额' width='70'></el-table-column>
                            <el-table-column label='操作' width='100'>
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        
                         <div class="totalDiv">
                                <small>数量：</small>
                                <strong>{{totalCount}}</strong> &nbsp;&nbsp;&nbsp;&nbsp;
                                <small>总计：</small>
                                <strong>{{totalMoney}}</strong> 元
                        </div>

                        <el-button type="warning" >挂单</el-button>
                        <el-button type="danger" @click="delAllGoods()">删除</el-button>
                        <el-button type="success" @click="checkout()">结账</el-button>
                    </el-tab-pane>



                    <el-tab-pane label='挂单'></el-tab-pane>


                    <el-tab-pane label='外卖'></el-tab-pane>
                </el-tabs>


            </el-col>
            <el-col :span='17' class="goods">商品栏
                <div class="often-goods">
                    <div class="title">常用商品</div>
                    <div class="often-goods-list">
                        <ul>
                            <li v-for="goods in oftenGoods" :key="goods.goodsId" @click="addOrderList(goods)">
                                <span class="foodName">{{goods.goodsName}}</span>
                                <span class="foodPrice">￥{{goods.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </div>

                <div class="goods-type">
                    <el-tabs>
                        <el-tab-pane label="汉堡">
                             <ul>
                                <li v-for="goods in type0Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="40px" height="40px"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="小食">
                             <ul>
                                <li v-for="goods in type1Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="40px" height="40px"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                             <ul>
                                <li v-for="goods in type2Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="40px" height="40px"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                             <ul>
                                <li v-for="goods in type3Goods" :key="goods.goodsId" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="40px" height="40px"></span>
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
  name: "Pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0, //订单总价格
      totalCount: 0 //订单商品总数量
    };
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  created() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(result => {
        console.log("----------------------------------->");
        console.log(result.data);
        this.oftenGoods = result.data;
      })
      .catch(err => {
        console.log("------------------------------------");
        console.log(err);
        alert("网络错误");
        console.log("------------------------------------");
      });

    //读取分类商品列表
    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        console.log(response);
        //this.oftenGoods=response.data;
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },
  methods: {
    addOrderList(goods) {
      let isHave = false;

      for (let i = 0; i < this.tableData.length; i++) {
        console.log(this.tableData[i].goodsId);
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true; //存在
        }
      }

      //根据isHave的值判断订单列表中是否已经有此商品
      if (isHave) {
        //存在就进行数量添加
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
        //console.log(arr);
      } else {
        //不存在就推入数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
        //注意不能修改原来的，不然显示一直不变
        // goods.count=1;
        // this.tableData.push(goods);
      }

      //进行数量和价格的汇总计算
      this.getAllMoney();
    },
    //删除按钮事件
    delSingleGoods(goods) {
      console.log(goods);
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      //进行数量和价格的汇总计算
      this.getAllMoney();
    },
    //汇总数量和金额
    getAllMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },
    //删除所有商品
    delAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    checkout() {
    if (this.totalCount!=0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        setTimeout(() => {
            this.$message({
            message: '结账成功，感谢你又为店里出了一份力!',
            type: 'success'
        });
        }, 2);
    }else{
        this.$message.error('不能空结。老板了解你急切的心情！');
    }
 
}
  }
};
</script>

<style scoped>
.order {
  /* background-color: lightgray; */
}
.goods {
  /* background-color: blanchedalmond; */
}
.tabs {
    background-color: #fff;
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
  overflow: hidden;
  background-color: #fff;
  display: flex;
  align-items: center;
}
.goods-type {
  clear: both;
}
.goods-type ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  overflow: hidden;
  background-color: #fff;
  display: flex;
  align-items: center;
}
.goods-type ul li span {
  display: block;
  float: left;
}
.o-price {
  color: #58b7ff;
}
.foodImg {
}
.foodName {
  margin-left: 5px;
}
.foodPrice {
  margin-left: 5px;
  color: #58b7ff;
}
.totalDiv {
  height: auot;
  overflow: hidden;
  text-align: right;
  font-size: 16px;
  background-color: #fff;
  border-bottom: 1px solid #e5e9f2;
  padding: 10px;
}
</style>