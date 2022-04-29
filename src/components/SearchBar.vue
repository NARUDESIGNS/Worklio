<template>
  <div :class="$style['search-bar']">
    <input type="text" :class="$style['searchBar__input']" />
    <section :class="$style['buttons']">
      <Cancel :class="$style['buttons__button--cancel']" />
      <Add :class="$style['buttons__button--add']" />
    </section>
  </div>
</template>

<script lang="ts">
import Cancel from "./Cancel.vue";
import Add from "./Add.vue";

export default {
  name: "SearchBar",
  components: { Cancel, Add },
  props: [],
  // setup() {},
};
</script>

<style lang="scss" module>
@use "sass/color";
.search-bar {
  background: color.$search-bar;
  height: 70px;
  width: 500px;
  border: none;
}
</style>
