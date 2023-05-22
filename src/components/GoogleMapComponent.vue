<template>
  <GoogleMap
    v-bind:api-key="apiValue"
    style="width: 100%; height: 500px"
    :center="center"
    :zoom="15"
  >
    <MarkerCluster>
      <Marker
        v-for="(location, i) in locations"
        :options="{ position: location }"
        :key="i"
      />
    </MarkerCluster>
  </GoogleMap>
</template>

<script>
import { GoogleMap, Marker } from "vue3-google-map";
import { reactive, watch } from "vue";

export default {
  computed: {
    apiValue() {
      return process.env.VUE_APP_GOOGLE_API;
    },
  },
  props: {
    latitude: Number,
    longitude: Number,
    locationsArray: Array,
  },
  components: { GoogleMap, Marker },
  setup(props) {
    const center = reactive({ lat: props.latitude, lng: props.longitude });
    let locations = reactive(props.locationsArray);

    watch(
      () => [props.latitude, props.longitude, props.locationsArray],
      ([newLat, newLng, newArr]) => {
        center.lat = newLat;
        center.lng = newLng;
        locations = newArr;
      }
    );

    return { center, locations };
  },
};
</script>
