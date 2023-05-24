<template>
  <el-descriptions
    title="Your Last Search"
    direction="vertical"
    :column="2"
    :size="size"
    border
  >
    <el-descriptions-item label="Address" align="center">{{ lastLocation.address }}</el-descriptions-item>
    <el-descriptions-item label="Time Zone" align="center">{{ lastLocation.timeZoneId }} ({{ lastLocation.timeZoneName }})</el-descriptions-item>
    <el-descriptions-item label="Local Time" align="center"><span id="span">{{ currentTime }}</span></el-descriptions-item>
  </el-descriptions>
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
