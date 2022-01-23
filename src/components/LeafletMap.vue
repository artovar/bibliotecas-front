<template>
  <l-map
    style="height: 300px; width: 400px; margin-left: 35%"
    :zoom="zoom"
    :center="center"
  >
    <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
    <l-marker :lat-lng="markerLatLng"></l-marker>

    <div v-for="biblioteca in bibliotecas" :key="biblioteca.id">
      <l-marker :lat-lng="[biblioteca.latitud, biblioteca.longitud]"></l-marker>
    </div>
  </l-map>
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import "leaflet/dist/leaflet.css";

import { Icon } from "leaflet";

delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
  },
  props: {
    bibliotecas: [],
  },
  data() {
    return {
      url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 5,
      center: [40.416729, -3.703339],
      markerLatLng: [51.504, -0.159],
    };
  },
};
</script>
