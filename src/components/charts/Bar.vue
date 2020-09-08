<script>
import { Bar } from "vue-chartjs";
export default {
  extends: Bar,
  props: ["chartdata", "options"],
  updated() {
    this.renderChart(this.chartdata, this.options);
  },
  mounted() {
    this.renderChart(this.chartdata, this.options);
  },
};
</script>

<style>
</style>