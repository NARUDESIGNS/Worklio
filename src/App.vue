<template>
  <div :class="$style.app">
    <main :class="$style.main">
      <SearchBar />
      <List />
      <List />
      <List />
    </main>
    <section :class="$style.sorter">
      <Sort value="Value" :isActive="selected === 'Value'" @click="sortList('Value')" />
      <Sort
        value="Added Date"
        :isActive="selected === 'Added Date'"
        @click="sortList('Added Date')"
      />
    </section>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from "vue";
import SearchBar from "./components/SearchBar.vue";
import List from "./components/List.vue";
import Sort from "./components/Sort.vue";

export default defineComponent({
  name: "App",
  components: { SearchBar, List, Sort },
  setup() {
    let selected = ref("Value");

    // sort list
    const sortList = (value: string): void => {
      selected.value = value;
    };

    return { selected, sortList };
  },
});
</script>

<style lang="scss" module>
.app {
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 60px;
  padding-top: 100px;
  width: 80%;
  max-width: 1000px;

  & .main {
    flex: 2;
  }
}

@media only screen and (max-width: 600px) {
  .app {
    flex-direction: column;

    & .sorter {
      order: -1;
    }
  }
}
</style>
