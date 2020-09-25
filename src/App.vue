<template>
  <div id="app">
    <MatrixTable :data="data" />
  </div>
</template>

<script>
import MatrixTable from "./components/MatrixTable.vue";
import payload from "./data/payload.json";

export default {
  name: "App",

  components: {
    MatrixTable
  },

  data() {
    return {
      data: payload
    };
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
