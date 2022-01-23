<template>
  <div ref="map-root" style="width: 1000px; height: 600px"></div>
</template>

<script>
import View from "ol/View";
import Map from "ol/Map";
import TileLayer from "ol/layer/Tile";
import OSM from "ol/source/OSM";
import Marker from "ol/layer/vec"

// importing the OpenLayers stylesheet is required for having
// good looking buttons!
import "ol/ol.css";

export default {
  name: "MapContainer",
  components: {},
  props: {
    bibliotecas: [],
  },
  data() {
    return {};
  },
  mounted() {
    // this is where we create the OpenLayers map

    var markers = new Mark

    var map = new Map({
      // the map will be created using the 'map-root' ref
      target: this.$refs["map-root"],
      layers: [
        // adding a background tiled layer
        new TileLayer({
          source: new OSM(), // tiles are served by OpenStreetMap
        }),
      ],

      // the map view will initially show the whole world
      view: new View({
        padding: [16, 16, 16, 16],
        zoom: 4,
        center: [39.4702, -0.376805],
        constrainResolution: true,
      }),
    });
  },
};
</script>