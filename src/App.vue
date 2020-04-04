<template>
  <div id="app">
    <Words :level="currentLevel" />
  </div>
</template>

<script>
import Words from "./components/Words";

export default {
  name: "App",
  components: {
    Words
  },
  data() {
    return {
      studentName: "Isaac",
      currentLevel: 1
    };
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  padding: 0;
  margin: 0;
  display: flex;
  background-color: #f2f2f2;
  width: 100%;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 0;
  flex: 1;
}
</style>
