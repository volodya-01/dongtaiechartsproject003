<template>
  <div>
    <div id="echart8" ref="echart8" style="width: 2600px;height:450px;"></div>
    <div></div>
  </div>
</template>
<script>
export default {
  name: "echart8",
  data() {
    return {
      echartsdata: [],
      yeardata:""
    };
  },
  mounted() {
    var that = this;
    that.api();
    setInterval(function() {
      that.api();
    }, 500);
  },
  methods: {
    api() {
      var that = this;
      that.$axios
        .post("http://112.64.170.158:9117/Service1.svc/HistoryThermalData")
        .then(res => {
          var that = this;
          var resdata = res.data.ThermalData;
          var arr = [];
          for (var i = 0; i < resdata.length; i++) {
            arr.push([resdata[i].Date, resdata[i].WarningNum]);
          }
          var yeararr=[]
          var year=new Date().getFullYear()+""
          var threeyearsago=(year-2)+""
          yeararr.push(threeyearsago,year)
          that.yeardata=yeararr
          that.echartsdata = arr;
          that.drawLine();
        })
        .catch(error => {
          console.log(error);
        });
    },
    drawLine() {
      var that = this;
      var myChart = this.$echarts.init(this.$refs.echart8);
      myChart.setOption({
        backgroundColor: "rgba(0,5,21,0.9)",
        grid: {
            left: "4%",
          right: "0%",
          bottom: "2%",
          top: "4%",
          height: "200",
           width: "2510",
          containLabel: true
        },
        title: {
          top: 30,
          left: "center"
        },
        tooltip: {},
        visualMap: {
          splitNumber: 11,
          type: "piecewise",
          pieces: [
            {
              gt: 1350,
              color: "#d50707",   
            }, 
            {
              gt: 1200,
              lte: 1350,
              color: "#db14a5",
            }, 
            {
              gt: 1050,
              lte: 1200,
              color: "#8d18d0",
            }, 
            {
              gt: 900,
              lte: 1050,
              color: "#4d19c2",   
            }, 
            {
              gt: 750,
              lte: 900,
              color: "#2a00ff", 
            }, 
            {
              gt: 600,
              lte: 750,
              color: "#1d0eb2",
            }, 
            {
              gt: 450,
              lte: 600,
              color: "#1e1169", 
            },
            {
              gt: 300,
              lte: 450,
              color: "#8c9cbc",
            }, 
            {
              gt: 150,
              lte: 300,
              color: "#5f718e",
            },
            {
              gt: 0,
              lte: 150,
              color: "#293955",
            },
            {
              value: 0,
              color: "#1f3051"
            } 
          ],
          orient: "horizontal",
          show:false
        },
        calendar: {
          top: 120,
          left: 60,
          right: 30,
           height: 206,
           width:2510,
          itemStyle: {
            borderColor: "#233455",
            cellSize: [20, 20],
          },
          
          range:that.yeardata,
          dayLabel: {
            show: true,
            color: "#4c5662",
            fontSize: 18,
            fontFamily: "微软雅黑"
          },
          monthLabel: {
            position: "bottom",
            color: "#4c5662",
            fontSize: 18,
            fontFamily: "微软雅黑"
          },
          yearLabel: {
            position: "bottom",
            color: "#c3ccdb",
            fontSize: 18,
            fontFamily: "微软雅黑"
          },
          splitLine: {
            lineStyle: {
              width: 3,
              color: "#011233"
            }
          }
        },
        series: [
          {
            type: "heatmap",
            coordinateSystem: "calendar",
            data: that.echartsdata
          }
        ]
      });
    }
  }
};
</script>
<style scoped>
/* #echart8 {
  width: 2815px;
  height: 400px;
  margin-left: 600px;
} */
</style>
