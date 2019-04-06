<template>
	<q-page class="q-pa-md">
			<div class="row">
				<div class="col col-md-6 col-xs-12">
					<div class="q-display-1 q-my-md">Demo Dashboard</div>
					<q-btn label="Display" :color="!mode ? 'primary' :'light'" icon="remove_red_eye" @click="mode=false" />
					<q-btn label="Edit" class="q-ml-md" :color="mode ? 'primary' :'light'" icon="edit" @click="mode=true"/>
				</div>
				<div class="col col-md-6 col-xs-12">
					<tool-box v-if="mode"></tool-box>
				</div>
			</div>
			<hr class="q-hr q-my-lg">
			<grid-layout
				:layout.sync="gridItems"
				:is-draggable="mode"
				:col-num="12"
				:row-height="55"
				:is-resizable="false"
				:responsive="true"
				:cols="{ lg: 12, md: 12, sm: 8,  xxs: 4 }"
				:margin="[30,30]"
				@layout-ready="gridLoaded = true"
				ref="gridLayout"
			>
				<grid-item v-for="item in gridItems"
					style="background:#f3f3f3; box-shadow: 1px 5px 10px #ccc; border: 1px solid #999; border-radius: 4px;"
					:key="item.i"
					:x="item.x"
					:y="item.y"
					:w="4"
					:h="4"
					:i="item.i"
				>
					<chart :data="item.chart" :index="item.i" :hasEdit="mode" ></chart>
				</grid-item>
			
			</grid-layout>
	</q-page>
</template>

<script>
import VueGridLayout from 'vue-grid-layout';
import ToolBox from 'components/toolbox'
import Chart from 'components/chart'
let samplePie = {
	customWidth: '100%',
	customHeight: '250px',
	customTitle: 'Demo pie',
	series : [{
			name: 'BBBBB', type: 'pie', radius: '60%',
			data:[
					{value:310, name:'Test abcd'},
					{value:335, name:'Bcd Test'},
					{value:400, name:'ABCDFEE'}
			]
	}],
}
//
let sampleBar = {
	customWidth: '100%',
	customHeight: '250px',
	customTitle: 'Demo bar',
	grid: { height: 150, bottom:30 },
	xAxis: {
		type: 'category',
		data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
	},
	yAxis: { type: 'value' },
	series: [{
			data: [120, 200, 150, 80, 70, 110, 130],
			type: 'bar'
	}],
}
export default {
	name: 'PageDashboard',
	components: {
		'grid-layout': VueGridLayout.GridLayout,
		'grid-item': VueGridLayout.GridItem,
		'tool-box' : ToolBox,
		'chart' : Chart
	},
	data () {
		return {
			mode: false,
			gridLoaded: false,
			gridItems: [
				{"x":0,"y":0,"i":"0", "chart": samplePie},
				{"x":4,"y":0,"i":"1", "chart": sampleBar},
				{"x":8,"y":0,"i":"2", "chart": samplePie },
				{"x":0,"y":4,"i":"3", "chart": sampleBar },
				{"x":4,"y":4,"i":"4", "chart": sampleBar },
				{"x":8,"y":4,"i":"5", "chart": samplePie },
				{"x":0,"y":8,"i":"6", "chart": sampleBar },
				{"x":4,"y":8,"i":"7", "chart": samplePie },
				{"x":8,"y":8, "i":"8", "chart": samplePie }
			],
		}
	},
	created () {
		this.$root.$on('addChart', (type) => this.addChart(type) )
		this.$root.$on('changeTitle', (name,i) => {
			let index = this.gridItems.findIndex(x => x.i == i )
			this.gridItems[index].chart.customTitle = name
		})
	},
	methods: {
		addChart(type) {
			this.gridLoaded = false
			let newChart = (type == 'pie') ? samplePie : sampleBar
			newChart.customTitle = 'New Chart'
			let item = {"x":0,"y":0, "i":this.gridItems.length+"", "chart": newChart}
			this.gridItems.push(item)
		},
	}
}
</script>

