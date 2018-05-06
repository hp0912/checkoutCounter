<template>
  <div class="pos">
    <div>
      <el-row>
        <el-col :span="7" class="pos-container">
          AAAAAAAAAAAAAAAAA
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
</style>
