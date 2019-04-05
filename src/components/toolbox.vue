<template>
<div class="m-toolbox">
	<q-card inline style="width:50%; padding:20px;">
				<chart :data="pie" @click="test()"></chart>
				<div class="text-center">
					<q-btn label="Add New Pie Chart" size="sm" color="secondary" @click="$root.$emit('addChart','pie')"/>
				</div>
		</q-card>
		<q-card inline style="width:50%; padding:20px;" >
				<chart :data="bar"></chart>
				<div class="text-center">
					<q-btn label="Add New Bar Chart" size="sm" color="secondary" @click="$root.$emit('addChart','bar')"/>
				</div>
		</q-card>
	</div>
	
</div>
</template>

<script>

import Chart from './chart'

export default {
	props: ['height', 'width', 'type', 'data', 'hasEdit'],
	components: {
  	'chart': Chart
  },
  data () {
  	return {
  		pie: {
  			customWidth: '100%',
  			customHeight: '150px',
  			customTitle: ' ',
        series : [{
            name: 'BBBBB', type: 'pie', radius: '60%',
            data:[
                {value:310, name:'Test abcd'},
                {value:335, name:'Bcd Test'},
                {value:400, name:'ABCDFEE'}
            ]
        }],
        animationDuration: 1000
      },
      //
      bar: {
      	customWidth: '100%',
  			customHeight: '150px',
  			customTitle: ' ',
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
        animationDuration: 1000
      },
  	}
  },
  methods: {
  	test () {
  		console.log('abcd')
  	}
  }
}
</script>

<style>
.m-widget--title { cursor:default; }
</style>