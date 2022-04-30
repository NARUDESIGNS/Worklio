<template>
  <div :class="$style['search-bar']">
    <input
      type="text"
      :class="$style['search-bar__input']"
      placeholder="Search or Add..."
    />
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
  props: {},
  // setup() {

  // },
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

    & .can-add {
      color: color.$green;
    }
  }
}

@media only screen and (max-width: 600px) {
}
</style>
