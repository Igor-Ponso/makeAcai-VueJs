<template>
  <div id="main-banner">
    <h1>Açaí Bow</h1>
  </div>
</template>

<script>
export default {
  name: "Banner"
};
</script>

<style scoped>

  #main-banner{
    background-image: url('/img/acai.jpeg');
    background-size: cover;
    width: 100%;
    height: 500px;
    display: flex;
    align-items: center;
    justify-content: flex-start;
  }

  #main-banner h1{
    color: #fff;
    text-align: center;
    font-size: 60px;
    background-color: #222;
    padding: 20px;
  }
</style>
