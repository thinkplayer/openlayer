<template>
  <div class="container">
    <div id="map">
      <div class="fixed-wrap">
        <el-button type="primary" @click="featureDrawer = true">
          feature
        </el-button>
      </div>
    </div>

    <el-drawer :visible.sync="featureDrawer" :with-header="false">
      <OLFeature @chooseFeatureType="chooseFeatureType"></OLFeature>
    </el-drawer>
  </div>
</template>

<script>
import "ol/ol.css";
import Map from "ol/Map";
import Draw from "ol/interaction/Draw";
import { OSM, Vector as VectorSource } from "ol/source";
import View from "ol/View";
import { Tile as TileLayer, Vector as VectorLayer } from "ol/layer";
import { fromLonLat } from "ol/proj";
import { Circle as CircleStyle, Fill, Stroke, Style } from "ol/style";
import {
  Snap,
  Modify,
  Select,
  defaults as defaultInteractions
} from "ol/interaction";

import OLFeature from "./components/feature";

export default {
  name: "OL",
  components: { OLFeature },
  data() {
    return {
      featureDrawer: false,
      map: null,
      mapOpt: {
        draw: null
      }
    };
  },
  computed: {},
  created() {},
  mounted() {
    this.initMap();
  },
  methods: {
    initMap() {
      const raster = new TileLayer({
        source: new OSM(),
        zIndex: 0,
        className: "ol-baseMap"
      });
      this.map = new Map({
        target: "map",
        layers: [raster],
        view: new View({
          // fromLonLat 将4326转成3857
          center: fromLonLat([113.658097, 34.745795]),
          zoom: 12
        })
      });
      this.mapEvents();
    },
    mapEvents() {
      this.map.on("click", evt => {
        console.log(
          "%c 🍔 evt: ",
          "font-size:20px;background-color: #E41A6A;color:#fff;",
          evt
        );
        const { pixel } = evt;
        const feature = this.map.forEachFeatureAtPixel(pixel, v => v);
        const layers = this.map.getLayers();
        const featureLlayer = layers.array_.find(
          layer => layer.className_ === "ol-feature"
        );
        if (!featureLlayer) return;
        const featureSource = featureLlayer.getSource();
        const modify = new Modify({ source: featureSource });
        const snanp = new Snap({ source: featureSource });
        this.map.addInteraction(modify);
        this.map.addInteraction(snanp);
      });
      this.map.on("pointermove", evt => {
        if (evt.dragging) return;
      });
      this.map.on("dbclick", evt => {
        console.log(
          "%c 🍑 evt: ",
          "font-size:20px;background-color: #6EC1C2;color:#fff;",
          evt
        );
      });
    },

    chooseFeatureType(type) {
      this.drawFeature(type);
    },

    drawFeature(type) {
      console.log("%c🌼type:", "font-size:20px;border:3px inset #42b983", type);
      const source = new VectorSource({ wrapX: false });
      const featureLayer = new VectorLayer({
        className: "ol-feature",
        source,
        zIndex: 1
      });
      const layers = this.map.getLayers();
      let { draw } = this.mapOpt;
      this.map.removeInteraction(draw);
      if (type) {
        draw = new Draw({
          source,
          type
        });
        this.map.addInteraction(draw);
      }
      // 如果没有feature的图层,就添加
      if (!layers.array_.some(layer => layer.className_ === "ol-feature")) {
        this.map.addLayer(featureLayer);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  height: calc(100vh - 50px);
  width: 100%;
}

#map {
  height: 100%;
  width: 100%;
  position: relative;

  &:focus {
    outline: #4a74a8 solid 0.15em;
  }

  .fixed-wrap {
    position: absolute;
    top: 10px;
    left: 10px;
    padding: 10px;
    z-index: 2;
  }
}
</style>
