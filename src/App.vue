<template>
  <div>
    <SearchBar />
    <List />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import SearchBar from "./components/SearchBar.vue";
import List from "./components/List.vue";

export default defineComponent({
  name: "App",
  components: { SearchBar, List },
  setup() {
    // Setup
  },
});
</script>

<style lang="scss" module>
// Style
</style>
