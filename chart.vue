<template>
  <div ref="chart" :style="{width:width,height:height}">
    &nbsp;</div>
</template>

<script>
  import axios from 'axios'
  import echarts from 'echarts/lib/echarts'
  /*
  require('zrender/lib/vml/vml');
  */


  /*
    config:{
       async:{
          url: 'get data url',
          cb(echarts, data) {
            ....do your thing....  
            return true;
         } 
       },
       type: ['line'],
       component: ['title', 'legend', 'grid'],
       option:配置
    }
  */

  export default {
    props: ['width', 'height', 'config'],
    mounted() {
      this.addDependencies()
    },
    methods: {
      addDependencies() {
        let {
          async
        } = this.config;

        this.config.type.map((t) => {
          this.addChart(t)
        })
        this.config.component.map((c) => {
          this.addComponent(c)
        })
        
        async ? (() => {
          axios.get(async.url).then((res) => {
            async.cb(echarts, res.data) && this.renderChart()
          })
        })() : this.renderChart()
      },
      addChart(t) {

        switch (t) {
          case "line":
            require('echarts/lib/chart/line');
            break;
          case "pie":
            require('echarts/lib/chart/pie');
            break;
          case "bar":
            require('echarts/lib/chart/bar');
            break;
          case "scatter":
            require('echarts/lib/chart/scatter');
            break;
          case "radar":
            require('echarts/lib/chart/radar');
            break;
          case "map":
            require('echarts/lib/chart/map');
            break;
          case "treemap":
            require('echarts/lib/chart/treemap');
            break;
          case "graph":
            require('echarts/lib/chart/graph');
            break;
          case "gauge":
            require('echarts/lib/chart/gauge');
            break;
          case "funnel":
            require('echarts/lib/chart/funnel');
            break;
          case "parallel":
            require('echarts/lib/chart/parallel');
            break;
          case "sankey":
            require('echarts/lib/chart/sankey');
            break;
          case "boxplot":
            require('echarts/lib/chart/boxplot');
            break;
          case "candlestick":
            require('echarts/lib/chart/candlestick');
            break;
          case "effectScatter":
            require('echarts/lib/chart/effectScatter');
            break;
          case "lines":
            require('echarts/lib/chart/lines');
            break;
          case "heatmap":
            require('echarts/lib/chart/heatmap');
            break;
          case "pictorialBar":
            require('echarts/lib/chart/pictorialBar');
            break;
          case "themeRiver":
            require('echarts/lib/chart/themeRiver');
            break;
          case "custom":
            require('echarts/lib/chart/custom');
            break;
        }
      },
      addComponent(c) {
        switch (c) {
          case "title":
            require('echarts/lib/component/title');
            break;
          case "legend":
            require('echarts/lib/component/legend');
            break;
          case "grid":
            require('echarts/lib/component/grid');
            break;
          case "tooltip":
            require('echarts/lib/component/tooltip');
            break;
          case "graphic":
            require('echarts/lib/component/graphic');
            break;
          case "axisPointer":
            require('echarts/lib/component/axisPointer');
            break;
          case "polar":
            require('echarts/lib/component/polar');
            break;
          case "geo":
            require('echarts/lib/component/geo');
            break;
          case "parallel":
            require('echarts/lib/component/parallel');
            break;
          case "singleAxis":
            require('echarts/lib/component/singleAxis');
            break;
          case "brush":
            require('echarts/lib/component/brush');
            break;
          case "calendar":
            require('echarts/lib/component/calendar');
            break;
          case "dataZoom":
            require('echarts/lib/component/dataZoom');
            break;
          case "visualMap":
            require('echarts/lib/component/visualMap');
            break;
          case "markPoint":
            require('echarts/lib/component/markPoint');
            break;
          case "markLine":
            require('echarts/lib/component/markLine');
            break;
          case "markArea":
            require('echarts/lib/component/markArea');
            break;
          case "toolbox":
            require('echarts/lib/component/toolbox');
            break;
          case "timeline":
            require('echarts/lib/component/timeline');
            break;
        }
      },
      renderChart() {
        let myChart = echarts.init(this.$refs.chart);
        myChart.setOption(this.config.option)
      }
    }
  }

</script>
