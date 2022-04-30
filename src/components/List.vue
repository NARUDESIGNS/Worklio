<template>
  <div :class="$style['list']">
    <section :class="$style['info-container']">
      <Tick :class="$style['info-container__check']" />
      <div :class="$style['info']">
        <p :class="$style['info__header']">Search query exact match</p>
        <p :class="$style['info__id']">
          <span :class="$style['match']">Exact match, </span>#3
        </p>
      </div>
    </section>
    <section :class="$style['details']">
      <p :class="$style['details__time']">2 minutes ago</p>
      <Trash :class="$style['details__trash']" />
    </section>
  </div>
</template>

<script>
import Trash from "./Trash.vue";
import Tick from "./Tick.vue";
export default {
  name: "List",
  components: { Tick, Trash },
  props: [],
};
</script>

<style lang="scss" module>
@use 'sass/color';
// layout template
@mixin flex($align, $justify, $gap: 10px) {
  display: flex;
  align-items: $align;
  justify-content: $justify;
  gap: $gap;
}

.list {
  @include flex(center, space-between);
  width: 100%;
  padding: 15px 20px;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;

  &:hover {
    background-color: color.$white;
    box-shadow: 0 0 40px color.$shadow;
  }
  &:hover .details__trash {
    display: block;
  }

  .info-container {
    @include flex(center, space-between, 25px);

    &__check,
    & .match {
      color: color.$green;
    }

    & .info__header,
    .info__id {
      margin: 0px;
    }

    & .info__id {
      font-size: 12px;
      color: color.$id-text;
    }
  }

  .details {
    @include flex(center, space-between);
    font-size: 13px;

    &__trash {
      color: color.$red;
      margin-left: 10px;
      display: none;
    }
  }
}
</style>
