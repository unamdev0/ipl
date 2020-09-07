<template>
  <div>
    <Stats v-bind:chartdata="chart" v-bind:options="options" />
  </div>
</template>


<script>
// import Quiz from './components/Quiz'
import Stats from "./components/Stats";
export default {
  name: "App",
  components: {
    // Quiz
    Stats,
  },
  data() {
    return {
      chart: {
        labels: ["Red", "Blue", "Yellow", "Green", "Purple", "Orange"],
        datasets: [
          {
            label: "# of Votes",
            data: [12, 19, 3, 5, 12, 3],

            borderWidth: 1,
          },
        ],
      },
      options: {
        scales: {
          xAxes: [{
         display: true,
         scaleLabel: {
           display: true,
           labelString: 'Month'
         }
       }],
          yAxes: [{
         display: true,
         scaleLabel: {
           display: true,
           labelString: 'Value'
         },
         ticks: {
           min: 0,
           max: 20,
           stepSize: 1,
           callback: function(label) {
             switch (label) {
               case 0:
                 return 'ZERO';
               case 1:
                 return 'ONE';
               case 2:
                 return 'TWO';
               case 3:
                 return 'THREE';
               case 4:
                 return 'FOUR';
               case 5:
                 return 'FIVE';
               default:
                 return "fd"
             }
           }
         },
         gridLines: {
           display: false
         }
       }]
        },
      },
      
    };
  },
};
</script>

<style scoped>
div{
  width:50%;
  height: 50%;
}
</style>