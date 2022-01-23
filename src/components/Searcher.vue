<template>
  <div>
    <leaflet-map :bibliotecas="bibliotecas"></leaflet-map>
    <h1>Buscador de Bibliotecas</h1>
    <input v-model="provincia" placeholder="Introduce Provincia" />
    <br />
    <input v-model="localidad" placeholder="Introduce Localidad" />
    <br />
    <input v-model="cod_postal" placeholder="Introduce Codigo Postal" />
    <br />

    <select v-model="tipo">
      <option value="">Seleccione un tipo</option>
      <option>Biblioteca</option>
      <option>Biblioteca Pública Municipal</option>
      <option>Centro de Documentacion</option>
      <option>Mediateca</option>
      <option>Agencia de Lectura</option>
      <option>Biblioteca Especializada</option>
      <option>Biblioteca Infantil</option>
    </select>
    <br />

    <button :disabled="this.loading" v-on:click="buscar">Buscar</button>

    <div>
      <h3 v-if="this.bibliotecas != []">Resultados de la búsqueda:</h3>

      <div v-else></div>
    </div>

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
import LeafletMap from "./LeafletMap.vue";

export default {
  name: "buscador",
  components: {
    PulseLoader,
    LeafletMap,
  },
  data() {
    LeafletMap;
    return {
      localidad: "",
      provincia: "",
      cod_postal: "",
      tipo: "",
      loading: false,
      bibliotecas: [],
    };
  },
  // created: async function () {
  //   const res = await axios.get("http://localhost:8080/find/all");

  //   this.bibliotecas = res.data;
  // },
  methods: {
    buscar: async function () {
      try {
        const res = await axios.get("http://localhost:8080/find/all");
        this.bibliotecas = res.data;

        this.loading = true;
        if (this.provincia != "") {
          this.bibliotecas = this.bibliotecas.filter(
            (d) => d.localidad.provincia.nombre === this.provincia
          );
        }
        if (this.localidad != "") {
          this.bibliotecas = this.bibliotecas.filter(
            (d) => d.localidad.nombre === this.localidad
          );
        }
        if (this.cod_postal != "") {
          this.bibliotecas = this.bibliotecas.filter(
            (d) => d.codigopostal === this.cod_postal
          );
        }
        if (this.tipo != "") {
          this.bibliotecas = this.bibliotecas.filter(
            (d) => d.tipo === this.tipo
          );
        }
        this.loading = false;
      } catch (e) {
        console.log(e);
        console.log("f");
      }
    },
  },
};
</script>