<template>
  <div id="app">
    <div class="btn" @click="handleBtn">
      {{text}}
    </div>
    <TreeChart :json="treeList" :class="{landscape: isShow}"></TreeChart>
  </div>
</template>

<script>
import TreeChart from './components/TreeChart.vue'
import data from './components/tree'
export default {
  name: 'App',
  data() {
    return {
      treeList:data,
      isShow:false,
      text:'纵向',
    }
  },
  components: {
    TreeChart
  },
  mounted() {
    console.log(data)
  },
  methods:{
    handleBtn() {
      this.isShow = !this.isShow;
      if (this.isShow) {
        this.text = '横向'
      } else {
        this.text = '纵向'
      }
    }
  }
}
</script>

<style lang="less">
.btn{
  width: 80px;
  height: 30px;
  border: 1px solid #813382;
  text-align: center;
  line-height: 30px;
  background: #813382;
  color: #ffffff;
  border-radius: 10px;
}
</style>
