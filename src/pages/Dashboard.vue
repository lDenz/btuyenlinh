<template>
	<q-page class="q-pa-md">
			<div class="row">
				<div class="col-md-6 col-xs-12">
					<div class="q-display-1 q-my-md">Demo Dashboard</div>
					<q-btn label="Display" :color="!mode ? 'primary' :'light'" icon="remove_red_eye" @click="mode=false" />
					<q-btn label="Edit" class="q-ml-md" :color="mode ? 'primary' :'light'" icon="edit" @click="mode=true"/>
				</div>
				<div class="col-md-6 col-xs-12">
					<div class="grid grid-1">
            <div class="item" data-type="pie">
              <div class="item-content">
                <chart :data="toolbox.samplePie" ></chart>
              </div>
            </div>
            <div class="item" data-type="bar">
              <div class="item-content">
                <chart :data="toolbox.sampleBar" ></chart>
              </div>
            </div>
          </div>
					<!-- <tool-box v-if="mode"></tool-box> -->
				</div>
			</div>
			<hr class="q-hr q-my-lg">

			<div class="grid grid-2">
        <div class="item" :data-type="item.chart.series[0].type" v-for="item in gridItems" :key="item.i">
          <div class="item-content" >
            <chart :data="item.chart" :hasEdit="mode"></chart> 
          </div>
        </div>
      </div>
	</q-page>
</template>

<script>
import Muuri from 'muuri'
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
		'tool-box' : ToolBox,
		'chart' : Chart
	},
	data () {
		return {
			toolbox: { sampleBar: sampleBar, samplePie: samplePie},
			mode: false,
			gridUpdated: true,
			gridLoaded: false,
			gridItems: [
				{ "i":0, "chart": sampleBar},
		    { "i":1, "chart": samplePie},
		    { "i":2, "chart": sampleBar },
		    { "i":3, "chart": sampleBar },
		    { "i":4, "chart": samplePie },
		    { "i":5, "chart": sampleBar },
		    { "i":6, "chart": samplePie },
		    { "i":7, "chart": sampleBar },
		    { "i":8, "chart": samplePie }
			],
		}
	},
	mounted () {
		let vm = this
		var grid1 = new Muuri('.grid-1', {
			dragEnabled:true,
			dragSort: function () {
        return [grid2]
      }
		})
		var grid2Hash = {};
    var grid2 = new Muuri('.grid-2', {
      dragEnabled: true,
      dragSort: true,
    })
    .on('receive', function (data) {
    	let newGrid = []
      grid2Hash[data.item._id] = function (item) {
        if (item === data.item) {
          var clone = vm.cloneElem(data.item.getElement());
          data.fromGrid.add(clone, {index: data.fromIndex});
          data.fromGrid.show(clone);
          grid1.refreshItems().layout();
		  		grid2.refreshItems().layout();

		  		//let newGrid = []
		  		// grid2.getItems().map((item,i) => {
		  		// 	let newItem = {}
		  		// 	let chartType = item.getElement().getAttribute('data-type')
		  		// 	newItem.i = i
		  		// 	newItem.chart = (chartType == 'pie') ? samplePie : sampleBar
		  		// 	newGrid.push(newItem)
		  		// })
		  		
		  		//vm.gridItems.splice(data.toIndex, 0, { i:10,  chart:sampleBar } )
		  		//vm.gridItems = newGrid
		  		//vm.$forceUpdate()
          
        }
      };
      grid2.once('dragReleaseStart', grid2Hash[data.item._id]);
    })
    // .on('send', function (data) {
    //   var listener = grid2Hash[data.item._id];
    //   if (listener) {
    //     //grid2.off('dragReleaseStart', listener);
    //   }
    // })
		window.addEventListener('load', function () {
      grid1.refreshItems().layout();
		  grid2.refreshItems().layout();
		});
	},
	methods: {
		addChart(type) {
		},
		cloneElem(elem) {
			let canvasOld= elem.querySelector('canvas')
      var clone = elem.cloneNode(true);
      // redraw canvas
      let newCanvas = clone.querySelector('canvas')
      newCanvas =  newCanvas.getContext('2d')
      newCanvas.drawImage(canvasOld, 0, 0)
      clone.setAttribute('style', 'display:none;');
      clone.setAttribute('class', 'item');
      clone.children[0].setAttribute('style', '');
      return clone;
    },
    saveGridItems () {

    }
	}
}
</script>

