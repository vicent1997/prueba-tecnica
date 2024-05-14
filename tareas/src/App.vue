<template>
  <div class="fondo">
    <Navbar />
    <TareaAct />
  </div>
</template>

<script>
import Navbar from "./components/NavBar.vue";
import TareaAct from "./components/TareaAct.vue";
export default {
  name: "App",
  components: {
    Navbar,
    TareaAct,
  },
};
</script>

<style>
.fondo {
  height: 100vh;
  background-image: url(./assets/fondo.jpg);
}
</style>
