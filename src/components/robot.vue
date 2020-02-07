<template>
  <div>
    <Form>
      <Button type="primary" @click="$router.go(-1)">返回</Button>
      <Formitem>
        <Select>
          <Option>1</Option>
          <Option>2</Option>
          <Option>3</Option>
        </Select>
      </Formitem>
    </Form>
    <div>
      <div v-if="robots.length > 0" style="top:-30px;position:relative;">
        <div>
          <Row v-for="(item,index) in robots" :key="index" class="row_card">
            <i-col :xs="8" :sm="8" :md="3" :lg="3">
              <span class="span_data" :title="item.robotName" >{{item.robotName}}</span>
              <br>
              <span class="span_data" :title="item.robotMark" >{{item.robotMark}}</span>
            </i-col>
            <i-col :xs="12" :sm="12" :md="3" :lg="3">
              <div :style="{width:'100%',height:'100%'}">
                <div :id="'use_'+index" :style="{width:'90%',height:'150px'}"></div>
              </div>
            </i-col>
            <i-col :xs="4" :sm="4" :md="1" :lg="1">
              <div>
                <span class="span_data">{{item.robotStatus === '' ? '离线' : item.robotStatus}}</span>
              </div>
            </i-col>
            <i-col :xs="24" :sm="24" :md="17" :lg="17">
              <div :style="{width:'100%',height:'120px'}">
                <div :id="'custom_'+index" class="div_custom"></div>
              </div>
            </i-col>
          </Row>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const echarts = require('echarts')

