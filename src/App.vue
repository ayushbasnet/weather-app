<template>
  <div id="app">
    <Weather />
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Weather,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  height: 100%;
  width: 100%;
}
</style>
