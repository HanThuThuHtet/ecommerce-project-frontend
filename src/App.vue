<template>
  <NavBar />
  <router-view/>
</template>


<script>
  import * as bootstrap from 'bootstrap';
  import NavBar from "@/components/NavBar";
  import axios from "axios";
  export default{
    components: {
    NavBar
  },
    created() {
      if(localStorage.getItem('auth') && localStorage.getItem('token')){
        this.$store.dispatch('setAuth',JSON.parse(localStorage.getItem('auth')));
        this.$store.dispatch('setToken',localStorage.getItem('token'));
        axios.defaults.headers.common['Authorization'] = "Bearer "+localStorage.getItem('token');
      }
    },
  }
</script>


<style lang="scss">
  @import "app.scss";
</style>
