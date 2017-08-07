/* 需要加载地图的实例 */

<template>
  <div>
    <Charts width="100%" height="760px" :config="config" />
  </div>
</template>

<script>
  import Charts from '../chart'
  import source from './data'  /* echarts 官方copy的数据 也可改成异步获取方式*/


  export default {
    data() {
      return {
        config: {
          async: { /*  异步 获取地图 */
            url: '/static/china.json',
            cb(echarts, data) {
              return new Promise((resolve, reject) => {
                if (data) {
                  echarts.registerMap('china', data);
                  resolve();
                } else reject();
              })
            }
          },
          type: ['map', 'scatter', 'effectScatter'],
          component: ['tooltip', 'legend', 'title', 'geo'],
          option: { /* echarts 官方 copy*/
            backgroundColor: '#404a59',
            title: {
              text: '全国主要城市空气质量',
              subtext: 'data from PM25.in',
              sublink: 'http:www.pm25.in',
              left: 'center',
              textStyle: {
                color: '#fff'
              }
            },
            tooltip: {
              trigger: 'item'
            },
            legend: {
              orient: 'vertical',
              y: 'bottom',
              x: 'right',
              data: ['pm2.5'],
              textStyle: {
                color: '#fff'
              }
            },
            geo: {
              map: 'china',
              label: {
                emphasis: {
                  show: false
                }
              },
              roam: true,
              itemStyle: {
                normal: {
                  areaColor: '#323c48',
                  borderColor: '#111'
                },
                emphasis: {
                  areaColor: '#2a333d'
                }
              }
            },
            series: [{
                name: 'pm2.5',
                type: 'scatter',
                coordinateSystem: 'geo',
                data: this.convertData(source.data),
                symbolSize: function (val) {
                  return val[2] / 10;
                },
                label: {
                  normal: {
                    formatter: '{b}',
                    position: 'right',
                    show: false
                  },
                  emphasis: {
                    show: true
                  }
                },
                itemStyle: {
                  normal: {
                    color: '#ddb926'
                  }
                }
              },
              {
                name: 'Top 5',
                type: 'effectScatter',
                coordinateSystem: 'geo',
                data: this.convertData(source.data.sort(function (a, b) {
                  return b.value - a.value;
                }).slice(0, 6)),
                symbolSize: function (val) {
                  return val[2] / 10;
                },
                showEffectOn: 'render',
                rippleEffect: {
                  brushType: 'stroke'
                },
                hoverAnimation: true,
                label: {
                  normal: {
                    formatter: '{b}',
                    position: 'right',
                    show: true
                  }
                },
                itemStyle: {
                  normal: {
                    color: '#f4e925',
                    shadowBlur: 10,
                    shadowColor: '#333'
                  }
                },
                zlevel: 1
              }
            ]
          }
        }
      }
    },
    methods: {
      convertData(data) { /* echrats 官方示例 函数*/
        let res = [];
        for (let i = 0; i < data.length; i++) {
          let geoCoord = source.geoCoordMap[data[i].name];
          if (geoCoord) {
            res.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].value)
            });
          }
        }
        return res;
      }
    },
    components: {
      Charts
    }
  }

</script>
