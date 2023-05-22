<template>
  <div class="get-location">
    <button type="button" @click="getLocation">Get your location</button>
    <div v-if="latitude && longitude">
      <div>Latitude: {{ latitude }}</div>
      <div>Longitude: {{ longitude }}</div>
    </div>
    <div v-if="address">
      Address: {{ address }}
    </div>
  </div>
</template>

<script>
export default {
  name: "GetLocation",
  data() {
    return {
      latitude: null,
      longitude: null,
      address: null,
    };
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        async (position) => {
          this.latitude = position.coords.latitude;
          this.longitude = position.coords.longitude;
          await this.getAddress(this.latitude, this.longitude);
        },
        (error) => {
          console.log(error.message);
        }
      );
    },
    async getAddress(lat, long) {
      try {
        const apiKey = process.env.VUE_APP_GOOGLE_API;
        const res = await fetch(`https://maps.googleapis.com/maps/api/geocode/json?latlng=${lat},${long}&key=${apiKey}`);
        const result = await res.json();
        if (result.error_message) {
          console.log(result.error_message);
        } else {
          this.address = result.results[0].formatted_address;
        }
      } catch(e) {
        console.log(e);
      }
    }
  },
};
</script>

<style scoped>
.get-location {
  font-size: 20px;
}

button {
  font-size: 30px;
  border-radius: 10px;
  cursor: pointer;
  margin-bottom: 10px;
}

button:hover {
  background-color: red;
  color: white;
}
</style>
