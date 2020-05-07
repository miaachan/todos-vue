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
@import url("https://fonts.googleapis.com/css?family=Raleway");
@import url("https://fonts.googleapis.com/css?family=Raleway:100");

#app {
  display: flex;
  align-items: center;
  font-family: "Raleway", "Noto Sans", Microsoft JhengHei !important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  min-height: 100vh;
  height: 100%;
  background: #fcfaf2;
  background-image: url("~@/assets/texture.png");
}
</style>
