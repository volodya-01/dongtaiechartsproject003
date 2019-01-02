<template>
  <div>
    <div id="echart1" ref="echart1"></div>
    <div>{{show}}</div>
  </div>
</template>
<script>
export default {
  name: "echart1",
  data() {
    return {
      aadata: [],
      bbdata: [],
      ccdata: [],
      zydata: [],
      show: []
    };
  },
  mounted() {
    var that = this;
    var aa = Math.random();
    var cc = [];
    for (var i = 0; i < 100; i++) {
      that.aadata.push(aa);
      that.bbdata.push(i);
      cc.push(0);
    }
    cc.splice(cc.length - 1, 1, that.aadata[that.aadata.length - 1]);
    that.ccdata = cc;
    that.api();
    setInterval(function() {
      that.api();
    }, 500);
  },
 /*  watch: {
   aadata() {
      this.drawLine();
    }
  }, */
  methods: {
   api() {
      var that = this;
      var obj = { CodeID: ["1"] };
      that.$axios
        .post(
          "http://112.64.170.158:9111/Service1.svc/RealTimeData",
          JSON.stringify(obj),
          { headers: { "Content-Type": "application/json;" } }
        )
        .then(res => {
          var that = this;
          that.show = res.data.Item;
          var b = res.data.Item[0];
          that.aadata.shift();
          that.bbdata.shift();
          that.aadata.push(b);
          that.bbdata.push(b);
          that.ccdata.splice(that.zydata.length - 1, 1, b);
          that.drawLine();
        })
        .catch(error => {
          console.log(error);
        });
    },
    drawLine() {
      var that = this;
      var myChart = this.$echarts.init(this.$refs.echart1);
      myChart.setOption({
         backgroundColor:"rgba(0,5,21,0.9)",
         grid:{
           height:"150",
           top:"10",
           bottom:"10",
           right:"20",
           left:"20"
         },
        xAxis: {
          type: "category",
          boundaryGap: false,
          data: that.bbdata,
          axisTick:{
            show:false
          },
          axisLabel:{
          show:false
          },
            axisLine: {
        lineStyle: {
            color: '#3a3e4d', 
            width: 1 
        }
    }
        },
        yAxis: {
          type: "value",
          show:false
        },
        series: [
          {
            data: that.aadata,
            type: "line",
             animation: false,
            smooth: true,
            symbol: "none",
            lineStyle: {
              color: {
                type: "linear",
                colorStops: [
                  {
                    offset: 0,
                    color: "#00e4fc" // 0% 处的颜色
                  },
                  {
                    offset: 0.66,
                    color: "#00e4fc" // 66% 处的颜色
                  },
                  {
                    offset: 1,
                    color: "#fff" // 100% 处的颜色
                  }
                ],
                opacity: 0.4,
                globalCoord: false // 缺省为 false
              }
            }
          },
          {
            name: "最高气温",
            barWidth: 2,
            type: "bar",
            data: that.ccdata,
            animation: false,
             itemStyle:{
                normal:{
                   color:'#fff'
                    } 
                    },
            markPoint: {
              animation: false,
              symbol: 'circle',
              data: [{ type: "max" }],
              symbolSize:10,
              itemStyle: {
                normal: {
                  color:"#020b1c",
                  borderColor:"#fff",
                  borderWidth:"2",
                  label: {
                    show: false,
                  }
                }
              }
            }
          }
        ]
      });
    }
  }
};
</script>
<style scoped>
#echart1 {
  width: 700px;
  height: 180px;
}
</style>
