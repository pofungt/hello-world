<template>
  <el-button-group class="toolbar">
    <el-button type="primary" color="#626aef" :dark="isDark" plain @click="goPage(1)">
      <el-icon class="icon"><House /></el-icon>
      <div class="hidden-xs-only">
        Get Location
      </div>
    </el-button>
    <el-button type="primary" color="#626aef" :dark="isDark" plain @click="goPage(2)">
      <el-icon class="icon"><Search /></el-icon>
      <div class="hidden-xs-only">
        Search Address
      </div>
    </el-button>
    <el-button type="primary" color="#626aef" :dark="isDark" plain @click="goPage(3)">
      <el-icon class="icon"><MessageBox /></el-icon>
      <div class="hidden-xs-only">
        Search History
      </div>
    </el-button>
    <el-button type="primary" color="#626aef" :dark="isDark" plain @click="goPage(4)">
      <el-icon><Clock /></el-icon>
      <div class="hidden-xs-only">
        Time Zone
      </div>
    </el-button>
  </el-button-group>
  <GetLocation v-if="page === 1"></GetLocation>
  <SearchLocation v-else :page="page"></SearchLocation>
</template>

<script>
import { ref } from "vue";
import GetLocation from "./components/GetLocation.vue";
import SearchLocation from "./components/SearchLocation.vue";
import 'element-plus/theme-chalk/display.css';

import { House, Search, MessageBox, Clock } from "@element-plus/icons-vue";

export default {
  name: "App",
  components: {
    GetLocation,
    SearchLocation,
    House,
    Search,
    MessageBox,
    Clock
  },
  setup() {
    const page = ref(1);

    function goPage(pageNumber) {
      page.value = pageNumber;
    }

    return { page, goPage };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.toolbar {
  margin-bottom: 20px;
}
.icon {
  margin-right: 10px;
}
</style>
