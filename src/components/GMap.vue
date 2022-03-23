<template>
  <div>
    <button @click="changeCenter">change center</button>
    <div class="map" ref="map"></div>
  </div>
</template>

<script>
import gmapsInit from "@/utils/gmaps";

export default {
  name: "GMap",
  props: {
    markers: {
      type: Array,
    },
  },
  async mounted() {
    try {
      // init and wait for the Google script is mounted
      this.google = await gmapsInit();

      // if the location is already set, for example
      // when returning back to this view from another one
      this.drawMap();
      // set the current location
      this.addMarker(this.center);
    } catch (err) {
      console.log("ERROR:", err);
    }
  },
  data() {
    return {
      google: null,
      map: null,
      innerMarkers: [],
      userMarker: null,
      marker: null,
      center: {
        lat: 48.853989,
        lng: 2.347368,
      },
    };
  },
  methods: {
    drawMap() {
      // creating the map object, displaying it in the $el DOM object
      this.map = new this.google.maps.Map(this.$refs["map"], {
        zoom: 16,
        center: this.center,
      });
      this.map.addListener("click", (event) => {
        console.log("click map", event.latLng);
        this.center = event.latLng;
      });
      // center the canvas of the map to the location of the user
      this.map.setCenter(this.center);
    },
    changeCenter() {
      this.center = {
        lat: 3,
        lng: 3,
      };
      console.log(this.center);
      this.map.setCenter(this.center);
    },

    // Adds a marker to the map and push to the array
    addMarker(location) {
      this.marker = new this.google.maps.Marker({
        position: location,
        map: this.map,
      });
    },

    clearMarker() {
      this.marker.setMap(null);
    },
    // Deletes all markers in the array by removing references to them
    deleteMarkers() {
      this.clearMarker();
      this.marker = null;
    },
  },
  watch: {
    center: function (newValue) {
      // clear the markers
      this.clearMarker();
      this.addMarker(newValue);
    },
  },
};
</script>

<style scoped>
.map {
  width: 100%;
  height: 400px;
}
</style>
