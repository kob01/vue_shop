<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图区 -->
    <el-card>
      <div id="main" class="chart-css" ref="myEchart1"></div>
    </el-card>
  </div>
</template>

<script>
import _ from 'lodash'
let myChart = null
const options = {
  title: {
    text: '用户来源'
  },
  tooltip: {
    trigger: 'axis',
    axisPointer: {
      type: 'cross',
      label: {
        backgroundColor: '#E9EEF3'
      }
    }
  },
  grid: {
    left: '3%',
    right: '4%',
    bottom: '3%',
    containLabel: true
  },
  xAxis: [
    {
      boundaryGap: false
    }
  ],
  yAxis: [
    {
      type: 'value'
    }
  ]
}
export default {
  data() {
    return {
      autoHeight: 0
    }
  },
  mounted() {
    this.init()
    window.addEventListener('resize', this.reszieChart)
    // 基于准备好的dom，初始化echarts实例
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.reszieChart)
    myChart = null
  },
  methods: {
    async init() {
      const { data: res } = await this.$http.get('reports/type/1')
      if (res.meta.status === 200) {
        myChart = this.$echarts.init(document.getElementById('main'))
      }
      if (res.meta.status !== 200) {
        return this.$message.error('获取折线图数据失败！')
      }
      this.autoHeight = res.data.series.length * 55 + 50
      // this.counts为柱状图的条数，即数据长度
      myChart.getDom().style.height = this.autoHeight + 'px'
      const result = _.merge(res.data, options)
      // 5展示数据
      myChart.resize()
      myChart.setOption(result)
    },
    reszieChart() {
      myChart && myChart.resize()
    }
  }
}
</script>

<style lang="less" scoped>
#main {
  width: 800px;
}
</style>
