<template>
<div class="m-widget">
	<v-chart :options="data"  :style="{width: data.customWidth, height:data.customHeight}" />
	<div class="q-title text-center" :class="data.customTitle ? '': 'text-light'">
		{{data.customTitle || 'No title'}}
		<q-popup-edit v-model="data.customTitle" title="Edit name chart" buttons v-if="hasEdit">
    	<q-input type="text" v-model="data.customTitle" />
  	</q-popup-edit>
	</div>
	
</div>
</template>

<script>
import ECharts from 'vue-echarts'
import 'echarts/lib/chart/bar'
import 'echarts/lib/chart/pie'
export default {
	props: ['data', 'hasEdit'],
	components: {
  	'v-chart': ECharts
  }

}
//:init-options="{renderer:'svg'}"
</script>

<style>
.echarts { width:100%; height:100%; }
.m-widget--title { cursor:default; }
</style>