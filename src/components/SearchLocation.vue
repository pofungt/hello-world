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
    <GoogleMapComponent lat="latitude" long="longitude"></GoogleMapComponent>
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
    };
  },
  methods: {
    async submitSearch() {
        this.latitude = "43.8534144";
        this.longitude = "-79.3247744";
    },
  },
};
</script>

<style scoped></style>
