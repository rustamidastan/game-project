<template>
  <Test />
</template>

<script>
import Test from "./Test.vue";
export default {
  name: "App",
  components: { Test },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  padding: 50px;
}

.main-wrapper {
  display: flex;
}

.wrapper {
  max-width: 200px;
  margin-top: 20px;
  margin-right: 50px;
  display: grid;
  grid-template-columns: repeat(3, 50px);
  grid-auto-rows: auto;
}

.item {
  width: 50px;
  height: 50px;
  border: 1px solid #eee;
  display: flex;
  align-items: center;
  justify-content: center;
}

button.active {
  background-color: rgba(0, 0, 255, 0.7);
  color: #fff;
}
</style>
