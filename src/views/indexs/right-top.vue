



<!--
 * @Author: daidai
 * @Date: 2022-02-28 16:16:42
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-10-25 09:18:22
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\left-center.vue
-->
<template>
  <Echart id="leftCenter" :options="options" class="left_center_inner" v-if="pageflag" ref="charts" />
  <Reacquire v-else @onclick="getData" style="line-height:200px">
    重新获取
  </Reacquire>
</template>

<script>
import { currentGET } from 'api/modules'
import { toRaw } from 'vue';
import {
		eventBus
	} from '../../main.js';
export default {
  data() {
    return {
		clickedProvince: '',
		newData:[
			
		],
      options: {},
      pageflag: true,
      timer: null
    };
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
	    this.getData()
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
		console.log('开始调取接口');
            // this.init();
			//  const param = {area:this.clickedProvince};// 根据接口实际需要传入相应参数，此处示例为空对象
			//       currentGET('reportdpzb', param).then(response => {
			//         // 假设接口返回的数据格式和之前本地定义的newData类似，是一个数组对象形式
			//         // 这里根据实际接口返回数据结构进行相应调整处理
			//         const newDataFromApi = response.rows; // 这里假设接口返回的数据在data属性下，按需修改
			// 		this.newData = newDataFromApi;
			// 		console.log(this.newData);
			        this.init();
			      // }).catch(error => {
			      //   console.error('接口调用出错：', error);
			      // });
			  
  
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
      let myChart = this.$refs.charts.chart
      myChart.on('mouseover', params => {
        this.clearData()
      });
      myChart.on('mouseout', params => {
        this.timer = setInterval(looper, this.$store.state.setting.echartsAutoTime);
      });
    },

init() {
    // 计算总数，这里根据newData中的value字段来计算总和，作为总数
    // let total = this.newData.reduce((acc, item) => acc + item.value, 0);
	
    // let piedata = {
    //     name: "蛋品库存占比",
    //     type: "pie",
    //     radius: ["42%", "65%"],
    //     data: this.newData.map(item => ({
    //         value: item.value,
    //         name: item.name
    //     }))
    // };
    this.options = {
        grid: {
          height: 200,
          bottom: 40
        },
        // title: {
        //     text: "蛋品占比",
        //     left: "center",
        //     textStyle: {
        //         rich: {
        //             value: {
        //                 color: "#ffffff",
        //                 fontSize: 24,
        //                 fontWeight: "bold",
        //                 lineHeight: 20,
        //             },
        //             name: {
        //                 color: "#ffffff",
        //                 lineHeight: 20,
        //             },
        //         }
        //     }
        // },
        tooltip: {
            trigger: "item",
            backgroundColor: "rgba(0,0,0,.6)",
            borderColor: "rgba(147, 235, 248,.8)",
            textStyle: {
                color: "#FFF"
            }
        },
        // series: [
        //     {
        //       ...piedata,
        //         label: {
        //             formatter: "   {b|{b}}   \n   {c|{c}个}   {per|{d}%}  ",
        //             //   position: "outside",
        //             rich: {
        //                 b: {
        //                     color: "#fff",
        //                     fontSize: 12,
        //                     lineHeight: 26,
        //                 },
        //                 c: {
        //                     color: "#31ABE3",
        //                     fontSize: 14,
        //                 },
        //                 per: {
        //                     color: "#31ABE3",
        //                     fontSize: 14,
        //                 }
        //             }
        //         }
        //     }
        // ]
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: [120, 200, 150, 80, 70, 110, 130],
            type: 'bar'
          }
        ]
    };
}
  },
};
</script>
<style lang='scss' scoped>
</style>