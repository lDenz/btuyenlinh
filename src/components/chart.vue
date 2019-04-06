<template>
<div class="m-widget">
	<v-chart :options="data"  :style="{width: data.customWidth, height:data.customHeight}" />
	<property-inspector :title="data.customTitle" :index="index" :hasEdit="hasEdit"></property-inspector>
	
</div>
</template>

<script>
import ECharts from 'vue-echarts'
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/pie'
import PropertyInspector from './property-inspector'
export default {
	props: ['data', 'hasEdit', 'index'],
	components: {
  	'v-chart': ECharts,
  	PropertyInspector
  },
  created () {
  	this.$on('changeTitle', (name) => {

  	})
  }

}
</script>

<style>
</style>