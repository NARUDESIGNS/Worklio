<template>
  <div :class="$style.app">
    <main :class="$style.main">
      <SearchBar
        @updatedQuery="(query) => updateSearchQuery(query)"
        @addItem="(item) => addNewItem(item)"
        @clearQuery="(query) => updateSearchQuery(query)"
        :foundMatch="foundMatch"
      />
      <div :class="$style['list-container']">
        <List
          v-for="item in computedSearch"
          :key="item.id"
          :data="item"
          :foundMatch="foundMatch"
          @deleteItem="(prop) => deleteItem(prop)"
        />
      </div>
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
import { ref, defineComponent, computed, watch, onUpdated } from "vue";
import SearchBar from "./components/SearchBar.vue";
import List from "./components/List.vue";
import Sort from "./components/Sort.vue";
import { capitalize } from "lodash";

export default defineComponent({
  name: "App",
  components: { SearchBar, List, Sort },
  setup() {
    // List items
    const count = ref(0);
    const date = new Date().getMinutes();

    const listItem = ref([
      { name: "John Smith", id: ++count.value, time: new Date().getMinutes() - date },
      { name: "Aria Blaze", id: ++count.value, time: new Date().getMinutes() - date },
      { name: "Rias Gremory", id: ++count.value, time: new Date().getMinutes() - date },
    ]);

    //load items from local storage
    if (localStorage.getItem("list")) {
      let loadedList = localStorage.getItem("list");
      // listItem.value = JSON.parse(loadedList);
    }

    // sort list
    let selected = ref("Value");
    const sortList = (value: string): void => {
      selected.value = value;
    };

    // delete item from list
    let itemToDelete = ref(0);
    const deleteItem = (id: number) => {
      itemToDelete.value = id;
      foundMatch.value = false;
      listItem.value = listItem.value.filter((item) => item.id !== id);
    };

    // search query
    const searchQuery = ref("");
    const updateSearchQuery = (query: string) => {
      searchQuery.value = query;
      // find exact match
      computedSearch.value.map((item) => {
        foundMatch.value = searchQuery.value.toLowerCase() === item.name.toLowerCase();
      });
    };

    // check for partial match
    let foundMatch = ref(false);
    let computedSearch = computed(() => {
      return listItem.value.filter((item) => {
        return item.name.toLowerCase().includes(searchQuery.value.toLowerCase());
      });
    });

    // add new item
    const addNewItem = (item: string) => {
      // capitaize initials of names
      item = item
        .split(" ")
        .map((item) => capitalize(item))
        .join(" ");

      listItem.value.push({
        name: item,
        id: ++count.value,
        time: new Date().getMinutes() - date,
      });

      foundMatch.value = true;
    };

    // save to local storage
    onUpdated(() => {
      localStorage.setItem("list", JSON.stringify(listItem.value));
    });

    return {
      selected,
      sortList,
      searchQuery,
      deleteItem,
      listItem,
      updateSearchQuery,
      foundMatch,
      computedSearch,
      addNewItem,
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

  & .list-container {
    height: 70vh;
    overflow-y: scroll;
  }

  & .sorter {
    align-self: flex-start;
    margin-top: 15%;
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
