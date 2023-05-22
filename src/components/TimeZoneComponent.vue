<template>
  <div>{{ lastLocation.timeZoneId }} ({{ lastLocation.timeZoneName }})</div>
  <div>
    <span id="span">{{ currentTime }}</span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentTime: null,
      intervalId: null
    };
  },
  props: {
    lastLocation: Object,
  },
  mounted() {
    this.updateTime();
    this.intervalId = setInterval(() => this.updateTime(), 1000);
  },
  methods: {
    updateTime() {
      this.currentTime = this.getLocalTime(this.lastLocation.timeZoneId);
    },
    getLocalTime(timeZoneId) {
        const date = new Date();
        const options = {timeZone: timeZoneId, hour: '2-digit', minute: '2-digit', second: '2-digit'};
        const localTime = date.toLocaleString('en-US', options);
        return localTime;
    }
  },
  beforeUnmount() {
    clearInterval(this.intervalId);
  },
};
</script>
