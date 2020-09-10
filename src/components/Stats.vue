<template>
  <div class="container">
    <div class="left" title="Click to go to previous stat" @click="prevStat()"></div>
    <div class="main-container" style="z-index: 3;padding: 15px;" :key="counter">
      <BarChart v-if="this.curr == 'Bar'" :chartdata="chart" :options="options" />
      <LineChart v-if="this.curr == 'Line'" :chartdata="chart" :options="options" />
      <PieChart v-if="this.curr == 'Pie'" :chartdata="chart" :options="options" />
    </div>
    <div class="right" title="Click to go to next stat" @click="nextStat()"></div>
  </div>
</template>

<script>
const json = () => import("../assets/stat.json");

const LineChart = () => import("./charts/HorizontalLine");
const PieChart = () => import("./charts/Pie");
const BarChart = () => import("./charts/Bar");

export default {
  name: "Stats",
  components: {
    LineChart,
    BarChart,
    PieChart,
  },
  data() {
    return {
      stats: null,
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
      this.options.title.text = this.stats[this.counter].title;
      this.options.scales.xAxes[0].scaleLabel.labelString = this.stats[
        this.counter
      ].xLabel;
      this.options.scales.yAxes[0].scaleLabel.labelString = this.stats[
        this.counter
      ].yLabel;
      if (this.stats[this.counter].Bar && window.innerWidth > 900) {
        this.curr = "Bar";
      } else if (this.stats[this.counter].Line && window.innerWidth > 1000) {
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
        this.options.title.display = true;
        if (this.stats[this.counter].Line == true) {
          this.options.title.text = this.stats[this.counter].title;
        } else {
          this.options.title.text = this.stats[this.counter].title;
        }
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
  mounted() {
    if (localStorage.getItem("stat")) {
      this.stats = JSON.parse(localStorage.getItem("stat"));
      this.nextStat();
    } else {
      json().then((data) => {
        this.stats = data.data;
        localStorage.setItem("stat", JSON.stringify(data.data));
        this.nextStat();
      });
    }

    window.addEventListener("resize", this.prep);
  },
  beforeDestroy() {
    window.addEventListener("resize", this.prep);
  },
};
</script>

<style scoped>
.left {
  position: fixed;
  width: 50%;
  height: 100%;
  left: 0;
  z-index: -1;
  cursor: pointer;
  -moz-user-select: none;
  -webkit-user-select: none; 

  background-color: #68d4a7e7;
}
.right {
  position: fixed;
  width: 50%;
  left: 50%;
  height: 100%;
  z-index: -1;
  background-color: #68d4a7e7;
  cursor: pointer;
  -moz-user-select: none;
  -webkit-user-select: none;
}

.left:hover {
  background-color: #4f8b72e7;
}

.right:hover {
  background-color: #4f8b72e7;
}
</style>
