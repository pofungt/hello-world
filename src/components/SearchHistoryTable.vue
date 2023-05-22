<template>
  <button @click="deleteSelected">Delete Selected</button>
  <table>
    <tr>
      <td></td>
      <td>Time</td>
      <td>Address</td>
    </tr>
    <tr v-for="(location, i) in locationsArray" :key="i">
      <td>
        <input type="checkbox" v-model="location.selected" />
      </td>
      <td>{{ location.timestamp }}</td>
      <td>{{ location.address }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: {
    locationsArray: Array,
  },
  methods: {
    deleteSelected() {
      const updatedLocationsArray = this.locationsArray.filter(
        (location) => !location.selected
      );
      this.$emit("updateLocations", updatedLocationsArray);
    },
  },
};
</script>
