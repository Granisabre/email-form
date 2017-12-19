<template>
  <div id="app">
    <div class='background'></div>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 100%;
  height: 1080px;
  max-height: 100vh;
}

.background {
  top: 0;
  width: 100%;
  height: 1080px;
  max-height: 100vh;
  position: fixed;

  background: linear-gradient(#00BFFF, #00CED1);
}
</style>
