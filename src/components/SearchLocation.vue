<template>
  <div class="search-location">
    <input
      type="text"
      name="searchLocationInput"
      v-model="inputValue"
      @keyup.enter="submitSearch"
    />
    <button type="button" @click="submitSearch">Search</button>
  </div>
  <div v-if="latitude && longitude">
    <GoogleMapComponent
      :key="componentKey"
      v-bind:latitude="latitude"
      v-bind:longitude="longitude"
      v-bind:locationsArray="locationsArray"
    ></GoogleMapComponent>
  </div>
</template>

<script>
import GoogleMapComponent from "./GoogleMapComponent.vue";
export default {
  name: "SearchLocation",
  components: {
    GoogleMapComponent,
  },
  data() {
    return {
      inputValue: "",
      latitude: null,
      longitude: null,
      locationsArray: [],
      componentKey: 0,
    };
  },
  methods: {
    async submitSearch() {
      // Search for lat long
      try {
        const apiKey = process.env.VUE_APP_GOOGLE_API;
        const res = await fetch(
          `https://maps.googleapis.com/maps/api/geocode/json?address=${this.inputValue}&key=${apiKey}`
        );
        const result = await res.json();
        if (result.error_message) {
          console.log(result.error_message);
        } else {
            const locationResult = result.results[0].geometry.location;
          // Store lat long in the state
          this.latitude = locationResult.lat;
          this.longitude = locationResult.lng;
          this.locationsArray.push({lat:this.latitude, lng:this.longitude});
          this.componentKey += 1; // Increment the key each time search is submitted
        }
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>

<style scoped></style>
