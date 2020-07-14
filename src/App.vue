<template>
  <div id="app">
    <!-- <div class="columns"> -->
    <TranslationWidget />
    <TokenClassificationWidget />
    <!-- </div> -->
  </div>
</template>

<script>
import TranslationWidget from './components/TranslationWidget.vue'
import TokenClassificationWidget from './components/TokenClassificationWidget.vue'

export default {
  name: 'App',
  components: {
    TranslationWidget,
    TokenClassificationWidget
  }
}
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
