<template>
  <div class="div">
    <div class="left" @click="prevStat()"></div>
    <div class="container" :key="counter">
      <BarChart v-if="this.curr==`Bar`" :chartdata="chart" :options="options" />
      <LineChart v-if="this.curr=='Line'" :chartdata="chart" :options="options" />
      <PieChart v-if="this.curr=='Pie'" :chartdata="chart" :options="options" />
    </div>
    <div class="right" @click="nextStat()"></div>
  </div>
</template>


<script>
import json from "../assets/stat.json";
import LineChart from "./charts/HorizontalLine";
import BarChart from "./charts/Bar";
import PieChart from "./charts/Pie";
export default {
  name: "Stats",
  components: {
    LineChart,
    BarChart,
    PieChart,
  },
  data() {
    return {
      stats: json.data,
      chart: {
        labels: null,
        datasets: [
          {
            backgroundColor: "#f87979",
            borderWidth: 1,
          },
        ],
      },
      options: {
        title: {
          display: true,
          text: "Custom Chart Title",
        },
        responsive: true,
        showTooltips: true,
        legend: {
          display: false,
        },
        scales: {
          xAxes: [
            {
              lineHeight: 5,
              stacked: false,
              ticks: {
                autoSkip: false,
              },
              display: true,
              scaleLabel: {
                display: true,
                labelString: null,
              },
            },
          ],
          yAxes: [
            {
              ticks: {
                autoSkip: false,
              },
              display: true,
              scaleLabel: {
                display: true,
                labelString: null,
              },

              gridLines: {
                display: false,
              },
            },
          ],
        },
      },
      counter: -1,
      curr: null,
    };
  },
  methods: {
    getRandomColor() {
      var letters = "0123456789ABCDEF";
      var color = "#";
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    },
    prep() {
      this.chart.labels = this.stats[this.counter].xAxis;
      this.chart.datasets[0].label = this.stats[this.counter].xLabel;
      this.chart.datasets[0].data = this.stats[this.counter].yAxis;
      this.chart.datasets[0].backgroundColor = this.getRandomColor();
      this.options.scales.xAxes[0].display = true;
      this.options.scales.yAxes[0].display = true;
      this.options.title.display=false;
      this.options.scales.xAxes[0].scaleLabel.labelString = this.stats[
        this.counter
      ].xLabel;
      this.options.scales.yAxes[0].scaleLabel.labelString = this.stats[
        this.counter
      ].yLabel;
      if (this.stats[this.counter].Bar) {
        this.curr = "Bar";
      } else if (this.stats[this.counter].Line) {
        this.curr = "Line";
      } else {
        this.curr = "Pie";
        var colors = [];
        for (var i = 0; i < this.stats[this.counter].xAxis.length; i++) {
          colors.push(this.getRandomColor());
        }
        this.chart.datasets[0].backgroundColor = colors;
        this.options.scales.xAxes[0].display = false;
        this.options.scales.yAxes[0].display = false;
        this.options.title.display=true;
        this.options.title.text=this.stats[this.counter].xLabel
      }
    },
    nextStat() {
      if (this.counter <= 10) {
        this.counter += 1;
        this.prep();
      }
    },
    prevStat() {
      if (this.counter > 0) {
        this.counter -= 1;
        this.prep();
      }
    },
  },
  created() {
    this.nextStat();
  },
};
</script>

<style scoped>
.div {
  display: flex;

  width: 100%;
  height: 100%;
  justify-content: center;
}

.container {
  width: 40%;
  height: 85%;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto;
  flex-flow: column;
  text-align: center;

  border-radius: 10px;
  z-index: 3;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.19), 0 6px 6px rgba(0, 0, 0, 0.23);
}

.left {
  position: fixed;
  width: 50%;
  height: 100%;
  z-index: 2;
  left: 0;
}
.right {
  position: fixed;
  width: 50%;
  left: 50%;
  height: 100%;
  z-index: 2;
}
</style>