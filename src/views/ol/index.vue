<template>
  <div class="container">
    <div id="map"></div>
  </div>
</template>

<script>
import 'ol/ol.css'
import Map from 'ol/Map'
import OSM from 'ol/source/OSM'
import TileLayer from 'ol/layer/Tile'
import View from 'ol/View'
import { fromLonLat } from 'ol/proj'

export default {
  name: 'OL',
  data() {
    return {
      map: null
    }
  },
  computed: {},
  created() {
  },
  mounted() {
    this.initMap()
  },
  methods: {
    initMap() {
      this.map = new Map({
        target: 'map',
        layers: [
          new TileLayer({
            source: new OSM()
          })
        ],
        view: new View({
          center: fromLonLat([113.658097, 34.745795]),
          zoom: 12
        })
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  height: calc(100vh - 50px);
  width: 100%;
}

#map {
  height: 100%;
  width: 100%;

  &:focus {
    outline: #4A74A8 solid 0.15em;
  }
}
</style>
