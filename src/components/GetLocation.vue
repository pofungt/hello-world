<template>
  <div class="get-location">
    <el-button type="primary" @click="getLocation">Get your location</el-button>

    <el-descriptions
      title="Your Location"
      direction="vertical"
      :column="2"
      :size="size"
      border
      v-if="latitude && longitude && address"
    >
      <el-descriptions-item label="Latitude" align="center">{{ latitude }}</el-descriptions-item>
      <el-descriptions-item label="Longitude" align="center">{{ longitude }}</el-descriptions-item>
      <el-descriptions-item label="Address" align="center">{{ address }}</el-descriptions-item>
    </el-descriptions>
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
        const res = await fetch(
          `https://maps.googleapis.com/maps/api/geocode/json?latlng=${lat},${long}&key=${apiKey}`
        );
        const result = await res.json();
        if (result.error_message) {
          console.log(result.error_message);
        } else {
          this.address = result.results[0].formatted_address;
        }
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>

<style scoped>
.get-location-result {
  font-size: 15px;
}
</style>
