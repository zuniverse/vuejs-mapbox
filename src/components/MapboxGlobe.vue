<template>
  <div ref="mapContainer" class="map-container"></div>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import 'mapbox-gl/dist/mapbox-gl.css';
import mapboxgl from 'mapbox-gl'; // or "const mapboxgl = require('mapbox-gl');"

mapboxgl.accessToken = import.meta.env.VITE_MAPBOX_ACESS_TOKEN

const mapContainer = ref<HTMLElement>()

onMounted(() => {
  const map = new mapboxgl.Map({
    // container: 'map', // container ID
    container: mapContainer.value!, // container ID
    style: 'mapbox://styles/mapbox/streets-v12', // style URL
    //@48.870596,2.3281411 = Palais Garnier
    center: [2.3281411, 48.870596], // starting position [lng, lat]
    zoom: 1, // starting zoom
    projection: 'globe',
  } as any);
  
  map.on('style.load', () => {
    map.setFog({
      color: 'rgb(186, 210, 235)', // Lower atmosphere
      'high-color': 'rgb(36, 92, 223)', // Upper atmosphere
      'horizon-blend': 0.02, // Atmosphere thickness (default 0.2 at low zooms)
      'space-color': 'rgb(11, 11, 25)', // Background color
      'star-intensity': 0.6 // Background star brightness (default 0.35 at low zoooms )
    } as any);
  })

  map.boxZoom.enable();
  map.doubleClickZoom.enable();

  // Set marker options.
  const marker = new mapboxgl.Marker({
    color: "red",
    draggable: false
  })
  marker
    .setLngLat([2.3281411, 48.870596])
    .addTo(map);
})
</script>

<style scoped>
  .map-container {
    width: 100vw;
    height: 100vh;
  }
</style>