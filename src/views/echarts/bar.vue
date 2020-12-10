<template>
  <page-header-wrapper>
    <a-card>
      <echarts-bar :elemId="'ceshi'" :xData="xData" :chartConfig="chartConfig" :seriesData="seriesData" />
    </a-card>
  </page-header-wrapper>
</template>
<script>
import { PageHeaderWrapper } from '@ant-design-vue/pro-layout'
import EchartsBar from './components/EchartsBar'
export default {
  name: 'Workplace',
  components: {
    PageHeaderWrapper
  },
  data () {
    return {
      xData:['周一', '周二'],
      chartConfig:{

      },
      seriesData:[]
    }
  },
  computed: {
  },
  components:{
    EchartsBar
  },
  created () {
  },
  mounted () {
  },
  methods: {
  }
}
</script>

<style lang="less" scoped>

</style>
