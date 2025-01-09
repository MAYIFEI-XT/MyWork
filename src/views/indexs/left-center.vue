<!--
 * @Author: daidai
 * @Date: 2022-03-01 15:51:43
 * @LastEditors: Please set LastEditors
 * @LastEditTime: 2022-09-29 15:12:46
 * @FilePath: \web-pc\src\pages\big-screen\view\indexs\right-bottom.vue
-->
<template>
  <div id="right_bottom">
    <!-- <dv-capsule-chart :config="config" style="width:100%;height:260px" /> -->
	<!-- <dv-scroll-ranking-board :config="config" style="width:100%;height:260px" /> -->
  <Echart
      :options="options"
      id="salesBarChart"
  ></Echart>
  </div>
</template>

<script>
import * as echarts from 'echarts';
import { currentGET } from 'api/modules'
import {
		eventBus
	} from '../../main.js';
export default {
  data() {
    return {
      gatewayno: '',
	  clickedProvince: '',
      config: {
        showValue: true,
        unit: "",
        data: []
      },
      options:{}
    };
  },
  created() {


  },
  computed: {
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
				console.log("在jam手机");
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
    //轮询
    // switper() {
    //   if (this.timer) {
    //     return
    //   }
    //   let looper = (a) => {
    //     this.getData()
    //   };
    //   this.timer = setInterval(looper, this.$store.state.setting.echartsAutoTime);
    // },
    
    
    getData() {
      this.pageflag = true
      this.init()
      // this.pageflag =false
	//  const param = {area:this.clickedProvince};
    //   currentGET('reportrk', param).then(res => {

    //     if (!this.timer) {
    //       console.log('入库排名', res);
    //     }
    //     if (res.code == 200 ) {

						
		// 				this.config = {
		// 				    showValue: true,
		// 				    unit: "",
		// 				    data: []
		// 				};
    //    const convertedData = res.rows.map(item => ({
    //        name: item.area, 
    //        value: item.qty
    //    }));
    //    this.config.data = convertedData;
	  //  console.log(this.config);
          // this.switper()
    //     } else {
    //       this.pageflag = false
    //       this.srcList = []
    //       this.$Message({
    //         text: res.msg,
    //         type: 'warning'
    //       })
    //     }
    //   })
    },
    init() {
      var chartDom = document.getElementById('right_bottom');
      var myChart = echarts.init(chartDom);

      const options = {
        tooltip: {
          trigger: 'axis',
          backgroundColor: 'rgba(0, 0, 0, 0.6)',
          borderColor: 'rgba(147, 235, 248, 0.8)',
          textStyle: {
            color: '#FFF'
          },
          formatter: function (params) {
            return params[0].name + '<br>' + params[0].seriesName + ' : ' + params[0].value + '%';
          }
        },
        xAxis: {
          type: 'category',
          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        },
        yAxis: {
          type: 'value'
        },
        series: [
          {
            data: [120, 180, 150, 180, 70, 110, 130],
            type: 'bar'
          }
        ],
        grid:{
          bottom:30,
          top:20
        }
      };
      myChart.setOption(options);
    }
  },
};
</script>
<style lang='scss' scoped>
#right_bottom{
  width: 100%;
  height: 100%;
}
.list_Wrap {
  height: 100%;
  overflow: hidden;
  :deep(.kong)   {
    width: auto;
  }
}
.echarts_bottom {
        width: 100%;
        height: 100%;
    }
.sbtxSwiperclass {
  .img_wrap {
    overflow-x: auto;
  }

}

.right_bottom {
  box-sizing: border-box;
  padding: 0 16px;

  .searchform {
    height: 80px;
    display: flex;
    align-items: center;
    justify-content: center;

    .searchform_item {
      display: flex;
      justify-content: center;
      align-items: center;

      label {
        margin-right: 10px;
        color: rgba(255, 255, 255, 0.8);
      }

      button {
        margin-left: 30px;
      }

      input {}
    }
  }

  .img_wrap {
    display: flex;
    // justify-content: space-around;
    box-sizing: border-box;
    padding: 0 0 20px;
    // overflow-x: auto;

    li {
      width: 105px;
      height: 137px;
      border-radius: 6px;
      overflow: hidden;
      cursor: pointer;
      // background: #84ccc9;
      // border: 1px solid #ffffff;
      overflow: hidden;
      flex-shrink: 0;
      margin: 0 10px;

      img {
        flex-shrink: 0;
      }
    }




  }

  .noData {
    width: 100%;
    line-height: 100px;
    text-align: center;
    color: rgb(129, 128, 128);

  }
}
</style>
