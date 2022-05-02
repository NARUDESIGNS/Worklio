<template>
  <div :class="[$style['sort'], isActive ? $style.isActive : null]">
    <p :class="$style['sort__header']">
      Sort by <span :class="$style.sorter"> {{ value }} </span>
    </p>
    <span v-if="isActive" :class="$style['sort__indicator']"></span>
  </div>
</template>

<script lang="ts">
export default {
  name: "Sort",
  props: { value: String, isActive: Boolean },
};
</script>

<style lang="scss" module>
@use 'sass/color';

.sort {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 30px;
  flex: 1;
  width: 200px;
  color: color.$mid-grey;
  padding: 11px 15px;
  //   background-color: color.$white;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  margin-bottom: 5px;

  &__header {
    margin: 0;
    color: color.$id-text;

    & .sorter {
      color: color.$text;
    }
  }

  &__indicator {
    height: 5px;
    width: 5px;
    border-radius: 50%;
    background-color: color.$green;
  }
}

.isActive {
  background-color: color.$white;
}
</style>
