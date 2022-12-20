<template>
  <ValidID />
</template>

<script>
import ValidID from "./components/ValidID.vue";

export default {
  name: "App",
  components: {
    ValidID,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 600px;
}
</style>
