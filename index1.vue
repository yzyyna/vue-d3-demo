<!--
 original:https://bl.ocks.org/mbostock/3883245
 author:Demon3443002624@qq.com
 date:2022-2-25 15:20:58
 -->
<template>
  <div id="vue-line-chart">
    <svg width="960" height="500"></svg>
  </div>
</template>
<script>
import * as d3 from 'd3'

import _data from './data'

export default {
  name: 'VueLineChart',
  methods: {
    calculatePath() {
      // const d3 = this.$d3
      var svg = d3.select('#vue-line-chart>svg')
      var margin = { top: 20, right: 20, bottom: 30, left: 50 }
      var width = +svg.attr('width') - margin.left - margin.right
      var height = +svg.attr('height') - margin.top - margin.bottom
      var g = svg
        .append('g')
        .attr('transform', 'translate(' + margin.left + ',' + margin.top + ')')

      var parseTime = d3.timeParse('%d-%b-%y')

      var x = d3.scaleTime().rangeRound([0, width])

      var y = d3.scaleLinear().rangeRound([height, 0])

      var line = d3
        .line()
        .x(function(d) {
          return x(d.date)
        })
        .y(function(d) {
          return y(d.close)
        })
      // console.log(require('./data.csv')) *** 无法加载csv文件
      const data = []
      _data.forEach((e) => {
        const arr = e.split(',')
        const obj = {
          date: parseTime(arr[0]),
          close: +arr[1]
        }
        data.push(obj)
      })
      console.log(svg, margin, width, height, x, y)
      console.log(_data, data)
      x.domain(
        d3.extent(data, function(d) {
          return d.date
        })
      )
      y.domain(
        d3.extent(data, function(d) {
          return d.close
        })
      )

      g.append('g')
        .attr('transform', 'translate(0,' + height + ')')
        .call(d3.axisBottom(x))
        .select('.domain')
        .remove()

      g.append('g')
        .call(d3.axisLeft(y))
        .append('text')
        .attr('fill', '#000')
        .attr('transform', 'rotate(-90)')
        .attr('y', 6)
        .attr('dy', '0.71em')
        .attr('text-anchor', 'end')
        .text('Price ($)')

      g.append('path')
        .datum(data)
        .attr('fill', 'none')
        .attr('stroke', 'steelblue')
        .attr('stroke-linejoin', 'round')
        .attr('stroke-linecap', 'round')
        .attr('stroke-width', 1.5)
        .attr('d', line)
    }
  },
  mounted() {
    this.calculatePath()
  }
}
</script>
<style lang="less" scoped>
svg {
  margin: 25px;
}
path {
  fill: none;
  stroke: #76bf8a;
  stroke-width: 3px;
}
</style>
