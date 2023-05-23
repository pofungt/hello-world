<template>
  <div class="searchTable">
    <button @click="deleteSelected">Delete Selected</button>
    <table>
      <tr>
        <td></td>
        <td>Time Zone</td>
        <td>Address</td>
      </tr>
      <tr v-for="(location, i) in locationsArrayInPage" :key="i">
        <td>
          <input type="checkbox" v-model="location.selected" />
        </td>
        <td>{{ location.timeZoneName }}</td>
        <td>{{ location.address }}</td>
      </tr>
    </table>
    <div class="pageNavigation">
      <button type="button" @click="previousPage">&lt;</button>
      <div>Page {{ page }} of {{ totalPage }}</div>
      <button type="button" @click="nextPage">&gt;</button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";

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
  setup(props) {
    const page = ref(1);
    const totalPage = Math.ceil(props.locationsArray.length / 10);

    const locationsArrayInPage = computed(() => {
      return props.locationsArray.slice(10 * (page.value - 1), 10 * page.value);
    });

    function previousPage() {
      if (page.value > 1) {
        page.value -= 1;
      }
    }

    function nextPage() {
      if (page.value < totalPage) {
        page.value += 1;
      }
    }

    return { page, totalPage, locationsArrayInPage, previousPage, nextPage };
  },
};
</script>

<style scoped>
.pageNavigation {
  display: flex;
}
.searchTable {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
</style>
