<template>
  <div id="app">
    <BogoSortVisualizer />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import BogoSortVisualizer from './components/Bogo.vue';

export default defineComponent({
  name: 'App',
  components: {
    BogoSortVisualizer
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>