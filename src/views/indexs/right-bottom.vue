<template>
    <div class="center_bottom">
        <Echart
            :options="options"
            id="salesBarChart"
            class="echarts_bottom"
        ></Echart>
    </div>
</template>

<script>
import { currentGET } from "api";
import { graphic } from "echarts";
import {
    eventBus
} from '../../main.js';

export default {
    data() {
        return {
            clickedProvince: '',
            salesData: [
				{
					date:'1',
					online:1000,
					offline:1500
				},
				{
					date:'2',
					online:800,
					offline:1200
				},
				{
					date:'3',
					online:950,
					offline:1130
				},
				{
					date:'4',
					online:986,
					offline:1350
				},
				{
					date:'5',
					online:953,
					offline:1125
				},
				{
					date:'6',
					online:1212,
					offline:1100
				},
			],
            options: {}
        };
    },
    props: {},
    mounted() {
        eventBus.$on('map-province-clicked', (params) => {
            console.log(params.name + "阿萨");
            this.clickedProvince = params.name;
            this.getData();
        });
        eventBus.$on('get-all-data', () => {
            this.clickedProvince = '';
            this.getData();
        });
        this.getData();
    },
    methods: {
        getData() {
            const param = { area: this.clickedProvince };
            // currentGET("reportSales", param).then((res) => {
            //     console.log("销售数据", res);
            //     if (res.code === 200) {
            //         // this.salesData = res.data;
			// 		this.salesData = this.salesData;
                    this.init();
            //     } else {
            //         this.$Message({
            //             text: res.msg,
            //             type: "warning"
            //         });
            //     }
            // });
        },
        init() {
			
            const dates = this.salesData.map(data => data.date);
            const onlineSales = this.salesData.map(data => data.online);
            const offlineSales = this.salesData.map(data => data.offline);

            this.options = {
                tooltip: {
                    trigger: 'axis',
                    backgroundColor: 'rgba(0, 0, 0, 0.6)',
                    borderColor: 'rgba(147, 235, 248, 0.8)',
                    textStyle: {
                        color: '#FFF'
                    },
                    formatter: function (params) {
                        var result = params[0].name + '<br>';
                        params.forEach(function (item) {
                            result += item.marker + ' ' + item.seriesName + ' : ' + item.value + ' 个<br>';
                        });
                        return result;
                    }
                },
                legend: {
                    data: ['线上', '线下'],
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
                xAxis: {
                    type: 'category',
                    data: dates,
                    axisLine: {
                        lineStyle: {
                            color: '#B4B4B4'
                        }
                    },
                    axisTick: {
                        show: false
                    }
                },
                yAxis: {
                    splitLine: { show: false },
                    axisLine: {
                        lineStyle: {
                            color: '#B4B4B4'
                        }
                    },
                    axisLabel: {
                        formatter: '{value}'
                    }
                },
                series: [
                    {
                        name: '线上',
                        type: 'bar',
                        barWidth: 20,
                        itemStyle: {
                            borderRadius: 5,
                            color: new graphic.LinearGradient(0, 0, 0, 1, [
                                { offset: 0, color: '#956FD4' },
                                { offset: 1, color: '#3EACE5' }
                            ])
                        },
                        data: onlineSales
                    },
                    {
                        name: '线下',
                        type: 'bar',
                        barWidth: 20,
                        itemStyle: {
                            borderRadius: 5,
                            color: new graphic.LinearGradient(0, 0, 0, 1, [
                                { offset: 0, color: 'rgba(156,107,211,0.8)' },
                                { offset: 0.2, color: 'rgba(156,107,211,0.5)' },
                                { offset: 1, color: 'rgba(156,107,211,0.2)' }
                            ])
                        },
                        data: offlineSales
                    }
                ]
            };
        }
    }
};
</script>

<style lang="scss" scoped>
.center_bottom {
    width: 100%;
    height: 100%;

  .echarts_bottom {
        width: 100%;
        height: 100%;
    }
}
</style>