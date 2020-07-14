<template>
  <div id="app">
    <div class="columns">
      <div class="column is-half is-offset-one-quarter">
        <TranslationWidget />
      </div>
    </div>
    <br />
    <hr />
    <br />
    <div class="columns">
      <div class="column is-half is-offset-one-quarter">
        <TokenClassificationWidget />
      </div>
    </div>
  </div>
</template>

<script>
import TranslationWidget from "./components/TranslationWidget.vue";
import TokenClassificationWidget from "./components/TokenClassificationWidget.vue";

export default {
  name: "App",
  components: {
    TranslationWidget,
    TokenClassificationWidget
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 60px;
  margin-right: 60px;
}
</style>
