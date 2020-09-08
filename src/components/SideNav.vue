<template>
  <div>
    <div id="mySidenav" class="sidenav">
      <p class="closebtn" @click="closeNav()">&times;</p>
      <p v-on:click="$emit('increment','stat')">Stats
      </p>
      <p v-on:click="$emit('increment','quiz')" >Quiz
      </p>
    </div>
    <p class="opener" @click="openNav()">≡</p>
  </div>
</template>


<script>
export default {
  name: "SideNav",
  methods: {
    openNav() {
      document.getElementById("mySidenav").style.width = "250px";
    },
    closeNav() {
      document.getElementById("mySidenav").style.width = "0";
    },
  },
};
</script>


<style scoped>
.sidenav {
  height: 100%;
  width: 0; 
  position: fixed; 
  z-index: 5; 
  top: 0;
  left: 0;
  background-color: #111; 
  overflow-x: hidden; 
  padding-top: 60px; 
  transition: 0.5s; 
}


.sidenav p {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  color: #818181;
  display: block;
  transition: 0.3s;
}


.sidenav p {
  color: #f1f1f1;
}

.sidenav .closebtn {
  position: absolute;
  top: 0;
  right: 25px;
  font-size: 36px;
  margin-left: 50px;
}

#main {
  transition: margin-left 0.5s;
  padding: 20px;
}

.opener{
  
  font-size: 50px;
}

@media screen and (max-height: 450px) {
  .sidenav {
    padding-top: 15px;
  }
  .sidenav a {
    font-size: 18px;
  }
}
</style>