<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" class="pos-container">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border show-summary style="width: 100%;">
                <el-table-column prop="goodsName" label="商品"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small">删除</el-button>
                    <el-button type="text" size="small">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="a1">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger">删除</el-button>
                <el-button type="success">结账</el-button>
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
          BBBBBBBBBBBBBBBBB
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Pos',
  data () {
    return {
      tableData: [{
        goodsName: '可口可乐',
        price: 8,
        count: 1
      }, {
        goodsName: '香辣鸡腿堡',
        price: 15,
        count: 1
      }, {
        goodsName: '爱心薯条',
        price: 8,
        count: 1
      }, {
        goodsName: '甜筒',
        price: 8,
        count: 1
      }]
    }
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
</style>
