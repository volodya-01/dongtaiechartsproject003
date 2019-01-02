<template>
  <div>
    <div id="echart5" ref="echart5" style="width: 1200px;height:300px;"></div>
  </div>
</template>
<script>
export default {
  name: "echart5",
  data() {
    return {
      chartData: [
        {
          name: "沙头角水厂11111",
          children: [
            {
              name: "梧桐苑2221"
            },
            {
              name: "盐田管网所2221",
              children: [
                {
                  name: "盐田党校330"
                }
              ]
            },
            {
              name: "边检生活小区2221"
            },
            {
              name: "山泉小区2221",
              children: [
                {
                  name: "沙头角街道办330",
                  children: [
                    {
                      name: "桥东社康330"
                    },
                    {
                      name: "盐田区政府330"
                    }
                  ]
                },
                  {
                  name: "黄金珠宝大厦2221"
                },
                {
                  name: "海荣居2221"
                },
                {
                  name: "鹏湾一村2221",
                  children: [
                    {
                      name: "盐田市场监督局330",
                      children: [
                        {
                          name: "海滨制药厂330"
                        }
                      ]
                    }
                  ]
                },
                {
                  name: "鹏湾二村2221",
                  children: [
                    {
                      name: "海涛花园330",
                      children: [
                        {
                          name: "东埔海景二期330"
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ]
    };
  },
  mounted() {
    this.initChart();
  },

  methods: {
    initChart() {
      this.chart = this.$echarts.init(this.$refs.echart5);
      this.chart.setOption({
        backgroundColor: "#000515",
        series: [
          {
            type: "tree",
            name: "tree2",
            data: this.chartData,
            top: "10%",
            bottom: "10%",
            right: "25%",
            height: "230",
            left: "7%",
            symbolSize: 8,
            /*  symbol: "circle", */
            // 展开发的层级数
            initialTreeDepth: 10,
            itemStyle: {
              color: "#515b67",
              borderColor: "#0098ff"
            },
            label: {
              normal: {
                position: "bottom",
                verticalAlign: "left",
                align: "center",
                fontSize: 9,
                distance: -10,
                /*  padding: [10, 0, 5, 0], */
                padding: [10, 0, 0, 0],
                formatter: function(dat) {
                  console.log(dat.name);
                  var num = dat.name.replace(/[^0-9]/gi, "");
                  num = num.substring(0, num.length - 1);
                  var reg = /[\u4e00-\u9fa5]/g;
                  var names = dat.name.match(reg);
                  var hz = names.join("") + ":";
                  var aa = dat.name.charAt(dat.name.length - 1);
                  var arr = [];
                  if (aa < 1) {
                    arr = ["{ignornormal|" + hz + "}", "{normal|" + num + "}"];
                  } else {
                    arr = [
                      "{ignornormal|" + hz + "}",
                      "{abnormal|" + num + "}"
                    ];
                  }
                  /* return arr.join("\n"); */
                  return arr;
                },

                rich: {
                  ignornormal: {
                    color: "#d0d9e8",
                    fontSize: 12,
                    padding: [0, 5, 0, 0]
                  },
                  normal: {
                    color: "#d0d9e8",
                    align: "center"
                  },
                  abnormal: {
                    color: "yellow",
                    backgroundColor: "rgba(245,48,8,0.5)",
                    width: "3",
                    height: "14",
                    align: "center"
                  }
                }
              }
            },
            // 线的样式
            lineStyle: {
              color: "#0098ff",
              curveness: "0.5",
              width: "1"
            },
            leaves: {
              label: {
                normal: {
                  // 叶子节点字的样式
                  /*  position: 'left',
                          verticalAlign: 'top',
                          align: 'left', */
                  position: "right",
                  verticalAlign: "middle",
                  align: "left",
                  fontSize: 12,
                  distance: 10,
                  padding: [0, 5, 0, 0]
                }
              }
            },
            expandAndCollapse: true,
            animationDuration: 550,
            animationDurationUpdate: 750
          }
        ]
      });
    },
    hidePopoverPanel() {
      this.popoverPanelShow = false;
    }
  }
};
</script>
<style scoped>
/* #echart5 {
  width: 1200px;
  height: 600px;
} */
</style>
