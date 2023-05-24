<template>
  <div class="searchTable">
    <el-button
      type="primary"
      color="#626aef"
      :dark="isDark"
      plain
      @click="deleteSelected"
    >
      Delete Selected
    </el-button>
    <table>
      <tr class="table-title">
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
      <el-button
        type="primary"
        color="#626aef"
        :dark="isDark"
        plain
        @click="previousPage"
      >
        <el-icon><ArrowLeftBold /></el-icon>
      </el-button>
      <div class="page-info">Page {{ page }} of {{ totalPage }}</div>
      <el-button type="primary" color="#626aef" :dark="isDark" plain @click="nextPage">
        <el-icon><ArrowRightBold /></el-icon>
      </el-button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { ArrowLeftBold, ArrowRightBold } from "@element-plus/icons-vue";

export default {
  components: {
    ArrowLeftBold,
    ArrowRightBold
  },
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
td {
  border: 1px solid black;
  padding: 8px;
}
table {
  font-family: "Helvetica Neue";
  margin: 10px 0;
}
.table-title {
  font-weight: bold;
}
.page-info {
  display: flex;
  justify-content: center;
  align-items: center;
  color: #626aef;
  font-weight: bold;
  margin: 0 10px;
}
</style>
