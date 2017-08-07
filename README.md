# vue-echarts-usually
像往常一样使用echarts
虽然已经有很多vue-echarts的插件了，感觉并没有解决像往常一样直接从官方示例copy再来修改那么便利，而且还得引入很多东西，下面是我自己的一个解决方案，非常的简单但用起来和原来直接在页面上使用差不多。

## 首先你还是得安装echarts
    npm install echarts -S
## 然后新建一个公共的vue组件
    chart.vue 你也可以自己写
## 最后，使用方式如下

    <template>
      <div>
        <Charts width="100%" height="760px" :config="config" />
      </div>
    </template>

    <script>
       import Chart form 'xxxx/chart'

       export default { 
         data(){
            return {
                config:{
                    async:{ /* 异步数据 */
                        url: 'get data url',
                        cb(echarts, data) { /* 返回echarts 和数据 */
                            ....do your thing....  
                            return true;
                         } 
                     },
                    type: ['line'],  // 类型
                    component: ['title', 'legend', 'grid'],  // 组件
                    option: 和往常一样的option配置
                }
            }
         },
         components: {
            Charts
         }
       }   
    </script> 

注：类型和组件，需要根据你的option来配置，echarts所有的类型和组件，可以参考chart.vue，
也可以查看官方：https://github.com/ecomfe/echarts/blob/master/index.js
