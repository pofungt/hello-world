<template>
  <div>
    {{ lastLocation.address }}
  </div>
  <div>{{ lastLocation.timeZoneId }} ({{ lastLocation.timeZoneName }})</div>
  <div>
    <span id="span">{{ currentTime }}</span>
  </div>
</template>

<script>
import { ref, onBeforeUnmount } from "vue";

export default {
  props: {
    lastLocation: Object,
  },
  setup(props) {
    let currentTime = ref("");

    function updateTime() {
      currentTime.value = getLocalTime(props.lastLocation.timeZoneId);
    }

    function getLocalTime(timeZoneId) {
      const date = new Date();
      const options = {
        timeZone: timeZoneId,
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
      };
      const localTime = date.toLocaleString("en-US", options);
      return localTime;
    }

    updateTime();
    const intervalId = setInterval(() => updateTime(), 1000);

    onBeforeUnmount(() => {
      clearInterval(intervalId);
    });

    return { currentTime };
  },
};
</script>
