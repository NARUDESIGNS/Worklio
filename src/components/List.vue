<template>
  <div class="list">
    <Tick class="list__check" />
    <section class="info">
      <p class="info__header">Search query exact match</p>
      <p class="info__id"><span class="match">Exact match, </span>#3</p>
    </section>
    <section class="details">
      <p class="details__time">2 minutes ago</p>
      <Trash class="details__trash" />
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

<style>
/* .list {
  background-color: green;
} */
</style>