export default {
  data() {
    return {
      pieFontSize: 14,
      formItem: {
        robot: '0',
        scene: 'All',
        data: '0',
      },
      robotList: [],
      sceneList: [],
      robots: [],
      useRate: '',
      startTime: 0,
      endTime: 0,
      colorList: [
        '#09f',
        '#f90',
        '#adf',
        '#09f',
        '#f90',
        '#adf',
        '#09f',
        '#f90',
        '#adf',
        '#09f',
        '#f90',
        '#adf',
        '#09f'
      ]
    }
  },
  methods: {
    getData() {
      var data = res.data.serialResult
      this.useRate = data.generalUserate
      this.startTime = data.startTime
      this.endTime = data.endTime
      var robotsResult = res.data.result
      this.robots = []
      this.robots = robotsResult
      setTimeout(() => {
        for (var i = 0; i < robotsResult.length; i++) {
          this.drawUsageRate('use_' + i, this.robots[i].robotUseRate)
          this.drawCustom('custom_' + i, this.robots[i].praList)
        }
      }, 0)
    },
    drawUsageRate(name, data) { // 当日总体使用情况
      const usage_rate = this.$echarts.init(document.getElementById(name))
      usage_rate.setOption({
        series: [{
          startAngle: 90, // 起始角度, 支持范围[0, 360]
          hoverAnimation: false, // 是否开启hover在扇区上的放大动画效果
          name: '当日总体使用情况',
          type: 'pie',
          color: ['#f90', '#09f'],
          radius: ['60%', '80%'],
          avoidLabelOverlap: true,
          silent: true, // 图形不触发鼠标事件
          label: {
            normal: {
              position: 'center'
            }
          },
          data: [
            {
              value: data,
              name: '',
              label: {
                normal: {
                  formatter: '{d}%',
                  textStyle: {
                    color: '#000',
                    fontSize: this.pieFontSize
                  }
                }
              }
            }
          ]
        }]
      })
      return usage_rate
    },
    drawCustom(name, robotPraList) {
      var categories = ['']
      var data = []
      this.$echarts.util.each(categories, (category, index) => {
        for (var i = 0; i < robotPraList.length; i++) {
          var staTim = Date.parse(new Date(robotPraList[i].staTim.replace(new RegExp(/-/gm), "/").split(".")[0]))
          var endTim = Date.parse(new Date(robotPraList[i].endTim.replace(new RegExp(/-/gm), "/").split(".")[0]))
          var eqTim = robotPraList[i].eqTim
          var name = robotPraList[i].sceNam
          var color = ''
          for (var j = 0; j < this.sceneList.length; j++) {
            if (this.sceneList[j].name == name) {
              color = this.sceneList[j].color
              break
            }
          }
          data.push({
            name: name,
            value: [
              index,
              staTim,
              endTim,
              eqTim
            ],
            itemStyle: {
              normal: {
                color: color
              }
            }
          })
        }
      })
      var option = {
        tooltip: {
          borderColor: '#7797B2',
          borderWidth: 2,
          extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3)',
          padding: 0,
          background: 'rgba(119, 151, 178, 0.53)',
          confine: true,
          formatter: function(params) {
            return "<div style='background-color:white;float:left;border-right:1px solid transparent;'>"
                + "<span style='font-size:14px;color:#242424;'>" + params.name + "</span><br/>" 
                + "<span style='font-size:10px;color:#242424;'></span>" + 
                + this.formatDate(new Date(params.value[1]), "yyyy-MM-dd hh:mm:ss") + " 至 "
                + this.formatDate(new Date(params.value[2]), "yyyy-MM-dd hh:mm:ss") + "</span><br>"
                "<span style='font-size:10px;color:#242424;>耗时: " + params.value[3] + "</span></div>"
          }
        },
        grid: {
          height: 80,
          top: '20%',
          right: '3%',
          left: 0,
          bottom: '10%',
          width: '95%',
          containLabel: true
        },
        dataZoom: [{
          type: 'inside',
        }],
        xAxis: [
          {
            show: false,
            scale: true,
            min: Date.parse(new Date(this.startTime.replace(new RegExp(/-/gm), "/").split(".")[0])),
            max: Date.parse(new Date(this.endTime))
          }
        ],
        yAxis: {
          height: 80,
          data: categories,
          show: true,
          axisLine: {
            lineStyle: {
              color: '#FFF'
            }
          },
          splitArea: {
            show: true,
          }
        },
        series: [
          {
            type: 'custom',
            renderItem: this.renderItem,
            encode: {
              x: [1, 2]
            },
            data: data
          }
        ]
      }
      const custom = this.$echarts.init(document.getElementById(name))
      custom.setOption(option)
      return custom
    },
    renderItem(params, api) {
      var categoryIndex = api.value(0)
      var start = api.coord([api.value(1), categoryIndex])
      var end = api.coord([api.value(2), categoryIndex])
      var height = api.size([0, 1])[1]
      var width = end[0] - start[0]
      return {
        type: 'rect',
        shape: this.$echarts.graphic.clipRectByRect({
          x: start[0],
          y: start[1] - height / 2,
          width: width,
          height: height
        },{
          x: params.coordSys.x,
          y: params.coordSys.y,
          width: params.coordSys.width,
          height: params.coordSys.height
        }),
        style: api.style()
      }
    },
    changeEcharts() { // echarts自适应
      setTimeout(() => {
        window.onresize = function() {
          this.setPieFontSizeForWidth()
          for (var i = 0; i < this.robots.length; i++) {
            this.drawUsageRate('use_' + i, this.robots[i].robotUseRate).resize()
            this.drawCustom('custom_' + i, this.robots[i].praList).resize()
          }
        }
      }, 500)
    },
    formatDate(date, fmt) { // 时间格式化
      if (/(y+)/.test(fmt)) {
        fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length))
      }
      const o = {
        'M+': date.getMonth() + 1,
        'd+': date.getDate(),
        'h+': date.getHours(),
        'm+': date.getMinutes(),
        's+': date.getSeconds(),
      }
      for (const k in o) {
        if (new RegExp(`(${k})`).test(fmt)) {
          const str = o[k] + ''
          fmt = fmt.replace(RegExp.$1, (RegExp.$1.length == 1) ? str : this.padLeftZero(str))
        }
      }
      return fmt
    },
    padLeftZero(str) {
      return ('00' + str).substr(str.length)
    },
    setPieFontSizeForWidth() {
      var clientWidth = document.body.clientWidth // 获取网页可见区域宽度
      if (clientWidth < 1200 && clientWidth >= 1000) {
        this.pieFontSize = 8
      } else {
        this.pieFontSize = 14
      }
    }
  },
  beforeDestroy() {
    window.onresize = null
  },
}
</script>

<style scoped>

</style>