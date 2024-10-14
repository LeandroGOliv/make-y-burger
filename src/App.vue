<script setup>
import { RouterView } from "vue-router";
import NavBar from "./components/NavBar.vue";
import Footer from "./components/Footer.vue";

const logo_src = "/img/logo.png";
const app_name = "Make Your Burger";
</script>

<template>
  <div id="app">
    <NavBar :logo="logo_src" :alt="app_name" />
    <main>
      <RouterView />
    </main>
    <Footer />
  </div>
</template>

<style>
* {
  font-family: Helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex-grow: 1;
}

.main_container {
  margin: 50px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
