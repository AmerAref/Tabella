<template>
  <div id="app">
  <div>

   <b-navbar type="dark" variant="dark" > 
      <b-nav-toggle target="nav_dropdown_collapse">
        </b-nav-toggle> <b-collapse is-nav id="nav_dropdown_collapse"> 
          <b-nav is-nav-bar> 
            <b-nav-item href="/#/" align="left"><b>Home</b></b-nav-item> 
            <b-nav-item href="/#/MyVuetable" align="left">  <b>Table</b> </router-link> </b-nav-item>
            <b-nav-item href="/#/Charts" align="left"><b>Dashboard</b></b-nav-item> 
          </b-nav> 
        </b-collapse> 
      </b-navbar>
  
<router-view></router-view>

  </div>
  </div>


</template>

<script>
import MyVuetable from './components/MyVuetable'

export default {
  name: 'app',
  components: {
    MyVuetable
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, 'hackers', Times, serif, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color:black;
}
body{
  background-color: yellowgreen
}
</style>