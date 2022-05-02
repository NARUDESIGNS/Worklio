<template>
  <div :class="$style['list']">
    <section :class="$style['info-container']">
      <Tick v-if="foundMatch" :class="$style['info-container__check']" />
      <div :class="$style['info']">
        <p :class="$style['info__header']">{{ data.name }}</p>
        <p :class="$style['info__id']">
          <span v-if="foundMatch" :class="$style['match']">Exact match, </span> #{{ data.id }}
        </p>
      </div>
    </section>
    <section :class="$style['details']">
      <p :class="$style['details__time']">{{ data.time }} minutes ago</p>
      <Trash @click="deleteItem" :class="$style['details__trash']" />
    </section>
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import Trash from "./Trash.vue";
import Tick from "./Tick.vue";

export default defineComponent({
  name: "List",
  components: { Tick, Trash },
  props: {
    data: {
      name: String,
      id: Number,
      time: Number
    }, 
    foundMatch: Boolean
  },
  setup(props, context) {
    const deleteItem = () => {
      context.emit("deleteItem", props.data.id);
    };

    return { deleteItem };
  },
});
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
  cursor: pointer;
  font-size: 14px;
  border-bottom: 1px solid rgb(231, 231, 231);

  &:hover {
    border: none;
    border-radius: 6px;
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

    & .info__header{
      text-transform: capitalize;
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
