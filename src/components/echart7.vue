<template>
  <div>
    <div id="echart7" ref="echart7" style="width: 530px;height:850px;backgroundColor:#000"></div>
  </div>
</template>
<script>
export default {
  name: "echart7",
  data() {
    return {
      phup: [],//ph>7的柱状图数组
      phdown: [],//ph<7的柱状图数组
      colorlist1: [],//ph<7的柱状图颜色数组
      colorlist2: [],//ph>7的柱状图颜色数组
      adressdata: [],//柱状图标签名称数组
      statusdata: [],//柱状图状态码数组（0，-1,1）
      maxdat:""//柱状图数组的最大值
    };
  },
  mounted() {
var that=this
that.api2()
setInterval(function(){
that.api2()
},300000)
  },

  methods: {
    api2(){
        var that = this;
      that.$axios
        .post("http://112.64.170.158:9117/Service1.svc/PipePHTargetData")
        .then(res => {
          var that = this;
          var adr = [];
          var staus = [];
          var a = [];
          var c = [];
          var maxdata=res.data.CurrentMaxData
          console.log(res);
          for (var i = 0; i < res.data.CurveDataList.length; i++) {
            adr.push(res.data.CurveDataList[i].PointName);
            staus.push(res.data.CurveDataList[i].WarningType);
            a.push(res.data.CurveDataList[i].Data);
          }
          for (var i = 0; i < a.length; i++) {
            c.push(a[i]);
          }
          that.b = c;
        that.adressdata = adr;
          that.statusdata = staus;
          that.maxdat=maxdata
    //复制一份获取的全部ph数据存在b数组内
    var b = [];
    for (var i = 0; i < a.length; i++) {
      b.push(a[i]);
    }
    //处理a数组内的数据符合ph<7的数据保留，其余的用""替换
    for (var i = 0; i < a.length; i++) {
      if (a[i] >= 7) {
        a.splice(i, 1, "");
      }
    }
    for (var i = 0; i < a.length; i++) {
      if (a[i] <= 7) {
        a.splice(i, 1, -a[i]);
      }
    }
    //处理b数组内的数据符合ph>7的数据保留，其余的用""替换
    for (var j = 0; j < b.length; j++) {
      if (b[j] <= 7) {
        b.splice(j, 1, "");
      }
    }
    for (var j = 0; j < b.length; j++) {
      if (b[j] >= 7) {
        b.splice(j, 1, b[j] - 7);
      }
    }
    //把处理后的a、b数组赋值给data中的变量phup、phdown
    this.phdown = a;
    this.phup = b;
    //分别复制一份this.phdown、this.phup数组存在upcolordata、downcolordata内用于对柱状图颜色数组的构造
    var upcolordata = [];
    var downcolordata = [];
    for (var i = 0; i < this.phup.length; i++) {
      upcolordata.push(this.phup[i] + 7);
      downcolordata.push(-this.phdown[i]);
    }
    console.log(upcolordata);
    console.log(downcolordata);
    //根据分级标准把构造的柱状图颜色数组从新再造分别赋值给data中的colorlist1 （代表p<7的柱状图颜色数组）、colorlist1 （代表p>7的柱状图颜色数组）
    //p<7的柱状图颜色数组
    for (var i = 0; i < downcolordata.length; i++) {
      if (
        (downcolordata[i] < 6.01 && downcolordata[i] > 0) ||
        downcolordata[i] == 6.01
      ) {
        downcolordata.splice(i, 1, "#de7815");
      } else if (6.01 < downcolordata[i] && downcolordata[i] < 7) {
        downcolordata.splice(i, 1, "#e1a629");
      } else {
        downcolordata.splice(i, 1, "");
      }
    }
    this.colorlist1 = downcolordata;
    //p>7的柱状图颜色数组
    for (var i = 0; i < upcolordata.length; i++) {
      if (
        (upcolordata[i] < 7.09 && upcolordata[i] > 7) ||
        upcolordata[i] == 7.09
      ) {
        dupcolordata.splice(i, 1, "#f0ed77");
      } else if (
        (7.09 < upcolordata[i] && upcolordata[i] < 7.17) ||
        upcolordata[i] == 7.17
      ) {
        upcolordata.splice(i, 1, "#e4e872");
      } else if (
        (7.17 < upcolordata[i] && upcolordata[i] < 7.24) ||
        upcolordata[i] == 7.24
      ) {
        upcolordata.splice(i, 1, "#a6d759");
      } else if (
        (7.24 < upcolordata[i] && upcolordata[i] < 7.31) ||
        upcolordata[i] == 7.31
      ) {
        upcolordata.splice(i, 1, "#82b442");
      } else if (
        (7.31 < upcolordata[i] && upcolordata[i] < 7.39) ||
        upcolordata[i] == 7.39
      ) {
        upcolordata.splice(i, 1, "#6aa52a");
      } else if (
        (7.39 < upcolordata[i] && upcolordata[i] < 7.47) ||
        upcolordata[i] == 7.47
      ) {
        upcolordata.splice(i, 1, "#437f1c");
      } else if (
        (7.47 < upcolordata[i] && upcolordata[i] < 7.99) ||
        upcolordata[i] == 7.99
      ) {
        upcolordata.splice(i, 1, "#346713");
      } else if (7.99 < upcolordata[i] && upcolordata[i] < 14) {
        upcolordata.splice(i, 1, "#285609");
      } else {
        upcolordata.splice(i, 1, "");
      }
    }
    this.colorlist2 = upcolordata;
    /*    console.log(this.phdown);
    console.log(this.phup);
 */
/*     console.log(this.colorlist1); */
/*     console.log(this.colorlist2); */
    this.initChart();
        })
        .catch(error => {
          console.log(error);
        });

   
    
    },
    initChart() {
      var updata = this.phup;
      var downdata = this.phdown;
      var ssys = this.colorlist2;
      var xjys = this.colorlist1;
      var bjdata = this.statusdata;
      var adressdat = this.adressdata;
/*       console.log(bjdata); */
      //报警时，复制并构造bj1、bj2、bjda1、bjda2辅助柱状图显示数组
      var bj1 = [];
      var bj2 = [];
      var bjda1 = [];
      var bjda2 = [];
          //报警时，辅助柱状图ph>7数组构造
      //报警时，辅助柱状图ph<7数组构造
      for (var i = 0; i < bjdata.length; i++) {
        bj1.push(ssys[i]);
        bj2.push(xjys[i]);
        bjda1.push(updata[i]);
        bjda2.push(downdata[i]);
        if (bjdata[i] != 0) {
          bj1.splice(i, 1, "red");
          bj2.splice(i, 1, "red");
          bjda1.splice(i, 1, 16);
          bjda2.splice(i, 1, -16);
        }
      }
      for (var i = 0; i < updata.length; i++) {
        if (updata[i] == -0 || updata[i] == "") {
          bjda1.splice(i, 1, 0);      
          /*  bjda2.splice(i, 1, -16); */
        }
      }
      for (var i = 0; i < downdata.length; i++) {
        if (downdata[i] == -0 || downdata[i] == "") {
          bjda2.splice(i, 1, 0);
          
          /*  bjda2.splice(i, 1, -16); */
        }
      }
    /*   console.log(updata);
      console.log(downdata);
      console.log(this.phup);
      console.log(bjda1);
      console.log(this.phdown);
      console.log(bjda2);
   */
      this.chart = this.$echarts.init(this.$refs.echart7);
      this.chart.setOption({
         backgroundColor:"rgba(0,5,21,0.9)",
        grid: {
          left: "2%",
          right: "10%",
          bottom: "2%",
          top: "4%",
          height: "810",
          width: "497",
          containLabel: true
        },
        xAxis: [
          {
            type: "value",
            name: "",
             show:false,
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
             /*  splitNumber: 10, */
              // color:"#fff",
              textStyle: {
                //fontFamily: "微软雅黑",
                fontSize: 12
              }
            }
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
                color: "#3a3e4d",
                width: "1"
              }
            },
            axisLabel: {
              interval: 0,
              /*   rotate: 40, */
              show: true,
              splitNumber: 1,
              textStyle: {
                fontFamily: "微软雅黑",
                fontSize: "18"
              },
              //名称标签报警及正常状态处理过程
              formatter: function(dat, w) {
                console.log(dat);
                console.log(w);
                console.log(bjdata);
                var reg = /[\u4e00-\u9fa5]/g;
                var names = dat.match(reg);
                var hz = names.join("") + ":";
                var aa = dat.charAt(dat.length - 1);
                var arr = [];
                console.log(hz);
                if (aa == "0") {
                  arr = ["{normal|" + hz + "}"];
                } else if (w == adressdat.length - 1) {
                  arr = ["{makepoint|" + hz + "}"];
                } else if (aa == "1" || aa == "-1") {
                  arr = ["{abnormal|" + hz + "}"];
                }
                return arr;
              },

              rich: {
                normal: {
                  color: "#aeb5bf",
                  align: "center"
                },
                abnormal: {
                  color: "red",
                  backgroundColor: "rgba(255,0,0,0.5)",
                  width: "10",
                  height: "20",
                  align: "center"
                },
                makepoint: {
                  color: "#000",
                  backgroundColor: "yellow",
                  width: "10",
                  height: "20",
                  align: "center"
                }
              }
            },
            data: this.adressdata
          }
        ],
        series: [
       /*    {
            name: "",
            type: "bar",
            barWidth: 20, //柱图宽度
            data: bjda1,
            barGap: "-100%",
            itemStyle: {
              normal: {
                color: "rgba(255,0,0)",
                opacity: 0.5
              }
            }
          },
          {
            name: "",
            type: "bar",
            barWidth: 20, //柱图宽度
            data: bjda2,
            barGap: "-100%",
            itemStyle: {
              normal: {
                color: "rgba(255,0,0)",
                opacity: 0.5
              }
            }
          }, */
          {
            name: "PH<7",
            type: "bar",
            barWidth: 20, 
            barCategoryGap: "10",
            stack: "总量",
            label: {
              normal: {
                show: true,
                position: "left",
                color: "#fff",
                width: "40",
                height: "10"
              }
            },
            itemStyle: {
              normal: {
                opacity: 1,
               
                color: function(params) {
              
                  return bj2[params.dataIndex];
                },
                label: {
                  show: true,
                  position: "top",
                
                  formatter: function(re) {
                    if (Math.abs(re.data) == 0 || Math.abs(re.data) == 7) {
                      var ar = "";
                    } else {
                      var ar = Math.abs(re.data);
                    }
                    console.log(re);
                    for (var i = 0; i < bjdata.length; i++) {
                      if (bjdata[re.dataIndex] == 0) {
                        var arr = ["{normal|" + ar + "" + "}"];
                      } else if (bjdata[re.dataIndex] == 1) {
                        var arr = ["{up|" + ar + "↑" + "}"];
                      } else if (bjdata[re.dataIndex] == -1) {
                        var arr = ["{down|" + ar + "↓" + "}"];
                      }
                    }
                   
                    return arr;
                  },
                  rich: {
                    normal: {
                      color: "#fff",
                      align: "center",
                     
                      letterSpace: "6"
                    },
                    up: {
                      color: "#000",
                     
                      align: "center",
                    
                      letterSpace: "6"
                    },
                    down: {
                      color: "#000",
                     
                      align: "center",
                     
                      letterSpace: "6"
                    }
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
            barWidth: 15, //柱图宽度
            barCategoryGap: "10",
            label: {
              normal: {
                show: true,
                position: "right",
                color: "#000",
                width: "40",
                height: "10",
               
              }
            },
            data: this.phup,
            itemStyle: {
              normal: {
                color: function(params) {
                  /*  console.log(cc); */
                  return bj1[params.dataIndex];
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
                    for (var i = 0; i < bjdata.length; i++) {
                      if (bjdata[re2.dataIndex] == 0) {
                        var arr2 = ["{normal|" + ar2 + "" + "}"];
                      } else if (bjdata[re2.dataIndex] == 1) {
                        var arr2 = ["{up|" + ar2 + "↑" + "}"];
                      } else if (bjdata[re2.dataIndex] == -1) {
                        var arr2 = ["{down|" + ar2 + "↓" + "}"];
                      }
                    }
                    console.log(arr2);
                    console.log(ar2);
                    return arr2;
                  },
                  rich: {
                    normal: {
                      color: "#fff",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      /* padding: [22, -30], */
                      letterSpace: "6"
                    },
                    up: {
                      color: "#000",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      /*    padding: [22, -30], */
                      letterSpace: "6"
                    },
                    down: {
                      color: "#000",
                      /*    backgroundColor: "red",
                                width: "3",
                                height: "12", */
                      align: "center",
                      /*   padding: [22, -30], */
                      letterSpace: "6"
                    }
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
