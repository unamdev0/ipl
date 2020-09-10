<template>
<div>
  <SideNav :curr="currPage" v-on:change="update($event)"/>
  <keep-alive>
  <Quiz  v-if="currPage=='IPL Quiz'"/>
  <Stats v-if="currPage=='IPL Stats'"/>
  </keep-alive>
  </div>
</template>


<script>
const Quiz = () => import('./components/Quiz')
const Stats = () => import('./components/Stats')
import SideNav from './components/SideNav'
export default {
  name: "App",
  components: {
    Quiz,
    Stats,
    SideNav
  },
   data(){
    return{
      currPage:'IPL Stats'
    }
  },
  methods:{
    update(event){
     this.currPage=event;
    }
  }
};
</script>
