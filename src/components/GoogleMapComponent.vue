<template>
  <GoogleMap
    v-bind:api-key="apiValue"
    style="width: 100%; height: 500px"
    :center="center"
    :zoom="15"
  >
    <Marker :options="markerOptions" />
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
  },
  components: { GoogleMap, Marker },
  setup(props) {
    const center = reactive({ lat: props.latitude, lng: props.longitude });
    const markerOptions = reactive({
      position: center,
      label: "L",
      title: "LADY LIBERTY",
    });

    watch(
      () => [props.latitude, props.longitude],
      ([newLat, newLng]) => {
        center.lat = newLat;
        center.lng = newLng;
        markerOptions.position = center;
      }
    );

    return { center, markerOptions };
  },
};
</script>
