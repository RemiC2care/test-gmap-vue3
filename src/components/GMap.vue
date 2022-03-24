<template>
  <div>
    <div>
      <p><strong>Put your Google map api key in src/utils/gmaps.js</strong> </p>
      <p>When clicking on the map, the previous marker should desappear. But it doesn't.</p>
      <p>The exact same code work well in vue 2</p>
      <p>in /src/components/GMap.vue line 75 .setMap() function is not working</p>
    </div>
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
      // creating the map object
      this.map = new this.google.maps.Map(this.$refs["map"], {
        zoom: 16,
        center: this.center,
      });
      //add event listener on click
      this.map.addListener("click", (event) => {
        console.log("click map", event.latLng);
        this.center = event.latLng;
      });
      // center the canvas of the map to the location of the user
      this.map.setCenter(this.center);
    },

    // Add a marker to the map 
    addMarker(location) {
      this.marker = new this.google.maps.Marker({
        position: location,
        map: this.map,
      });
    },
    // Hide marker
    clearMarker() {
      this.marker.setMap(null);// <------ THIS ONE IS NOT WORKING -------------
    },
    // Delete marker
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
