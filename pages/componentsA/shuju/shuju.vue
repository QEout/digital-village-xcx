<template>
  <view class="p-8 flex-col">
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart1" :option="option" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart2" :option="option2" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart3" :option="option3" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart4" :option="option4" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart5" :option="option5" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart6" :option="option6" />
    </view>
    <view class="card">
      <echarts-uniapp ref="echarts" canvasId="chart7" :option="option7" />
    </view>
  </view>
</template>

<script>
  import echartsUniapp from "../components/echarts-uniapp/echarts-uniapp.vue"
  export default {
    components: { echartsUniapp },
    data() {
      return {
        option:{},
        option2:{},
        option3:{},
        option4:{},
        option5:{},
        option6:{},
        option7:{},
        chart_fwsj: {
          title: {
            text: "房屋数据概况"
          },
          grid: {
            left: "0",
            right: "0",
            bottom: "0",
            containLabel: true
          },
          xAxis: { data: ["自住", "出租", "空置"] },
          yAxis: {},
          series: [
            {
              name: "房屋数据概况",
              type: "bar",
              data: [75, 20, 6]
            }
          ]
        },
           chart_clsj: {
        title: {
          text: "车辆数据概况"
        },
                grid: {
          left: "0",
          right: "0",
          bottom: "0",
          containLabel: true
        },
        xAxis: { data: ["车辆总数", "车位总数", "车辆入库"] },
        yAxis: {},
        series: [
          {
            name: "车辆数据概况",
            type: "bar",
            data: [60, 20, 10]
          }
        ]
      },
      chart_rksj: {
        title: {
          text: "人口数据概况"
        },
        tooltip: {
          trigger: "item"
        },
                grid: {
          left: "0",
          right: "0",
          bottom: "0",
          containLabel: true
        },
        series: [
          {
            name: "男女比例",
            radius: ["40%", "70%"],
            type: "pie",

            data: [
              { value: 50, name: "男" },
              { value: 50, name: "女" }
            ],
            label: {
              show: true,
              position: "inside",
              formatter: "{b}:{d}%"
            },
            right: "50%"
          },
          {
            name: "常住和暂住",
            type: "pie",
            radius: ["40%", "70%"],
            data: [
              { value: 70, name: "常住" },
              { value: 30, name: "暂住" }
            ],
            label: {
              show: true,
              position: "inside",
              formatter: "{b}:{d}%"
            },
            left: "50%"
          }
        ]
      },
      chart_rkhj: {
        title: { text: "人口户籍统计" },
        yAxis: {},
                grid: {
          left: "0",
          right: "0",
          bottom: "0",
          containLabel: true
        },
        xAxis: {
          data: ["其他", "农业", "非农业", "无户口"]
        },
        series: [
          {
            name: "人口户籍统计",
            type: "bar",
            data: [10, 50, 30, 20]
          }
        ]
      },
      chart_rkjg: {
        title: { text: "人口结构分布" },
        yAxis: {},
                grid: {
          left: "0",
          right: "0",
          bottom: "0",
          containLabel: true
        },
        xAxis: {
          data: ["0-19", "20-29", "30-39", "40-49", "50-59", "60+"]
        },
        series: [
          {
            name: "人口结构分布",
            type: "bar",
            data: [10, 18, 30, 20, 15, 7]
          }
        ]
      },
      chart_zzmm: {
        title: { text: "政治面貌统计" },
        xAxis: {
          type: "value"
        },
        grid: {
              left: "3%",
          right: "3%",
          bottom: "0",
          containLabel: true
        },
        yAxis: {
          type: "category",
          data: ["中共党员", "中共预备党员", "团员", "群众", "其他"]
        },
        series: [
          {
            name: "政治面貌统计",
            type: "bar",
            data: [10, 2, 8, 50, 10]
          }
        ]
      },
      chart_zjxy: {
        title: { text: "宗教信仰统计" },
        xAxis: {
          type: "value"
        },
                grid: {
          left: "3%",
          right: "3%",
          bottom: "0",
          containLabel: true
        },
        yAxis: {
          type: "category",
          data: ["佛教", "道教", "基督教", "伊斯兰教", "其他"]
        },
        series: [
          {
            name: "宗教信仰统计",
            type: "bar",
            data: [10, 2, 8, 50, 10]
          }
        ]
      }
      }
    },
    onReady() {
        this.option = this.chart_fwsj
        this.option2 = this.chart_clsj
        this.option3 = this.chart_rksj
        this.option4 = this.chart_rkhj
        this.option5 = this.chart_rkjg
        this.option6 = this.chart_zzmm
        this.option7 = this.chart_zjxy
    },
    methods: {}
  }
</script>

<style scoped>
  .card {
    padding: 16rpx 20rpx;
    margin-bottom: 16rpx;
    background-color: #fff;
    border-radius: 8rpx;
    box-shadow: 0 0 10rpx rgba(0, 0, 0, 0.1);
    height: 400rpx;
  }
</style>
