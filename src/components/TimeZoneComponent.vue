<template>
    <div>{{lastLocation.timeZoneId}} ({{lastLocation.timeZoneName}})</div>
</template>

<script>
export default {
  props: {
    lastLocation: Object
  },
};
</script>
