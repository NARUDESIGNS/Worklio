<template>
  <div :class="$style['search-bar']">
    <input
      @input="loadButtons"
      v-model="searchQuery"
      type="text"
      :class="$style['search-bar__input']"
      placeholder="Search or Add..."
    />
    <section v-if="showButtons" :class="$style['buttons']">
      <Cancel @click="clearSearchQuery" :class="$style['buttons__button--cancel']" />
      <Add
        @click="addNewItem"
        :class="!foundMatch ? $style['buttons__can-add'] : $style['buttons__button--add']"
      />
    </section>
  </div>
</template>

<script lang="ts">
import { ref } from "vue";
import Cancel from "./Cancel.vue";
import Add from "./Add.vue";

export default {
  name: "SearchBar",
  components: { Cancel, Add },
  props: { foundMatch: Boolean },
  setup(props: any, context: any) {
    let searchQuery = ref();
    let showButtons = ref(false);

    // hide or show input buttons
    const loadButtons = (): void => {
      // disallow searchQuery from starting with a space
      searchQuery.value = searchQuery.value.startsWith(" ")
        ? searchQuery.value.trim()
        : searchQuery.value;

      showButtons.value = searchQuery.value !== "";
      context.emit("updatedQuery", searchQuery.value);
    };

    // clear search query
    const clearSearchQuery = () => {
      searchQuery.value = "";
      showButtons.value = false;
    };

    return { searchQuery, showButtons, loadButtons, clearSearchQuery };
  },
};
</script>

<style lang="scss" module>
@use "sass/color";
// layout template
@mixin flex($align, $justify, $gap: 10px) {
  display: flex;
  align-items: $align;
  justify-content: $justify;
  gap: $gap;
}

.search-bar {
  @include flex(center, space-around);
  width: 100%;
  position: relative;
  margin-bottom: 30px;

  &__input {
    background: color.$search-bar;
    height: 60px;
    width: 100%;
    border: none;
    border-radius: 6px;
    padding: 20px;
    padding-right: 100px;
    font-size: 14px;
    outline: none;

    &:focus {
      border: 2px solid color.$mid-grey;
    }
  }

  .buttons {
    @include flex(center, space-between, 13px);
    position: absolute;
    top: 50%;
    right: 20px;
    transform: translateY(-50%);
    width: 75px;
    cursor: pointer;

    &__button--cancel path {
      color: color.$red;
    }

    &__button--add path {
      color: color.$mid-grey;
    }

    &__can-add {
      color: color.$teal;
    }
  }
}
</style>
