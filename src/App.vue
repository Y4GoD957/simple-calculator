<template>
  <div id="app">
    <SimpleCalculator />
  </div>
</template>

<script>
import SimpleCalculator from './components/SimpleCalculator.vue';

export default {
  name: 'App',
  components: {
    SimpleCalculator,
  },
};
</script>

<style>
body {
  font-family: 'Arial', sans-serif;
  background-color: #282c34;
  color: white;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>