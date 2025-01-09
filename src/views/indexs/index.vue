<!--
 * @Author: daidai
 * @Date: 2022-03-04 09:23:59
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-05-07 11:05:02
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\index.vue
-->
<template>
	<div class="contents">
		<div class="contetn_left">
			<div class="pagetab">
				<!-- <div class="item">实时监测</div> -->

			</div>
			<ItemWrap class="contetn_left-top contetn_lr-item" title="饲料1">
				<LeftTop />

			</ItemWrap>
			<ItemWrap class="contetn_left-center contetn_lr-item" title="饲料2">
				<LeftCenter />
			</ItemWrap>
			<ItemWrap class="contetn_left-bottom contetn_lr-item" title="饲料3" style="padding: 0 10px 16px 10px">
				<LeftBottom />
			</ItemWrap>
		</div>
		<div class="contetn_center" v-if="showMap">
			<CenterMap class="contetn_center_top" />

			<ItemWrap class="contetn_center-bottom" title="饲料4">
				<CenterBottom />

			</ItemWrap>
		</div>
		<div class="contetn_center" v-if="showMp4">

			<VideoComponent :videoUrl="mp4Url" />
			<ItemWrap class="contetn_center-bottom" title="出库分析">
				<CenterBottom />

			</ItemWrap>
		</div>
		<div class="contetn_right">
			<ItemWrap class="contetn_left-bottom contetn_lr-item" title="饲料5">
				<RightTop />
			</ItemWrap>
			<ItemWrap class="contetn_left-bottom contetn_lr-item" title="饲料6" >
				<RightCenter/>
				<!-- <RightTop /> -->
			</ItemWrap>
			<ItemWrap class="contetn_left-bottom contetn_lr-item" title="饲料7 ">
				<RightBottom />
			</ItemWrap>
		</div>
	</div>
</template>

<script>
	import LeftTop from './left-top.vue'
	import {
		currentGET
	} from 'api/modules'
	import LeftCenter from "./left-center.vue";
	import LeftBottom from "./left-bottom.vue";
	import CenterMap from "./center-map.vue";
	import CenterBottom from "./center-bottom.vue";
	import RightTop from "./right-top.vue";
	import RightCenter from "./right-center.vue";
	import RightBottom from "./right-bottom.vue";
	import {
		eventBus
	} from '../../main.js';
	export default {
		components: {
			LeftTop,
			LeftCenter,
			LeftBottom,
			CenterMap,
			RightTop,
			RightCenter,
			RightBottom,
			CenterBottom,
		},
		data() {
			return {
				showMap: true,
				showMp4: false,
				clickedProvince: '',
				mp4Url: '',

			};
		},
		filters: {
			numsFilter(msg) {
				return msg || 0;
			},
		},
		created() {},

		mounted() {
			eventBus.$on('map-province-clicked', (params) => {
				console.log(params.name + "主页");
				// 根据接收到的参数更新数据，这里简单将省份名称赋值给clickedProvince，实际可按需处理
				if (params.name != '中国') {
					this.showMp4 = true;
					this.showMap = false;
				}
				this.clickedProvince = params.name;
				this.getData();

			});
			eventBus.$on('get-all-data', () => {
				// 调用获取并展示全部数据的方法（需根据实际需求自行实现该方法内具体的数据获取、页面更新等逻辑）
				this.clickedProvince = ''
				this.showMp4 = false;
				this.showMap = true;

			});
			 eventBus.$on('hide-video-and-show-map', () => {
				 this.clickedProvince = ''
			            this.showMap = true;
			            this.showMp4 = false;
						
			        });

		},
		methods: {

		getData() {
		            let param = { area: this.clickedProvince };
		            currentGET('getMp4Url', param).then(response => {
		                if (response.code === 200 && Array.isArray(response.rows) && response.rows.length > 0 ) {
		                    this.mp4Url = response.rows[0].mp4Url;
							console.log(this.mp4Url);
		                } else {
		                    console.error('接口返回数据格式不正确，无法获取mp4url');
		                }
		            }).catch(error => {
		                console.error('接口调用出错：', error);
		            });
		        }
		},
	};
</script>
<style lang="scss" scoped>
	// 内容
	.contents {

		.contetn_left,
		.contetn_right {
			width: 540px;
			box-sizing: border-box;
			// padding: 16px 0;
		}

		.contetn_center {
			display: grid;
			grid-template-rows: auto 330px;
			width: 720px;
		}


		//左右两侧 三个块
		.contetn_lr-item {
			height: 310px;
		}

		.contetn_center_top {
			width: 100%;
		}

		// 中间
		.contetn_center {
			display: flex;
			flex-direction: column;
			justify-content: space-around;
		}

		.contetn_center-bottom {
			height: 315px;
		}

		//左边 右边 结构一样
		.contetn_left,
		.contetn_right {
			display: flex;
			flex-direction: column;
			justify-content: space-around;
			position: relative;


		}
	}


	@keyframes rotating {
		0% {
			-webkit-transform: rotate(0) scale(1);
			transform: rotate(0) scale(1);
		}

		50% {
			-webkit-transform: rotate(180deg) scale(1.1);
			transform: rotate(180deg) scale(1.1);
		}

		100% {
			-webkit-transform: rotate(360deg) scale(1);
			transform: rotate(360deg) scale(1);
		}
	}
</style>