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
  <div v-if="latitude && longitude && locationsArray.length">
    <GoogleMapComponent
      :key="componentKey"
      :latitude="latitude"
      :longitude="longitude"
      :locationsArray="locationsArray"
    ></GoogleMapComponent>
    <SearchHistoryTable
      :key="componentKey"
      :locationsArray="locationsArray"
      @updateLocations="updateLocations"
    ></SearchHistoryTable>
  </div>
</template>

<script>
import SearchHistoryTable from "./SearchHistoryTable";
import GoogleMapComponent from "./GoogleMapComponent.vue";
export default {
  name: "SearchLocation",
  components: {
    GoogleMapComponent,
    SearchHistoryTable,
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
      const apiKey = process.env.VUE_APP_GOOGLE_API;
      await this.checkAddress(apiKey);
      await this.checkTimezone(apiKey);

      // Increment the key each time search is submitted
      this.componentKey += 1;
    },
    async checkAddress(apiKey) {
      // Search for lat long
      try {
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
          this.locationsArray.push({
            latLng: { lat: this.latitude, lng: this.longitude },
            address: result.results[0].formatted_address,
            timestamp: Date.now(),
            // For the history table checkbox
            selected: false,
          });
        }
      } catch (e) {
        console.log(e);
      }
    },
    async checkTimezone(apiKey) {
      try {
        const res = await fetch(
          ` https://maps.googleapis.com/maps/api/timezone/json?location=${this.latitude},${this.longitude}&timestamp=${Math.floor(Date.now() / 1000)}&key=${apiKey}`
        );
        const result = await res.json();
        // if (result.error_message) {
        //   console.log(result.error_message);
        // } else {
        //   const locationResult = result.results[0].geometry.location;
        //   // Store lat long in the state
        //   this.latitude = locationResult.lat;
        //   this.longitude = locationResult.lng;
        //   this.locationsArray.push({
        //     latLng: { lat: this.latitude, lng: this.longitude },
        //     address: result.results[0].formatted_address,
        //     timestamp: Date.now(),
        //     // For the history table checkbox
        //     selected: false,
        //   });
        console.log(result);
      } catch (e) {
        console.log(e);
      }
    },
    updateLocations(updatedLocationsArray) {
      this.locationsArray = updatedLocationsArray;
      this.componentKey += 1;
    },
  },
};
</script>

<style scoped></style>
