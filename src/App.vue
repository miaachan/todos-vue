<template>
  <div id="app">
    <Main />
  </div>
</template>

<script>
import Main from "./components/Main";

export default {
  name: "App",
  components: {
    Main
  }
};
</script>

<style>
#app {
  display: flex;
  align-items: center;
  font-family: "Noto Sans", Helvetica Neue, Helvetica, Arial, Microsoft JhengHei,
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  height: 100%;
  background: #FCFAF2;
  background-image: url("~@/assets/texture.png");
}
</style>
