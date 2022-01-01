<template>
  <div>
    <h1>Buscador de Bibliotecas</h1>
    <input v-model="provincia" placeholder="Introduce Provincia" />
    <br />
    <input v-model="localidad" placeholder="Introduce Localidad" />
    <br />
    <input v-model="cod_postal" placeholder="Introduce Codigo Postal" />
    <br />

    <button :disabled="this.loading" v-on:click="buscar">Buscar</button>
    <div v-if="this.loading == true">
      <pulse-loader :loading="this.loading"></pulse-loader>
    </div>
    <div
      v-else
      style="
        scroll: {
          margin: 4px, 4px;
          padding: 4px;
          height: 200px;
          overflow-y: auto;
        }
      "
    >
      <p v-for="biblioteca of bibliotecas" :key="biblioteca.id">
        {{ biblioteca.nombre }}
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

export default {
  name: "buscador",
  components: {
    PulseLoader,
  },
  data() {
    return {
      localidad: "",
      provincia: "",
      cod_postal: "",
      loading: false,
      bibliotecas: [],
    };
  },
  created: async function () {
    const res = await axios.get("http://localhost:8080/find/all");

    this.bibliotecas = res.data;
  },
  methods: {
    buscar: async function () {
      try {
        this.loading = true;
        var search = "http://localhost:8080/search";
        if (this.provincia != "") {
          search += "?provincia=" + this.provincia + "&";
        }
        if (this.localidad != "") {
          search += "?localidad=" + this.localidad + "&";
        }
        if (this.cod_postal != "") {
          search += "?cod-postal=" + this.cod_postal + "&";
        }

        console.log(search);
        const res = await axios.get(search);

        this.bibliotecas = res.data;
        this.loading = false;
      } catch (e) {
        console.log(e);
        console.log("f");
      }
    },
  },
};
</script>