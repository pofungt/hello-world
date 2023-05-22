<template>
  <GoogleMap
    api-key=""
    style="width: 100%; height: 500px"
    :center="center"
    :zoom="15"
  >
    <Marker :options="{ position: center }" />
  </GoogleMap>
</template>

<script>
import { GoogleMap, Marker } from "vue3-google-map";

export default {
  data() {
    return {
      apiKey: process.env.VUE_APP_GOOGLE_API,
    };
  },
  props: {
    lat: String,
    long: String,
  },
  components: { GoogleMap, Marker },
  setup() {
    const center = { lat: 0, lng: 0 };

    return { center };
  },
};
</script>
