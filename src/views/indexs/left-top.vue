<template>
	<div class="left_top">
		<Echart :options="options" id="defectRateLineChart" class="echarts_bottom"></Echart>
	</div>
</template>

<script>
	import {
		currentGET
	} from 'api/modules';
	import {
		graphic
	} from "echarts";
	import {
		eventBus
	} from '../../main.js';

	export default {
		data() {
			return {
				clickedProvince: '',
				defectRateData: [],
				options: {},
				
			};
		},
		created() {
	eventBus.$on('map-province-clicked', (params) => {
				console.log(params.name + "阿萨");
				this.clickedProvince = params.name;
				this.getData();
			});
			eventBus.$on('get-all-data', () => {
				// 调用获取并展示全部数据的方法（需根据实际需求自行实现该方法内具体的数据获取、页面更新等逻辑）
				console.log("清空函数");
				this.clickedProvince = ''
				this.getData();
			});
			this.getData();

		},
		props: {},
		mounted() {
			eventBus.$on('map-province-clicked', (params) => {
				console.log(params.name+"阿萨");
				// 根据接收到的参数更新数据，这里简单将省份名称赋值给clickedProvince，实际可按需处理
				this.clickedProvince = params.name;
				this.getData();
			});
			  eventBus.$on('get-all-data', () => {
			        // 调用获取并展示全部数据的方法（需根据实际需求自行实现该方法内具体的数据获取、页面更新等逻辑）
						this.clickedProvince = ''
			        	this.getData();
			    });
					  eventBus.$on('hide-video-and-show-map', () => {
					  		 this.clickedProvince = ''
					         this.getData();
					  						
					         });
			  this.getData()
		},
		methods: {
			getData() {
				console.log("++++" + this.clickedProvince);
				const param = {
					area: this.clickedProvince
				};
				// currentGET("reportCPL", param).then((res) => {
				// 	console.log("次品率数据", res);
				// 	if (res.code === 200) {
						// this.defectRateData = res.rows;
			
						this.init();
				// 	} else {
				// 		this.$Message({
				// 			text: res.msg,
				// 			type: "warning"
				// 		});
				// 	}
				// });
			},
			init() {
				const dates = this.defectRateData.map(data => data.date);
				const defectRates = this.defectRateData.map(data => data.defectRate);

				this.options = {
					tooltip: {
						trigger: 'axis',
						backgroundColor: 'rgba(0, 0, 0, 0.6)',
						borderColor: 'rgba(147, 235, 248, 0.8)',
						textStyle: {
							color: '#FFF'
						},
						formatter: function(params) {
							return params[0].name + '<br>' + params[0].seriesName + ' : ' + params[0].value + '%';
						}
					},
					legend: {
						data: ['饲料'],
						textStyle: {
							color: '#B4B4B4'
						},
						top: '0'
					},
					grid: {
						left: '50px',
						right: '40px',
						bottom: '30px',
						top: '20px'
					},
					// xAxis: {
					// 	type: 'category',
					// 	data: dates,
					// 	axisLine: {
					// 		lineStyle: {
					// 			color: '#B4B4B4'
					// 		}
					// 	},
					// 	axisTick: {
					// 		show: false
					// 	}
					// },
					xAxis:{
						type: 'category',
						data:['1','2','3','4','5','6','7']
					},
					// yAxis: {
					// 	splitLine: {
					// 		show: false
					// 	},
					// 	axisLine: {
					// 		lineStyle: {
					// 			color: '#B4B4B4'
					// 		}
					// 	},
					// 	axisLabel: {
					// 		formatter: '{value}%'
					// 	}
					// },
					yAxis:{
						type: 'value'
					},
					series: [{
						name: '饲料',
						// type: 'line',
						smooth: true,
						showAllSymbol: true,
						symbol: 'circle',
						symbolSize: 8,
						itemStyle: {
							color: '#00ffff'
						},
						// data: defectRates
						data: [120, 200, 150, 80, 70, 110, 130],
						type: 'bar'
					}]
				};
			}
		}
	};
</script>

<style lang="scss" scoped>
	.left_top {
		width: 100%;
		height: 100%;

		.echarts_bottom {
			width: 100%;
			height: 100%;
		}
	}
</style>