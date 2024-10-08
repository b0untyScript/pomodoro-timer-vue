<template>
  <div id="app">
    <Timer />
  </div>
</template>

<script>
import Timer from './components/Timer.vue';

export default {
  components: {
    Timer,
  },
};
</script>

<style>
body {
  background-color: rgb(183, 0, 0);

}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #c3c3c3;
  margin-top: 0;
  padding: 0;
}
</style>
