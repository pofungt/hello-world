<template>
  <div class="search-location" v-if="page === 2">
    <el-input
      v-model="inputValue"
      class="w-50 m-1"
      placeholder="Search Address"
      :prefix-icon="Search"
      size="large"
      @keyup.enter="submitSearch"
    >
      <template #append>
        <el-button :icon="Search" @click="submitSearch" />
      </template>
    </el-input>
  </div>

  <div v-if="latitude && longitude && locationsArray.length">
    <GoogleMapComponent
      v-if="page === 2"
      :key="componentKey"
      :latitude="latitude"
      :longitude="longitude"
      :locationsArray="locationsArray"
    ></GoogleMapComponent>
    <SearchHistoryTable
      v-if="page === 3"
      :key="componentKey"
      :locationsArray="locationsArray"
      @updateLocations="updateLocations"
    ></SearchHistoryTable>
    <TimeZoneComponent
      v-if="page === 4"
      :lastLocation="locationsArray[locationsArray.length - 1]"
    ></TimeZoneComponent>
  </div>
</template>

<script>
import SearchHistoryTable from "./SearchHistoryTable";
import GoogleMapComponent from "./GoogleMapComponent.vue";
import TimeZoneComponent from "./TimeZoneComponent.vue";

import { Search } from "@element-plus/icons-vue";

export default {
  name: "SearchLocation",
  components: {
    GoogleMapComponent,
    SearchHistoryTable,
    TimeZoneComponent,
  },
  data() {
    return {
      inputValue: "",
      latitude: null,
      longitude: null,
      locationsArray: [],
      componentKey: 0,
      Search,
    };
  },
  props: {
    page: Number,
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

          await this.checkTimezone(apiKey, result.results[0].formatted_address);
        }
      } catch (e) {
        console.log(e);
      }
    },
    async checkTimezone(apiKey, address) {
      try {
        const res = await fetch(
          ` https://maps.googleapis.com/maps/api/timezone/json?location=${
            this.latitude
          },${this.longitude}&timestamp=${Math.floor(Date.now() / 1000)}&key=${apiKey}`
        );
        const result = await res.json();
        if (result.error_message) {
          console.log(result.error_message);
        } else {
          this.locationsArray.push({
            latLng: { lat: this.latitude, lng: this.longitude },
            address,
            timestamp: Date.now(),
            // For the history table checkbox
            selected: false,
            timeZoneId: result.timeZoneId,
            timeZoneName: result.timeZoneName,
          });
          // Increment the key each time search is submitted
          this.componentKey += 1;
        }
      } catch (e) {
        console.log(e);
      }
    },
    updateLocations(updatedLocationsArray) {
      this.locationsArray = updatedLocationsArray;
      this.componentKey += 1;
    },
  }
};
</script>

<style scoped></style>
