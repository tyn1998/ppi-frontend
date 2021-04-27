<template>
  <div id="myChart"></div>
</template>

<script>
import * as echarts from "echarts";
import * as axios from "axios";

export default {
  name: "Tree",
  props: {},
  data() {
    return {
      treeData: null,
    };
  },
  // 组件生命周期进入mounted阶段后执行
  mounted() {
    // AJAX
    axios
      .get("http://localhost:7001/tree")
      .then((response) => {
        // 获取返回body中的json数据
        this.treeData = response.data;

        let myChart = echarts.init(document.getElementById("myChart"));
        // 绘制图表
        myChart.setOption({
          tooltip: {
            trigger: "item",
            triggerOn: "mousemove",
          },
          series: [
            {
              type: "tree",
              data: [this.treeData],

              top: "1%",
              left: "7%",
              bottom: "1%",
              right: "20%",

              symbolSize: 7,

              label: {
                position: "left",
                verticalAlign: "middle",
                align: "right",
                fontSize: 14,
              },

              leaves: {
                label: {
                  position: "right",
                  verticalAlign: "middle",
                  align: "left",
                },
              },

              emphasis: {
                focus: "descendant",
              },

              expandAndCollapse: true,
              animationDuration: 550,
              animationDurationUpdate: 750,
            },
          ],
        });
      })
      .catch(function (error) {
        // 请求失败处理
        console.log(error);
      });
  },
};
</script>

<style>
#myChart {
  width: 100%;
  height: 100%;
}
</style>
