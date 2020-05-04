<template>
  <div id="app">
    <b-container class="layout">
      <b-row class="h-100">
        <b-col cols="3" class="p-0">
          <LSection class="p-3" user="miaachan" />
        </b-col>
        <b-col cols="9" class="p-0">
          <Main class="p-3" />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import LSection from "./components/LSection";
import Main from "./components/Main";

export default {
  name: "App",
  components: {
    LSection,
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
  background: #BDC0BA;
  background-image: url("~@/assets/texture.png");
}

.layout {
  min-height: 400px;
  box-shadow: 2px 5px 8px 0 rgba(0, 0, 0, 0.2);
}
</style>
