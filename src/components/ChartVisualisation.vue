<template>
  <div>
    <div ref="chart" style="width: 1000px; height: 600px;"></div>
  </div>
</template>

<script>
import Highcharts from 'highcharts';
import HighchartsBoost from 'highcharts/modules/boost';

HighchartsBoost(Highcharts);

export default {
  
  data() {
    return {
      data: [],
      newdata: [],
      time: 0,
      startTime: new Date(),
      chart: null,
      requestAnimationFrameId: null

    };
  },
  mounted() {

    for (var i = 0; i < 20000; i++) {
      this.time = (new Date() - this.startTime) / 1000;
      let value = Math.floor(Math.random() * 100) + 1; 
      this.data.push([this.time, value]);
    }

    this.chart = Highcharts.chart(this.$refs.chart, {
      chart: {
        type: 'scatter',
        
      },
      boost: {
          useGPUTranslations: true,
          enabled: true,
          seriesThreshold: 1
          
    },
      title: {
        text: '',
      },
      xAxis: {
        animation: false,
        minPadding: 0,
      },
      yAxis: {
        animation: false,
        title: {
          text: '',
        },
      },
      legend: {
        enabled: true,
      },
      series: [{
        name: '',
        marker: {
          radius: 3
         },
        boostThreshold: 1,
        turboThreshold: 1,
        animation: false,
        data: this.data,
      }],
    });



setInterval(() =>  {
  if (this.data.length < 50000) {
    const newValue = Math.floor(Math.random() * 100) + 1;
    this.time = (new Date() - this.startTime) / 1000;
    this.data.push([this.time, newValue]);
    console.log(this.data[this.data.length - 1]);
    this.chart.series[0].setData(this.data, false);
    const xAxis = this.chart.xAxis[0];
    xAxis.setExtremes(0, this.time);
  }
  }, 50);

  },

};
</script>

<style scoped>
@import '~highcharts/css/highcharts.css';
</style>