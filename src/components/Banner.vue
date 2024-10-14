<template>
  <div id="main_banner">
    <h1>Make Your Burger</h1>
  </div>
</template>

<script setup></script>

<style scoped>
#main_banner {
  background-image: url(/img/burger.jpg);
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  height: 80vh;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  box-shadow: inset 0px -12px 13px 0px rgba(0, 0, 0, 0.384);
}

#main_banner h1 {
  color: #fff;
  text-align: center;
  font-size: 60px;
  background-color: #222;
  padding: 20px 40px;
}
</style>
