<template>
  <div id="outbound-stacked-bar-chart" style="width: 100%; height: 310px;"></div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  data() {
    return {
      baseList: [],
      eggTypeList: [],
      outboundData: []
    };
  },
  mounted() {
    this.initChart();
  },
  methods: {
    initChart() {
      const mockData = [
        // { base: '河南', eggType: '大蛋', quantity: 100 },
        // { base: '河南', eggType: '小蛋', quantity: 50 },
        // { base: '河南', eggType: '大蛋', quantity: 80 },
        // { base: '安徽', eggType: '次蛋', quantity: 120 },
        // { base: '安徽', eggType: '成品蛋', quantity: 60 },
        // { base: '贵州', eggType: '大蛋', quantity: 90 },
        // { base: '广西', eggType: '大蛋', quantity: 90 },
        // { base: '云南', eggType: '大蛋', quantity: 90 }
      ];

      // 处理数据
      // this.baseList = Array.from(new Set(mockData.map(item => item.base)));
      // this.eggTypeList = Array.from(new Set(mockData.map(item => item.eggType)));
      // this.outboundData = this.eggTypeList.map(eggType => {
      //   return {
      //     name: eggType,
      //     type: 'bar',
      //     stack: '总量',
      //     data: this.baseList.map(base => {
      //       const item = mockData.find(i => i.base === base && i.eggType === eggType);
      //       return item? item.quantity : 0;
      //     }),
      //     itemStyle: {
      //       color: this.getColor(eggType)
      //     }
      //   };
      // });

      const chartDom = document.getElementById('outbound-stacked-bar-chart');
      const myChart = echarts.init(chartDom);

      const option = {
        title: {
          text: 'Customized Pie',
          left: 'center',
          top: 15,
          textStyle: {
            color: '#ccc'
          }
        },
        tooltip: {
          trigger: 'item'
        },
        visualMap: {
          show: false,
          min: 80,
          max: 600,
          inRange: {
            colorLightness: [0, 1]
          }
        },
        series: [
          {
            name: 'Access From',
            type: 'pie',
            radius: '55%',
            center: ['50%', '45%'],
            data: [
              { value: 335, name: 'Direct' },
              { value: 310, name: 'Email' },
              { value: 274, name: 'Union Ads' },
              { value: 235, name: 'Video Ads' },
              { value: 400, name: 'Search Engine' }
            ].sort(function (a, b) {
              return a.value - b.value;
            }),
            roseType: 'radius',
            label: {
              color: 'rgba(255, 255, 255, 0.3)'
            },
            labelLine: {
              lineStyle: {
                color: 'rgba(255, 255, 255, 0.3)'
              },
              smooth: 0.2,
              length: 10,
              length2: 20
            },
            itemStyle: {
              color: '#c23531',
            },
            animationType: 'scale',
            animationEasing: 'elasticOut',
            animationDelay: function (idx) {
              return Math.random() * 200;
            }
          }
        ]
        // title: {
        //   // text: '出库统计'
        // },
        // tooltip: {
        //   trigger: 'axis',
        //   axisPointer: {
        //     type:'shadow'
        //   }
        // },
        // legend: {
        //   // data: this.eggTypeList
        // },
        // xAxis: {
        //   type: 'category',
        //   // data: this.baseList
        // },
        // yAxis: {
        //   type: 'value',
        //   // name: '出库数量'
        // },
        // series: this.outboundData
      };

      myChart.setOption(option);
    },
    getColor(eggType) {
      const colorMap = {
        '大蛋': '#0fcbff',
        '小蛋': '#ffff00',
        '次蛋': '#ff92ed',
        '成品蛋': '#59e174'
      };
      return colorMap[eggType] || '#808080';
    }
  }
};
</script>

<style scoped>
/* 可以添加自定义样式 */
</style>