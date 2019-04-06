<template>
	<div class="m-widget">
		<e-charts :options="data" :autoresize="true" :style="{width: data.customWidth, height:data.customHeight}" />
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
		ECharts,
		PropertyInspector
	}
}
</script>

<style>
</style>