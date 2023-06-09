<template>
  <div id="welcome">
    <h4>{{ message }}</h4>
  </div>
</template>

<script>
export default {
  name: "WelcomeYou",
  data() {
    return {
      message: "THE WEATHER APP",
    };
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./Welcome.css" scoped></style>
