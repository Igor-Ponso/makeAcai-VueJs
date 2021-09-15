<template>
  <Navbar :logo="logo" :alt="alt" />
  <router-view />
  <Footer/>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";

export default {
  components: {
    Navbar,
    Footer
  },
  data(){
    return{
      logo: "/img/logo.png",
      alt: 'Make Your Burger'
    }
  }
};
</script>

<style>
*{
  font-family: Helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}

.main-container {
  padding: 50px 0;
  height: 100%;
  max-height: calc(100vh - 145px);
}


</style>
