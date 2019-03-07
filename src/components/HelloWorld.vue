<template>
  <div class="hello">
    <div class="map">
        <LMap :zoom=10 :center="initialLocation">
          <v-icondefault></v-icondefault>
          <v-tilelayer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></v-tilelayer>
          <v-marker-cluster :options="clusterOptions" @clusterclick="click()">
            <v-marker v-for="l in locations" :key="l.id" :lat-lng="l.latlng" :icon="icon">
              <v-popup :content="l.text"></v-popup>
            </v-marker>
          </v-marker-cluster>
        </LMap>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import * as Vue2Leaflet from 'vue2-leaflet'
import { latLng, Icon, icon } from 'leaflet'
import Vue2LeafletMarkercluster from 'vue2-leaflet-markercluster'
import iconUrl from 'leaflet/dist/images/marker-icon.png'
import shadowUrl from 'leaflet/dist/images/marker-shadow.png'
  function rand(n) {
    let max = n + 0.1
    let min = n - 0.1
    return Math.random() * (max - min) + min;
  }
  export default {
    components: {
      'LMap': Vue2Leaflet.LMap,
      'v-tilelayer': Vue2Leaflet.LTileLayer,
      'v-icondefault': Vue2Leaflet.LIconDefault,
      'v-marker': Vue2Leaflet.LMarker,
      'v-popup': Vue2Leaflet.LPopup,
      'v-marker-cluster': Vue2LeafletMarkercluster
    },
    methods: {
      click: function (e) {
        alert("clusterclick")
      }
    },
    data () {
      let locations = []
      for (let i = 0; i < 100; i++) {
        locations.push({
          id: i,
          latlng: latLng(rand(-34.9205), rand(-57.953646)),
          text: 'Hola ' + i
        })
      }
      let customicon = icon(Object.assign({},
        Icon.Default.prototype.options,
        {iconUrl, shadowUrl}
      ))
      return {
        locations,
        icon: customicon,
        clusterOptions: {},
        initialLocation: latLng(-34.9205, -57.953646)
      }
    },
    mounted() {
      setTimeout(() => {
        console.log('done')
        this.$nextTick(() =>{
          this.clusterOptions = { disableClusteringAtZoom: 11 }
        });
      }, 5000);
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.hello,
.map {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
</style>
