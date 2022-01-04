<template>
  <div>
    <h1>Carga de almacén de datos</h1>
    <h3>Seleccione fuente:</h3>
    <div>
      <label>
        <input
          type="radio"
          name="Seleccionar todas"
          value="all"
          v-model="selected"
        />
        Seleccionar Todas </label
      ><br />

      <label>
        <input type="radio" name="Catalunya" value="cat" v-model="selected" />
        Catalunya </label
      ><br />
      <label>
        <input
          type="radio"
          name="Comunitat Valenciana"
          value="cv"
          v-model="selected"
        />
        Comunitat Valenciana </label
      ><br />

      <label>
        <input type="radio" name="Euskadi" value="eus" v-model="selected" />
        Euskadi </label
      ><br />

      <div id="button-1">
        <button :disabled="this.loading" v-on:click="cargar">Cargar</button>
      </div>
    </div>

    <h1>Listado Bibliotecas</h1>
    <div>
      <pulse-loader :loading="this.loading"></pulse-loader>
    </div>
    <div
      contenteditable="false"
      style="
        scroll: {
          margin: 4px, 4px;
          padding: 4px;
          height: 200px;
          overflow-y: auto;
        }
      "
      v-if="this.bibliotecas != [] && !this.loading"
    >
      <p v-for="biblioteca of bibliotecas" :key="biblioteca.nombre">
        {{ biblioteca.nombre }}
      </p>
    </div>
    <h3 v-if="this.bibliotecas == []">No hay contenido cargado</h3>
  </div>
</template>



<script>
import axios from "axios";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

export default {
  name: "TodoList",
  data() {
    return {
      bibliotecas: [],
      selected: "all",
      loading: false,
      error: false,
    };
  },
  created: async function () {
    try {
      const res = await axios.get("http://localhost:8080/find/all");

      this.bibliotecas = res.data;
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    cargar: async function () {
      try {
        this.loading = true;
        await axios.get("http://localhost:8080/load/" + this.selected);

        const res = await axios.get(
          "http://localhost:8080/find/" + this.selected
        );
        this.bibliotecas = res.data;
        console.log("donete bobete");
        this.loading = false;
      } catch (e) {
        console.log(e);
      }
    },
  },
  components: {
    PulseLoader,
  },
};
</script>