<template>
  <div>
    <div id="echart4" ref="echart4" style="width: 1200px;height:300px;"></div>
  </div>
</template>
<script>
export default {
  name: "echart4",
  data() {
    return {
      chartData: [
        {
          name: "盐田水厂11111",
          children: [
            {
              name: "东海道2221",
              children: [
                {
                  name: "永安北泵房330"
                }
              ]
            },
            {
              name: "盐田高级中学2221"
            },
            {
              name: "盐田1号泵站2221"
            },
            {
              name: "南方明珠花园2221",
              children: [
                {
                  name: "金海雅居331",
                  children: [
                    {
                      name: "梅沙西泵站441",
                      children: [
                        {
                          name: "观景酒店550"
                        }
                      ]
                    }
                    /*  {
                  name: "第三级441"
                } */
                  ]
                },
                {
                  name: "大梅沙共同沟440",
                  children: [
                    {
                      name: "梅沙街道办441"
                    }
                  ]
                }
              ]
            },
            {
              name: "盐田四村2221"
            },
            {
              name: "盐田港人民医院2221"
            },
            {
              name: "北山道2221"
              /* children: [
            {
              name: "第三级331"
            },
            {
              name: "第三级331"
            }
          ] */
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
      this.chart = this.$echarts.init(this.$refs.echart4);
      this.chart.setOption({
          backgroundColor:"#000515",
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
                    arr = ["{ignornormal|" + hz + "}", "{abnormal|" + num + "}"];
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
              curveness: "0.8",
            width:"1"
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
/* #echart4 {
  width: 700px;
  height: 300px;
} */
</style>
