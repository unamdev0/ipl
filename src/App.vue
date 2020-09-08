<template>
<div>
  <SideNav v-on:increment="update($event)"/>
  <Quiz v-show="currPage=='quiz'"/>
  <Stats v-if="currPage=='stat'"/>
  </div>
</template>


<script>
import Quiz from './components/Quiz'
import Stats from "./components/Stats";
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
      currPage:'quiz'
    }
  },
  methods:{
    update(event){
     this.currPage=event;
    }
  }
};
</script>
