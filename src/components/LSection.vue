<template>
  <div class="LSection">
    Hi, {{ user }}!

    <div class="w-100 p-1"></div>

    <strong>{{ date }}</strong>
  </div>
</template>

<script>
export default {
  name: "LSection",
  props: {
    user: String
  },
  computed: {
    date: () => new Date().toDateString()
  }
};
</script>

<style>
.LSection {
  background-color: #f5f3f6;
  height: 100%;
  border-right: 2px solid #f3f3f3;
}
</style>
