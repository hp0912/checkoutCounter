<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" class="pos-container">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width: 100%;">
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="order_delete(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="order_add(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div style="text-align: right;padding: 2px 5px;">
                <span>总共: {{ totalPrice }} 元</span>
              </div>
              <div class="a1">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="order_delete_all">删除</el-button>
                <el-button type="success" @click="order_checkout">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              2
            </el-tab-pane>
            <el-tab-pane label="外卖">
              3
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span="17" class="pos-container">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul style="overflow: hidden;padding-left: 10px;margin: 0">
                <li v-for="good in oftenGoods" :key="good.goodsId" @click="order_add(good)">
                  <span>{{ good.goodsName }}</span>
                  <span class="o-price">￥ {{ good.price }}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class='cookList' style="overflow: hidden;padding-left: 10px;margin: 0;">
                  <li v-for="good in type0Goods" :key="good.goodsId" @click="order_add(good)">
                    <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                    <span class="foodName">{{ good.goodsName }}</span>
                    <span class="foodPrice">￥ {{ good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class='cookList' style="overflow: hidden;padding-left: 10px;margin: 0;">
                  <li v-for="good in type1Goods" :key="good.goodsId" @click="order_add(good)">
                    <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                    <span class="foodName">{{ good.goodsName }}</span>
                    <span class="foodPrice">￥ {{ good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class='cookList' style="overflow: hidden;padding-left: 10px;margin: 0;">
                  <li v-for="good in type2Goods" :key="good.goodsId" @click="order_add(good)">
                    <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                    <span class="foodName">{{ good.goodsName }}</span>
                    <span class="foodPrice">￥ {{ good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class='cookList' style="overflow: hidden;padding-left: 10px;margin: 0;">
                  <li v-for="good in type3Goods" :key="good.goodsId" @click="order_add(good)">
                    <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                    <span class="foodName">{{ good.goodsName }}</span>
                    <span class="foodPrice">￥ {{ good.price}}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'

export default {
  name: 'Pos',
  data () {
    return {
      totalPrice: 0,
      tableData: [],
      oftenGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: []
    }
  },
  methods: {
    order_add (goods) {
      let index = -1
      let newGoods = {}

      for (let i = 0, len = this.tableData.length; i < len; i++) {
        if (goods.goodsId === this.tableData[i].goodsId) {
          index = i
          break
        }
      }

      if (index === -1) {
        $.extend(newGoods, goods)
        newGoods.count = 1
        this.tableData.push(newGoods)
      } else {
        this.tableData[index].count++
      }

      this.totalPrice += +goods.price
    },
    order_delete (goods) {
      let index = -1
      for (let i = 0, len = this.tableData.length; i < len; i++) {
        if (goods.goodsId === this.tableData[i].goodsId) {
          index = i
          break
        }
      }

      if (goods.count === 1) {
        this.tableData.splice(index, 1)
      } else {
        this.tableData[index].count--
      }

      this.totalPrice -= goods.price
    },
    order_delete_all () {
      this.tableData = []
      this.totalPrice = 0
    },
    order_checkout () {
      if (this.totalPrice !== 0) {
        this.tableData = []
        this.totalPrice = 0
        this.$message.success('结账成功.')
      } else {
        this.$message.error('请先选择商品.')
      }
    }
  },
  created () {
    $.get('http://jspang.com/DemoApi/oftenGoods.php').then(result => {
      this.oftenGoods = JSON.parse(result)
    }).fail(result => {
      console.log(result)
    })

    $.ajax({
      'url': 'http://jspang.com/DemoApi/typeGoods.php',
      'method': 'get',
      'dataType': 'json'
    }).done(result => {
      this.type0Goods = result[0]
      this.type1Goods = result[1]
      this.type2Goods = result[2]
      this.type3Goods = result[3]
    }).fail(result => {
      console.log(result)
    })
  },
  mounted () {
    var elements = document.getElementsByClassName('pos-container')
    var containerHeight = document.body.clientHeight
    for (var i = 0, len = elements.length; i < len; i++) {
      elements[i].style.height = containerHeight + 'px'
    }
  }
}
</script>

<style scoped>
.pos-container {
  border-right: 1px solid #909399;
}

.a1 {
  margin-top: 10px;
  text-align: right;
}

.title {
  height: 20px;
  border-bottom:1px solid #D3DCE6;
  background-color: #F9FAFC;
  padding:10px;
}

.often-goods-list ul li {
  list-style: none;
  float:left;
  border:1px solid #E5E9F2;
  padding:10px;
  margin:5px;
  background-color:#fff;
}

.o-price {
  color:#58B7FF;
}

.cookList li {
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

.cookList li span {
  display: block;
  float:left;
}

.foodImg {
  width: 40%;
}

.foodName {
  font-size: 18px;
  padding-left: 10px;
  color:brown;
}

.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top:10px;
}

.goods-type {
  clear: both;
}
</style>
