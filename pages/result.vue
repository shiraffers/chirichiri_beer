<template>
  <div>
    <Chart class="chart" :graphData="sex"></Chart>
    <Chart class="chart" :graph-data="smiling"></Chart>
  </div>
</template>

<style>
  .chart {
    width: 40%;
    float: left;
  }
</style>

<script>
import Chart from '../components/layout/Chart';

export default {
  components: {
    Chart,
  },
  data() {
    return {
      sex: { type: ['man', 'woman'], data1: 60, data2: 40, color: [ 'rgba(60, 60, 255, 0.3)', 'rgba(255, 60, 60, 0.3)' ] }, // ここにデータ挿入
      smiling: { type: ['smiling'], data1: 60, data2: 40, color: [ 'rgba(255, 255, 0, 0.3)' ] }, // ここにデータ挿入
    }
  }
}
</script>
