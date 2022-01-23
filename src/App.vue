
<template>
  <div>
    <header>
      <ul>
        <button v-on:click="changeToLoad">Load Page</button>
        <button v-on:click="changeToSearch">Search Page</button>
      </ul>
    </header>
    <body>
      <div id="app">
        <img alt="Vue logo" src="./assets/logo.png" />

        <bibliotecas-list v-if="page_selected == 'load'" />
        <searcher v-if="page_selected == 'search'" />
      </div>
    </body>
  </div>
</template>



<script>
import BibliotecasList from "./components/BibliotecasList.vue";
import Searcher from "./components/Searcher.vue";
export default {
  name: "App",
  data() {
    return {
      page_selected: "load",
    };
  },
  components: {
    BibliotecasList,
    Searcher,
  },
  methods: {
    changeToLoad: function () {
      this.page_selected = "load";
    },
    changeToSearch: function () {
      this.page_selected = "search";
    },
  },
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
}
</style>
