<template>
  <div id="app">
    <QuoteGenerator />
  </div>
</template>

<script>
import QuoteGenerator from './components/QuoteGenerator.vue';

export default {
  components: {
    QuoteGenerator,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
}
</style>
