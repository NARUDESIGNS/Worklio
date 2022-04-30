<template>
  <div :class="$style.app">
    <main :class="$style.main">
      <SearchBar
        @updatedQuery="(query) => updateSearchQuery(query)"
        :foundMatch="foundMatch"
      />
      <List
        v-for="item in listItem"
        :key="item.id"
        :data="item"
        :foundMatch="foundMatch"
        @deleteItem="(prop) => deleteItem(prop)"
      />
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
import { ref, defineComponent, computed } from "vue";
import SearchBar from "./components/SearchBar.vue";
import List from "./components/List.vue";
import Sort from "./components/Sort.vue";

export default defineComponent({
  name: "App",
  components: { SearchBar, List, Sort },
  setup() {
    // search query
    const searchQuery = ref("");
    const updateSearchQuery = (query: string) => {
      searchQuery.value = query;
      console.log(query);
    };

    // check for match
    let foundMatch = ref(false);

    // List items
    const count = ref(0);

    const listItem = ref([
      { name: "John Smith", id: ++count.value, time: new Date().getDate() },
      { name: "Aria Blaze", id: ++count.value, time: new Date().getDate() },
      { name: "Rias Gremory", id: ++count.value, time: new Date().getDate() },
    ]);

    // sort list
    let selected = ref("Value");
    const sortList = (value: string): void => {
      selected.value = value;
    };

    // delete item from list
    const deleteItem = (id: number) => {
      console.log(id);
      count.value = 0;
      console.log(count.value);
      listItem.value = listItem.value.filter((item) => item.id !== id);
    };

    // const computedList = computed(() => {
    //   listItem.value = listItem.value.filter((item) => item.id !== itemToDelete.value);
    //   return listItem.value;
    // });

    return {
      selected,
      sortList,
      searchQuery,
      deleteItem,
      listItem,
      updateSearchQuery,
      foundMatch,
    };
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
