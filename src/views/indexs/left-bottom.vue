<template>
	<div id="egg-weight-pie-chart" style="width: 100%; height: 100%;"></div>
</template>

<script>
	import * as echarts from 'echarts';
	import {
		currentGET
	} from 'api/modules';
	import {
		eventBus
	} from '../../main.js';

	export default {
		data() {
			return {
				eggWeightData: [],
				options: {}
			};
		},
		mounted() {
			eventBus.$on('map-province-clicked', (params) => {
				console.log(params.name + "阿萨");
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
				// currentGET("reportSALDZZB", param).then((res) => {
				// 	console.log("蛋重数据", res);
				// 	if (res.code === 200) {
				// 		this.eggWeightData = res.rows;

						this.initChart();
					// } else {
					// 	this.$Message({
					// 		text: res.msg,
					// 		type: "warning"
					// 	});
					// }
				// });
			},
			initChart() {
				const totalWeight = this.eggWeightData.reduce((acc, cur) => acc + cur.defectRate, 0);
				const chartDom = document.getElementById('egg-weight-pie-chart');
				const myChart = echarts.init(chartDom);

				const seriesData = this.eggWeightData.map(item => ({
					name: item.name,
					value: item.defectRate,
					percentage: ((item.defectRate / totalWeight) * 100).toFixed(2)
				}));

				this.options = {
					// title: {
					// 	text: '每天蛋的重量占比'
					// },
					// tooltip: {
					// 	trigger: 'item',
					// 	formatter: '{a} <br/>{b} : {c} ({d}%)'
					// },
					// series: [{
					// 	name: '蛋重占比',
					// 	type: 'pie',
					// 	radius: ['40%', '70%'],
					// 	data: seriesData
					// }]
					tooltip: {
					trigger: 'axis'
				},
				legend: {
					data: ['Email', 'Union Ads', 'Video Ads', 'Direct', 'Search Engine']
				},
				grid: {
					left: '3%',
					right: '4%',
					bottom: '3%',
					containLabel: true
				},
				xAxis: {
					type: 'category',
					boundaryGap: false,
					data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
				},
				yAxis: {
					type: 'value'
				},
				series: [
					{
						name: 'Email',
						type: 'line',
						stack: 'Total',
						data: [120, 132, 101, 134, 90, 230, 210]
					},
					{
						name: 'Union Ads',
						type: 'line',
						stack: 'Total',
						data: [220, 182, 191, 234, 290, 330, 310]
					},
					{
						name: 'Video Ads',
						type: 'line',
						stack: 'Total',
						data: [150, 232, 201, 154, 190, 330, 410]
					},
					{
						name: 'Direct',
						type: 'line',
						stack: 'Total',
						data: [320, 332, 301, 334, 390, 330, 320]
					},
					{
						name: 'Search Engine',
						type: 'line',
						stack: 'Total',
						data: [820, 932, 901, 934, 1290, 1330, 1320]
					}
				]
				};

				myChart.setOption(this.options);
			}
		}
	};
</script>

<style scoped>
	/* 可以添加自定义样式 */
</style>