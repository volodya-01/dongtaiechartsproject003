<template>
  <div>
    <div id="echart6" ref="echart6" style="width: 1200px;height:1000px;backgroundColor:#000"></div>
  </div>
</template>
<script>
export default {
  name: "echart6",
  data() {
    return {
      b: [], //柱状图数组
      /*  c: [], */ colorlist: [], //柱状图颜色数组
      numstatus: [], //报警状态数组
      adress: [] //名称标签
    };
  },
  mounted() {
    var that = this;
    that.api();
   /*  setInterval(function() {
      that.api();
    }, 1000) */;
  },

  methods: {
    api() {
      var that = this;
      
      var adr = [
        "石家庄市0",
        "张家口市0",
        "承德市1",
        "秦皇岛市0",
        "唐山市1",
        "廊坊市1",
        "保定市1",
        "沧州市0",
        "衡水市-1",
        "邢台市-1",
        "邯郸市-1",
        "定州市0",
        "辛集市1"
      ];
      var staus = [0, 0, 1, 0, 1, -1, 1, 0, 1, -1, -1, 0, 1];
      that.adress = adr;
      that.numstatus = staus;
      var c = [];
      var a = [];
      for (var i = 0; i < 13; i++) {
        a.push(Math.random().toFixed(3));
      }
       for (var i = 0; i < a.length; i++) {
        c.push(a[i]);
      }
      that.b = c;
      console.log(c);
      //分级别时，柱状图颜色数组处理过程

      /* for (var i = 0; i < a.length; i++) {
      if (a[i] < 3 || a[i] == 3) {
        a.splice(i, 1, "#000");
      } else if ((3 < a[i] && a[i] < 6) || a[i] == 6) {
       a.splice(i, 1,"orange");
      } else if ((6 < a[i] && a[i] < 9) || a[i] == 9) {
       a.splice(i, 1,"yellow");
      } else if ((9 < a[i] && a[i] < 13) || a[i] == 13) {
       a.splice(i, 1,"green");
      } else if (13 < a[i] || a[i] == 13) {
       a.splice(i, 1,"blue");
       
      }
    } */
      for (var i = 0; i < a.length; i++) {
        if (a[i] < 0.131 || a[i] == 0.131) {
          a.splice(i, 1, "#054d94");
        } else if ((0.131 < a[i] && a[i] < 0.383) || a[i] == 0.383) {
          a.splice(i, 1, "#055599");
        } else if ((0.383 < a[i] && a[i] < 0.458) || a[i] == 0.458) {
          a.splice(i, 1, "#0465a5");
        } else if ((0.458 < a[i] && a[i] < 0.522) || a[i] == 0.522) {
          a.splice(i, 1, "#047db5");
        } else if ((0.522 < a[i] && a[i] < 0.581) || a[i] == 0.581) {
          a.splice(i, 1, "#0297ce");
        } else if ((0.581 < a[i] && a[i] < 0.642) || a[i] == 0.642) {
          a.splice(i, 1, "#02b6dd");
        } else if ((0.642 < a[i] && a[i] < 0.707) || a[i] == 0.707) {
          a.splice(i, 1, "#01cdec");
        } else if ((0.707 < a[i] && a[i] < 0.789) || a[i] == 0.789) {
          a.splice(i, 1, "#01d5f2");
        } else if ((0.789 < a[i] && a[i] < 1.398) || a[i] == 1.398) {
          a.splice(i, 1, "#00ddf7");
        } else if (1.398 < a[i]) {
          a.splice(i, 1, "#00e4fc");
        }
      }
      that.colorlist = a;

      console.log(this.colorlist);
      that.initChart();
    },
    initChart() {
      //报警时，柱状图颜色数组再次处理过程
      console.log(this.b);
      var cc = this.colorlist;
      var bbb = this.b;
      var bj = []; //报警时，辅助柱状图显示数组
      var dstatus = this.numstatus;
      for (var i = 0; i < cc.length; i++) {
        bj.push(0);
        if (dstatus[i] != 0) {
          cc.splice(i, 1, "red");
          console.log(cc);
          bj.splice(i, 1, 2);
        }
      }
      this.chart = this.$echarts.init(this.$refs.echart6);
      this.chart.setOption({
        /*  color: colorList, */
        tooltip: {
          show: true,
          trigger: "item",
          formatter: "{c}"
        },
        toolbox: {
          show: true,
          feature: {
            mark: {
              show: true
            }
          }
        },
        grid: {
          left: "2%",
          right: "10%",
          bottom: "3%",
          height: "90%",
          width: "80%",
          containLabel: true
        },
        yAxis: [
          {
            type: "category",
            nameGap: "25",
            data: this.adress,
            axisLine: {
              lineStyle: {
                type: "solid",
                color: "#28316d", //左边线的颜色
                width: "1" //坐标线的宽度
              }
            },
            axisLabel: {
              interval: 0,
              /*   rotate: 40, */
              show: true,
              splitNumber: 15,

              textStyle: {
                //fontFamily: "微软雅黑",
                fontSize: 10
              },
              //名称标签报警及正常状态处理过程
              formatter: function(dat, w) {
                console.log(dat);
                console.log(w);
                console.log(dstatus);
                console.log(bbb);
                var reg = /[\u4e00-\u9fa5]/g;
                var names = dat.match(reg);
                var hz = names.join("") + ":";
                var aa = dat.charAt(dat.length - 1);
                var arr = [];
                if (aa == "0") {
                  arr = ["{normal|" + hz + "}"];
                } else if (dat == "辛集市1") {
                  arr = ["{makepoint|" + hz + "}"];
                } else if (aa == "1" || aa == "-1") {
                  arr = ["{abnormal|" + hz + "}"];
                }
                return arr;
              },

              rich: {
                normal: {
                  color: "green",
                  align: "center"
                },
                abnormal: {
                  color: "#fff",
                  backgroundColor: "red",
                  width: "8",
                  height: "20",
                  align: "center"
                },
                makepoint: {
                  color: "#000",
                  backgroundColor: "yellow",
                  width: "8",
                  height: "20",
                  align: "center"
                }
              }
            },

            axisTick: {
              alignWithLabel: true
            }
          },
          {
            axisTick: "none",
            axisLine: "none"
            /*   data: ['12566','12566','12566','12566','12566','12566'], */
          }
        ],
        xAxis: [
          {
            type: "value",
            name: "",
            splitLine: {
              //分割线
              show: true,
              // color:"#fff",
              lineStyle: {
                color: "#28316d"
              }
            },
            axisLabel: {
              interval: 0,
              rotate: 0,
              show: true,
              splitNumber: 30,
              // color:"#fff",
              textStyle: {
                //fontFamily: "微软雅黑",
                fontSize: 12
              }
            }
          }
        ],
        series: [
          {
            name: "",
            type: "bar",
            barWidth: 30, //柱图宽度
            data: this.b,
            label: {
              normal: {
                show: true,
                position: "right",
                color: "#000",
                width: "60",
                height: "60",
                backgroundColor: {
                  image: "../assets/1.png"

                  // 这里可以是图片的 URL，
                  // 或者图片的 dataURI，
                  // 或者 HTMLImageElement 对象，
                  // 或者 HTMLCanvasElement 对象。
                }
              }
            },

            itemStyle: {
              normal: {
                opacity: 1,
                //设置柱状图颜色
                color: function(params) {
                  console.log(cc);
                  return cc[params.dataIndex];
                },
                //设置柱状图顶端标签数据处理显示
                label: {
                  show: false,
                  position: "top",
                  /*  formatter: "{c}↑" */
                  formatter: function(sdat) {
                    console.log(sdat.data);
                    console.log(sdat.dataIndex);
                    console.log(dstatus);
                    var sda = sdat.data;
                    for (var i = 0; i < dstatus.length; i++) {
                      if (dstatus[sdat.dataIndex] == 0) {
                        var arr = ["{normal|" + sda + "" + "}"];
                      } else if (dstatus[sdat.dataIndex] == 1) {
                        var arr = ["{up|" + sda + "↑" + "}"];
                      } else if (dstatus[sdat.dataIndex] == -1) {
                        var arr = ["{down|" + sda + "↓" + "}"];
                      }
                    }
                    return arr;
                  },
                  rich: {
                    normal: {
                      color: "green",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      padding: [22, -30],
                      letterSpace: "6"
                    },
                    up: {
                      color: "red",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      padding: [22, -30],
                      letterSpace: "6"
                    },
                    down: {
                      color: "blue",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      padding: [22, -30],
                      letterSpace: "6"
                    }
                  }
                }
              }
            }
          },
          {
            name: "",
            type: "bar",

            barWidth: 42, //柱图宽度
            data: bj,
            barGap: "-120%",
            barCategoryGap: "25%",
            itemStyle: {
              normal: {
                color: "rgba(255,0,0)",
                opacity: 0.5
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
/* #echart6 {
  width: 1200px;
  height: 600px;
} */
</style>
