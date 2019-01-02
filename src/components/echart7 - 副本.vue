<template>
  <div>
    <div id="echart7" ref="echart7" style="width: 1200px;height:1000px;"></div>
  </div>
</template>
<script>
export default {
  name: "echart7",
  data() {
    return {
      phup: [],
      phdown: [],
      colorlist:[]
    };
  },
  mounted() {
      var c = [1, 6, 9, 1, 5, 3, 5, 2, 4, 8, 9, 11, 12, 14, 3, 13];
    var a = [1, 6, 9, 1, 5, 3, 5, 2, 4, 8, 9, 11, 12, 14, 3, 13];
    var b = [1, 6, 9, 1, 5, 3, 5, 2, 4, 8, 9, 11, 12, 14, 3, 13];
    console.log(c);
     //分级别时，柱状图颜色数组处理过程
    var color1 = [];
    var color2 = [];
    var color3 = [];
    var color4 = [];
    var color5 = [];
      var color6 = [];
    var color7 = [];
    var color8 = [];
    var color9 = [];
    var color10 = [];
    for (var i = 0; i < c.length; i++) {
      if (c[i] <parseFloat(6.01)&& c[i] >parseFloat(0)|| c[i] == parseFloat(6.01)) {
        color1.push("#dfffa4");
      } else if ((parseFloat(6.01) < c[i] && c[i] <parseFloat(7)) || c[i] ==parseFloat(7)) {
        color2.push("#d9fb9e");
      } else if ((parseFloat(7)< c[i] && c[i] < parseFloat(7.09)) || c[i] ==parseFloat(7.09)) {
        color3.push("#f0ed77");
      } else if ((parseFloat(7.09) < c[i] && c[i] < parseFloat(7.17)) || c[i] == parseFloat(7.17)) {
        color4.push("#e4e872");
      } else if ((parseFloat(7.17) < c[i] && c[i] < parseFloat(7.24)) || c[i] ==parseFloat(7.24)) {
        color5.push("#a6d759");
      } else if ((parseFloat(7.24) < c[i] && c[i] < parseFloat(7.31)) || c[i] == parseFloat(7.31)) {
        color6.push("#82b442");
      } else if ((parseFloat(7.31) < c[i] && c[i] < parseFloat(7.39)) || c[i] == parseFloat(7.39)) {
        color7.push("#6aa52a");
      } else if ((parseFloat(7.39) < c[i] && c[i] < parseFloat(7.47)) || c[i] == parseFloat(7.47)) {
        color8.push("#437f1c");
      }else if ((parseFloat(7.47) < c[i] && c[i] < parseFloat(7.99)) || c[i] == parseFloat(7.99)) {
        color9.push("#346713");
      }else if (parseFloat(7.99) < c[i] || c[i] == parseFloat(14)) {
        color10.push("#285609");
      }
    }
    this.colorlist = color1
      .concat(color2)
      .concat(color3)
      .concat(color4)
      .concat(color5)
       .concat(color6)
      .concat(color7)
      .concat(color8)
      .concat(color9)
      .concat(color10);
    console.log(this.colorlist);
    for (var i = 0; i < a.length; i++) {
      if (a[i] >= 7) {
        a.splice(i, 1, 0);
      }
    }
    for (var i = 0; i < a.length; i++) {
      if (a[i] <= 7) {
        a.splice(i, 1, -a[i]);
      }
    }
    for (var j = 0; j < b.length; j++) {
      if (b[j] <= 7) {
        b.splice(j, 1, 0);
      }
    }
    for (var j = 0; j < b.length; j++) {
      if (b[j] >= 7) {
        b.splice(j, 1, b[j] - 7);
      }
    }
    this.phdown = a;
    this.phup = b;
    
    this.initChart();
  },

  methods: {
    initChart() {
          var cc = this.colorlist;
      this.chart = this.$echarts.init(this.$refs.echart7);
      this.chart.setOption({
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        xAxis: [
          {
            type: "value",
            show: false
            /* min:0,
                max:14 */
            /*             gridIndex:7 */
          }
        ],
        yAxis: [
          {
            type: "category",
            axisTick: {
              show: false
            },
            axisLine: {
              lineStyle: {
                type: "solid",
                color: "blue",
                width: "2"
              }
            },
            axisLabel: {
              textStyle: {
                color: "#000"
              }
            },
            data: [
              "一",
              "二",
              "三",
              "四",
              "五",
              "六",
              "七",
              "八",
              "九",
              "十",
              "十一",
              "十二",
              "十三",
              "十四",
              "十五",
              "十六"
            ]
          }
        ],
        series: [
          {
            name: "PH<7",
            type: "bar",
            stack: "总量",
            label: {
              normal: {
                show: true,
                position: "left"
              }
            },
            itemStyle: {
              normal: {
                   color: function(params) {
                  console.log(cc);
                  return cc[params.dataIndex];
                },
                label: {
                  show: false,
                  position: "top",
                  /* formatter: "{c}↑" */
                  formatter: function(re) {
                    if (Math.abs(re.data) == 0 || Math.abs(re.data) == 7) {
                      var ar = "";
                    } else {
                      var ar = Math.abs(re.data);
                    }

                    console.log(ar);
                    return ar;
                  }
                }
              }
            },
            data: this.phdown
          },
          {
            name: "PH>7",
            type: "bar",
            stack: "总量",
            label: {
              normal: {
                show: true,
                position: "right"
              }
            },
            data:this.phup,
            itemStyle: {
              normal: {
                   color: function(params) {
                  console.log(cc);
                  return cc[params.dataIndex];
                },
                label: {
                  show: false,
                  position: "top",
                  /* formatter: "{c}↑" */
                  formatter: function(re2) {
                    if (re2.data + 7 == 7) {
                      var ar2 = "";
                    } else {
                      var ar2 = Math.abs(re2.data + 7);
                    }

                    console.log(ar2);
                    return ar2;
                  }
                }
              }
            }
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
/* #echart7 {
  width: 1200px;
  height: 600px;
} */
</style>
