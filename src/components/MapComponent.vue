<template>
  <div id="map-wrapper">
    <div ref="mapElement" class="map"></div>
  </div>
</template>

<script>
import 'leaflet';
import 'leaflet/dist/leaflet.css';
import '@ansur/leaflet-pulse-icon';
import '@ansur/leaflet-pulse-icon/dist/L.Icon.Pulse.css';
const L = window['L'];

delete L.Icon.Default.prototype._getIconUrl;
L.Icon.Default.mergeOptions({
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
});



export default {
  name: "MapComponent",
  data: () => ({
    map: null,
    tileLayer: null,
    marker: null,
    pulsingIcon: null,
  }),

  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      this.map = L.map(this.$refs.mapElement, {
        pmIgnore: false,
      }).setView([-25.441105, -49.276855], 13);
      // console.log("Right Side bar ", this.rightSidebar);
      this.tileLayer = L.tileLayer(
        "https://{s}.tile.openstreetmap.fr/osmfr/{z}/{x}/{y}.png",
        {
          maxZoom: 20,
          attribution:
            '&copy; OpenStreetMap France | &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        }
      );
      this.tileLayer.addTo(this.map);
      this.pulsingIcon = L.icon.pulse({iconSize: [20,20], color: 'red'});
      this.marker = L.marker([50,50], {icon: this.pulsingIcon}).addTo(this.map);
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  z-index: 1;
}
#map-wrapper {
  height: calc(100vh - 90px);
}
</style>