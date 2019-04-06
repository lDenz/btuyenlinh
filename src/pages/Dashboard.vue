<template>
  <q-page class="q-pa-md">
  		<div class="row">
  			<div class="col col-md-6 col-xs-12">
  				<div class="q-display-1 q-my-md">Demo Dashboard</div>
  				<q-btn label="Display" :color="!editMode ? 'primary' :'light'" icon="remove_red_eye" @click="editMode=false" />
  				<q-btn label="Edit" class="q-ml-md" :color="editMode ? 'primary' :'light'" icon="edit" @click="editMode=true"/>
  			</div>
  			<div class="col col-md-6 col-xs-12">
  				<tool-box v-if="editMode"></tool-box>
  			</div>
  		</div>
  		<hr class="q-hr q-my-lg">
  		<grid-layout
  			:layout.sync="mockupLayout"
  			:is-draggable="editMode"
  			:col-num="12"
  			:row-height="55"
  			:is-resizable="false"
  			:responsive="true"
  			:cols="{ lg: 12, md: 10, sm: 6, xs: 4, xxs: 2 }"
  			:margin="[10,10]"
  			@layout-ready="layoutLoaded = true"
  			ref="gridLayout"
  		>
  			<grid-item v-for="item in mockupLayout"
  				style="background:#f3f3f3"
  				:key="item.i"
  				:x="item.x"
  				:y="item.y"
  				:w="4"
  				:h="4"
  				:i="item.i"
  			>
  				<chart :data="item.chart" :index="item.i" :hasEdit="editMode" v-if="layoutLoaded"></chart>
  			</grid-item>
  		
  		</grid-layout>
  </q-page>
</template>

<script>
import VueGridLayout from 'vue-grid-layout';
import ToolBox from 'components/toolbox'
import Chart from 'components/chart'
let mockupPie = {
	customWidth: '100%',
	customHeight: '200px',
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
let mockupBar = {
	customWidth: '100%',
	customHeight: '200px',
	customTitle: 'Demo bar',
	grid: { height: 100, bottom:30 },
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
    	editMode: false, // thay thế = props
    	layoutLoaded: false,
      mockupLayout: [
	      {"x":0,"y":0,"w":4,"h":4,"i":"0", "chart": mockupPie},
		    {"x":4,"y":0,"w":4,"h":4,"i":"1", "chart": mockupBar},
		    {"x":8,"y":0,"w":4,"h":4,"i":"2", "chart": mockupPie },
		    {"x":0,"y":4,"w":4,"h":4,"i":"3", "chart": mockupBar },
		    {"x":4,"y":4,"w":4,"h":4,"i":"4", "chart": mockupBar },
		    {"x":8,"y":4,"w":4,"h":4,"i":"5", "chart": mockupPie },
		    {"x":0,"y":8,"w":4,"h":4,"i":"6", "chart": mockupBar },
		    {"x":4,"y":8,"w":4,"h":4,"i":"7", "chart": mockupPie },
		    {"x":8,"y":8,"w":4,"h":4, "i":"8", "chart": mockupPie }
		    // 9
		  ],
    }
  },
  created () {
  	this.$root.$on('addChart', (type) => this.addChart(type) )
    this.$root.$on('changeTitle', (name,i) => {
      let index = this.mockupLayout.findIndex(x => x.i == i )
      this.mockupLayout[index].chart.customTitle = name
    })
  },
  methods: {
  	addChart(type) {
  		this.layoutLoaded = false
  		let newChart = (type == 'pie') ? mockupPie : mockupBar
  		newChart.customTitle = 'New Chart'
  		let item = {"x":0,"y":0,"w":4,"h":4, "i":this.mockupLayout.length+"", "chart": newChart}
  		this.mockupLayout.push(item)
  		setTimeout(() => this.layoutLoaded=true, 100 )
  	},
  }
}
</script>

