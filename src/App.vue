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
          v-for="item in computedList"
          :key="item.id"
          :data="item"
          :foundMatch="foundMatch"
          @deleteItem="(id) => deleteItem(id)"
        />
      </div>
    </main>
    <section :class="$style.sorter">
      <Sort value="Value" :isActive="sortBy === 'value'" @click="sortList('value')" />
      <Sort value="Added Date" :isActive="sortBy === 'date'" @click="sortList('date')" />
    </section>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent, computed, watch, onUpdated, onMounted } from "vue";
import { capitalize } from "lodash";
import SearchBar from "./components/SearchBar.vue";
import List from "./components/List.vue";
import Sort from "./components/Sort.vue";

export default defineComponent({
  name: "App",
  components: { SearchBar, List, Sort },
  setup() {
    // types
    type ListType = {
      name: string;
      id: number;
      time: number;
    };

    // List items
    const count = ref(0);

    const listItem = ref([
      { name: "John Smith", id: ++count.value, time: 2 },
      { name: "Aria Blaze", id: ++count.value, time: 6 },
      { name: "Rias Gremory", id: ++count.value, time: 1 },
    ]);

    // sort list
    let sortBy = ref("value");
    const sortList = (value: string): void => {
      sortBy.value = value;
      if (sortBy.value === "date") {
        listItem.value.sort((a, b) => a.time - b.time);
      } else {
        listItem.value.sort((a, b) => (a.name > b.name ? 1 : -1));
      }
    };

    // delete item from list
    const deleteItem = (id: number) => {
      const itemToDelete = listItem.value.findIndex((item) => item.id === id);
      listItem.value.splice(itemToDelete, 1);
    };

    // process search query
    const searchQuery = ref("");
    const updateSearchQuery = (query: string) => {
      searchQuery.value = query;
      // find exact match
      for (let item of computedList.value) {
        foundMatch.value = searchQuery.value.toLowerCase() === item.name.toLowerCase();
      }
    };

    // check for partial match
    let foundMatch = ref(false);
    const computedList = computed(() => {
      return listItem.value.filter((item) => {
        return item.name.toLowerCase().includes(searchQuery.value.toLowerCase());
      });
    });

    // add new item
    const startTime = new Date().getMinutes();
    const addNewItem = (item: string) => {
      // capitaize initials of names
      item = item
        .split(" ")
        .map((item) => capitalize(item))
        .join(" ");

      listItem.value.push({
        name: item.trim(),
        id: ++count.value,
        time: new Date().getMinutes() - startTime,
      });
      foundMatch.value = true;
    };

    // preserve sort option
    watch(computedList, () => {
      sortList(sortBy.value);
    });

    // save to local storage
    onUpdated(() => {
      localStorage.setItem("list", JSON.stringify(listItem.value));
      localStorage.setItem("sortOption", sortBy.value);
    });

    //load items from local storage
    onMounted(() => {
      // retrieve list items
      if (localStorage.list) {
        const loadedList: ListType[] = JSON.parse(localStorage.list);
        listItem.value = loadedList;
      }

      // retrieve sort option
      if (localStorage.sortOption) sortBy.value = localStorage.sortOption;
    });

    return {
      sortBy,
      sortList,
      searchQuery,
      deleteItem,
      updateSearchQuery,
      foundMatch,
      computedList,
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
