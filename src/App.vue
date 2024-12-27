<template>
  <div id="app">
    <Puzzle />
  </div>
</template>

<script>
import Puzzle from "./components/Puzzle.vue";

export default {
  name: "App",
  components: {
    Puzzle,
  },
};
</script>

<style>
/* Add some basic styling for the splash screen */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 20px;
}

button {
  padding: 10px 20px;
  font-size: 18px;
}
</style>
