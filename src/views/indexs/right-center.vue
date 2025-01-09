<!--
 * @Author: daidai
 * @Date: 2022-02-28 16:16:42
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-07-20 17:57:11
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\left-center.vue
--><template>
	<div class="user_Overview flex" v-if="pageflag">
		<!-- 这里添加一个div作为echarts图表的容器，设置好宽度和高度等样式 -->
		<div id="echartsContainers" style="width: 600px; height: 300px"></div>
	</div>
	<Reacquire v-else @onclick="getData" line-height="200px">
		重新获取
	</Reacquire>
</template>

<script>
	import {
		eventBus
	} from '../../main.js';
	import {
		currentGET
	} from 'api/modules'
	import * as echarts from 'echarts';
	let style = {
		fontSize: 24
	}
	export default {
		data() {
			return {
				clickedProvince: '',
				dataforbiaozhun:[],
				options: {},
				userOverview: {
					alarmNum: 0,
					offlineNum: 0,
					onlineNum: 0,
					totalNum: 0,
				},
				pageflag: true,
				timer: null,

				onlineconfig: {
					number: [0],
					content: '{nt}',
					style: {
						...style,
						// stroke: "#07f7a8",
						fill: "#07f7a8",
					},
				},
				offlineconfig: {
					number: [0],
					content: '{nt}',
					style: {
						...style,
						// stroke: "#e3b337",
						fill: "#e3b337",
					},
				},
				laramnumconfig: {
					number: [0],
					content: '{nt}',
					style: {
						...style,
						// stroke: "#f5023d",
						fill: "#f5023d",
					},
				}

			};
		},
		filters: {
			numsFilter(msg) {
				return msg || 0;
			},
		},
		created() {
			// this.getData()
		},
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
			this.getData();
		
		},
		beforeDestroy() {
			this.clearData()

		},
		methods: {
			clearData() {
				if (this.timer) {
					clearInterval(this.timer)
					this.timer = null
				}
			},
			getData() {
				console.log(this.clickedProvince + "省份");
				// const param = {area:this.clickedProvince}; // 根据接口实际需要传入相应参数，此处示例为空对象
				// currentGET('reporttzsw', param).then(response => {
				// 	// 假设接口返回的数据格式和之前本地定义的newData类似，是一个数组对象形式
				// 	// 这里根据实际接口返回数据结构进行相应调整处理
				// 	// const newDataFromApi = response.rows; // 这里假设接口返回的数据在data属性下，按需修改
				// 	let newDataFromApi = [
				// 		{
				// 			name:"2025-01-01",
				// 			ddsal:3.0,
				// 			pdsal:2.1,
				// 			cdsal:2.5,
				// 			hgdsal:2.8
				// 		},
				// 		{
				// 			name:"2025-01-02",
				// 			ddsal:2.9,
				// 			pdsal:2.1,
				// 			cdsal:2.5,
				// 			hgdsal:2.3
				// 		},
				// 		{
				// 			name:"2025-01-03",
				// 			ddsal:2.8,
				// 			pdsal:2.1,
				// 			cdsal:2.3,
				// 			hgdsal:2.5
				// 		},
				// 		{
				// 			name:"2025-01-04",
				// 			ddsal:3.2,
				// 			pdsal:3.0,
				// 			cdsal:2.4,
				// 			hgdsal:2.9
				// 		},
				// 		{
				// 			name:"2025-01-05",
				// 			ddsal:3.0,
				// 			pdsal:2.1,
				// 			cdsal:2.5,
				// 			hgdsal:2.4
				// 		},	{
				// 			name:"2025-01-06",
				// 			ddsal:3.5,
				// 			pdsal:3.1,
				// 			cdsal:3.2,
				// 			hgdsal:3.4
				// 		},
				// 		{
				// 			name:"2025-01-07",
				// 			ddsal:5.0,
				// 			pdsal:2.1,
				// 			cdsal:3.5,
				// 			hgdsal:2.8
				// 		}
						
				// 	]
				// 	this.dataforbiaozhun = newDataFromApi;
					
				// 	console.log(this.dataforbiaozhun);
				// 	const xAxisData = this.dataforbiaozhun.map(item => item.name);
				// 	const y1AxisData = this.dataforbiaozhun.map(item => item.ddsal);
				// 	const y2AxisData = this.dataforbiaozhun.map(item => item.pdsal);
				// 	const y3AxisData = this.dataforbiaozhun.map(item => item.cdsal);
				// 	const y4AxisData = this.dataforbiaozhun.map(item => item.hgdsal);

					// this.initEcharts(xAxisData, y1AxisData, y2AxisData, y3AxisData, y4AxisData);
			this.initEcharts();
				// }).catch(error => {
				// 	console.error('接口调用出错：', error);
				// });




			},
			// 新增初始化echarts图表的方法
			initEcharts() {
				const myChart = echarts.init(document.getElementById('echartsContainers'));
				const option = {
					 
				tooltip: {
					trigger: 'axis',
					axisPointer: {
						type: 'shadow'
					}
				},
				legend: {},
				grid: {
					left: '3%',
					right: '4%',
					bottom: '17%',
					containLabel: true
				},
				xAxis: {
					type: 'value',
					boundaryGap: [0, 0.01]
				},
				yAxis: {
					type: 'category',
					data: ['A', 'B', 'C', 'D']
				},
				series: [
					{
						barWidth: '10px',
						type: 'bar',
						data: [18203, 23489, 29034, 104970],
						itemStyle: {
							color: '#1370FB'
						}
					}
				]
					// xAxis: {
					// 	type: 'category',
					// 	data: xAxisData
					// },
					// yAxis: {
					// 	type: 'value',


					// },
					// // 添加图例配置
					// legend: {
					// 	data: ['大蛋', '破蛋', '合格蛋', '次蛋'] // 这里填写每条线对应的名称，可根据实际含义修改
					// },
					// series: [{
					// 		type: 'line',
					// 		data: y1AxisData,
					// 		name: '大蛋'
					// 	},
					// 	{
					// 		type: 'line',
					// 		data: y2AxisData,
					// 		name: '破蛋'
					// 	},
					// 	{
					// 		type: 'line',
					// 		data: y3AxisData,
					// 		name: '合格蛋'
					// 	},
					// 	{
					// 		type: 'line',
					// 		data: y4AxisData,
					// 		name: '次蛋'
					// 	}
					// ]
				};
				myChart.setOption(option);
			},
			//轮询
			switper() {
				if (this.timer) {
					return
				}
				let looper = (a) => {
					this.getData()
				};
				this.timer = setInterval(looper, this.$store.state.setting.echartsAutoTime);
			},
		},
	};
</script>
<style lang='scss' scoped>
/* 	.user_Overview {
		div {
			flex: 1;
		}
	} */
</style>